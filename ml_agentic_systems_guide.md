# ML DOMAIN INTERVIEW: PRODUCTION-GRADE AGENTIC SYSTEMS
## Google Forward Deployed Engineer - Applied AI
## Quick Reference Guide

## Related Obsidian Links

- [[README]]
- [[vibe_coding_mastery_guide]]
- [[python_dsa_patterns_explained]]

---

# THE 5 PILLARS

## 1. Modular Orchestration

**Core Pattern:**
```
User Query → Router/Orchestrator → Specialized Agent → Tools → Response
```

**Key Points:**
- Central router classifies intent and delegates to specialist agents
- Each agent has SINGLE responsibility (separation of concerns)
- Agents are composable — can call other agents if needed
- Tools are deterministic functions agents can invoke (APIs, DBs, calculators)

**Architecture Example:**
```
┌─────────────────────────────────────────┐
│              Orchestrator               │
│  (classifies intent, routes, manages)   │
└──────────────┬──────────────────────────┘
               │
    ┌──────────┼──────────┐
    ▼          ▼          ▼
┌───────┐ ┌────────┐ ┌─────────┐
│Search │ │ Code   │ │ Summary │
│ Agent │ │ Agent  │ │  Agent  │
└───┬───┘ └───┬────┘ └────┬────┘
    │         │           │
    ▼         ▼           ▼
┌───────┐ ┌────────┐ ┌─────────┐
│Google │ │Python  │ │  LLM    │
│  API  │ │Sandbox │ │  Call   │
└───────┘ └────────┘ └─────────┘
```

**Frameworks:**
- LangGraph — Graph-based agent orchestration
- CrewAI — Role-based multi-agent framework
- AutoGen — Microsoft's multi-agent conversation framework
- LangChain — General-purpose LLM orchestration

**What to Say:**
> "I'd design this with a central orchestrator that maintains conversation state and routes
> to specialized agents. Each agent has a focused capability — one for retrieval, one for
> code execution, one for summarization. This modularity makes testing easier and allows
> us to swap or upgrade individual agents without touching others."

---

## 2. Deterministic Reliability

**Problem:** LLMs are probabilistic — same input can give different outputs.

**Solutions:**

### Structured Outputs
```python
from pydantic import BaseModel

class ToolCall(BaseModel):
    tool_name: str
    parameters: dict
    confidence: float  # 0.0 - 1.0

class AgentResponse(BaseModel):
    thought: str
    action: ToolCall | None
    final_answer: str | None

# Force LLM to return valid schema
response = llm.generate(prompt, response_format=AgentResponse)
```

### Retry Logic
```python
from tenacity import retry, stop_after_attempt, wait_exponential

@retry(
    stop=stop_after_attempt(3),
    wait=wait_exponential(multiplier=1, min=2, max=10)
)
def call_external_api(query: str):
    return api.search(query)
```

### Validation & Fallbacks
```python
def execute_tool(tool_call: ToolCall):
    try:
        result = tools[tool_call.tool_name].run(tool_call.parameters)
        if not validate_result(result):
            raise InvalidResultError()
        return result
    except ToolNotFoundError:
        return "I don't have access to that tool. Let me try another approach."
    except TimeoutError:
        return "The service is slow. Here's what I know from cached data..."
    except Exception as e:
        log_error(e)
        return "I encountered an issue. Could you rephrase your request?"
```

**What to Say:**
> "For reliability, I enforce strict schemas using Pydantic for all LLM outputs. If the
> response doesn't parse, we retry with a clarified prompt. For tool calls, we wrap everything
> in retry logic with exponential backoff, and we have graceful fallbacks when things fail.
> The system should never crash — it should degrade gracefully and communicate clearly."

---

## 3. Context Optimization

**Problem:** LLMs have limited context windows. Long conversations or large documents don't fit.

**Solutions:**

### Memory Compression
```python
class ConversationMemory:
    def __init__(self, max_tokens=4000):
        self.messages = []
        self.summary = ""
        self.max_tokens = max_tokens
    
    def add_message(self, role, content):
        self.messages.append({"role": role, "content": content})
        
        if self.token_count() > self.max_tokens:
            self._compress()
    
    def _compress(self):
        # Keep last 5 messages, summarize the rest
        old_messages = self.messages[:-5]
        self.summary = llm.summarize(self.summary + str(old_messages))
        self.messages = self.messages[-5:]
    
    def get_context(self):
        return f"Previous context: {self.summary}\n\nRecent messages: {self.messages}"
```

### RAG (Retrieval Augmented Generation)
```python
# Indexing
def index_documents(docs: list[str]):
    embeddings = embedding_model.encode(docs)
    vector_db.upsert(embeddings, docs)

# Retrieval
def retrieve_context(query: str, top_k=5):
    query_embedding = embedding_model.encode(query)
    results = vector_db.search(query_embedding, top_k=top_k)
    return results

# Generation
def generate_response(query: str):
    context = retrieve_context(query)
    prompt = f"Context: {context}\n\nQuestion: {query}"
    return llm.generate(prompt)
```

**Vector DBs to Know:**
- Pinecone — Managed, easy to scale
- Weaviate — Open source, GraphQL API
- Chroma — Lightweight, good for prototyping
- FAISS — Facebook's library, great for local/fast search
- Qdrant — Rust-based, performant

**What to Say:**
> "For context optimization, I use a tiered approach. Recent messages stay in full,
> older conversation gets progressively summarized. For knowledge bases, I use RAG —
> embed documents into a vector DB like Pinecone, then retrieve only the top-k most
> relevant chunks for each query. This keeps context focused and costs down."

---

## 4. Operational Guardrails

**Problem:** Agents can loop infinitely, take dangerous actions, or burn through API costs.

**Solutions:**

### Circuit Breakers
```python
class AgentRunner:
    def __init__(self, max_iterations=10, max_tokens=10000, timeout_seconds=60):
        self.max_iterations = max_iterations
        self.max_tokens = max_tokens
        self.timeout = timeout_seconds
    
    def run(self, task: str):
        iterations = 0
        tokens_used = 0
        start_time = time.time()
        
        while not task_complete:
            # Circuit breaker checks
            if iterations >= self.max_iterations:
                return "Reached maximum iterations. Here's my best answer so far..."
            
            if tokens_used >= self.max_tokens:
                return "Reached token budget. Summarizing what I found..."
            
            if time.time() - start_time > self.timeout:
                return "Taking too long. Here's a partial response..."
            
            # Execute step
            result, tokens = self.execute_step()
            tokens_used += tokens
            iterations += 1
```

### Human-in-the-Loop
```python
DANGEROUS_ACTIONS = ["delete", "purchase", "send_email", "execute_code", "modify_production"]

def execute_action(action: Action):
    if action.type in DANGEROUS_ACTIONS:
        # Pause and ask for confirmation
        approved = request_human_approval(
            action=action,
            reason="This action has side effects",
            timeout_minutes=5
        )
        if not approved:
            return "Action cancelled by user."
    
    return action.execute()
```

### Cost Controls
```python
class CostTracker:
    def __init__(self, budget_per_session=1.00):  # $1 max
        self.budget = budget_per_session
        self.spent = 0.0
    
    def track_call(self, model: str, input_tokens: int, output_tokens: int):
        cost = calculate_cost(model, input_tokens, output_tokens)
        self.spent += cost
        
        if self.spent >= self.budget:
            raise BudgetExceededError(f"Session budget of ${self.budget} exceeded")
```

**What to Say:**
> "Production agents need strict guardrails. I implement circuit breakers that cap iterations,
> tokens, and wall-clock time. Any destructive action — deletes, purchases, external sends —
> requires explicit human approval. We also track costs per session and per user to prevent
> runaway spending. The agent should be safe to leave running without supervision."

---

## 5. Systematic Evaluation

**Problem:** "Vibe checks" don't scale. Need automated, reproducible quality measurement.

**Solutions:**

### Golden Datasets
```python
GOLDEN_TESTS = [
    {
        "input": "What's the capital of France?",
        "expected": "Paris",
        "criteria": ["factually_correct", "concise"]
    },
    {
        "input": "Summarize this 10-page document",
        "expected_criteria": ["captures_main_points", "under_500_words", "no_hallucinations"],
        "reference_doc": "doc_123.pdf"
    },
]

def run_golden_tests():
    results = []
    for test in GOLDEN_TESTS:
        output = agent.run(test["input"])
        score = evaluate(output, test)
        results.append({"test": test["input"], "score": score, "output": output})
    return results
```

### LLM-as-Judge
```python
JUDGE_PROMPT = """
Rate the following response on these criteria (1-5 each):
1. Relevance: Does it answer the question?
2. Accuracy: Is the information correct?
3. Clarity: Is it easy to understand?
4. Completeness: Does it fully address the query?

Question: {question}
Response: {response}

Return JSON: {"relevance": X, "accuracy": X, "clarity": X, "completeness": X, "reasoning": "..."}
"""

def llm_judge(question: str, response: str) -> dict:
    judgment = gpt4.generate(JUDGE_PROMPT.format(question=question, response=response))
    return json.loads(judgment)
```

### Evaluation Pipeline
```python
def nightly_eval_pipeline():
    # 1. Run all golden tests
    results = run_golden_tests()
    
    # 2. Calculate aggregate metrics
    metrics = {
        "success_rate": sum(r["passed"] for r in results) / len(results),
        "avg_latency": mean(r["latency"] for r in results),
        "avg_cost": mean(r["cost"] for r in results),
        "avg_judge_score": mean(r["judge_score"] for r in results),
    }
    
    # 3. Compare to baseline
    regression = detect_regression(metrics, previous_metrics)
    
    # 4. Alert if needed
    if regression:
        send_alert(f"Regression detected: {regression}")
    
    # 5. Log to dashboard
    log_metrics(metrics)
```

**Key Metrics:**
- Task success rate (binary: did it complete the task?)
- Factual accuracy (via ground truth comparison or LLM judge)
- Latency (p50, p95, p99)
- Cost per query
- User satisfaction (thumbs up/down, explicit ratings)

**What to Say:**
> "We move beyond vibe checks with systematic evaluation. We maintain golden datasets —
> curated test cases with expected outputs. Every PR runs against these. For subjective
> quality, we use LLM-as-judge with GPT-4 scoring on relevance, accuracy, and completeness.
> Nightly pipelines track metrics over time and alert on regressions. This catches issues
> before users do."

---

# PUTTING IT ALL TOGETHER

**Sample System Design Answer:**

"If I were building a customer support agent for an e-commerce platform, here's how I'd approach it:

**Orchestration:** Central router classifies tickets into categories — order status, returns, product questions, complaints. Each routes to a specialized agent with access to relevant tools (order DB, return policy docs, product catalog).

**Reliability:** All agent outputs use Pydantic schemas. Tool calls have retry logic with exponential backoff. If the order API is down, we gracefully tell the user we're checking and will follow up.

**Context:** Customer history is stored in a vector DB. When a ticket comes in, we retrieve relevant past interactions and order history. Long conversations get summarized every 10 turns.

**Guardrails:** Refund approvals over $100 require human review. Agents can't access other customers' data. We cap each session at 20 iterations and $0.50 in API costs.

**Evaluation:** We have 500 golden tickets with labeled correct resolutions. Nightly evals measure resolution accuracy, customer satisfaction scores, and average handle time. Any regression >3% blocks deployment."

---

# QUICK REFERENCE: TOOLS & FRAMEWORKS

| Category | Tools |
|----------|-------|
| Orchestration | LangGraph, LangChain, CrewAI, AutoGen |
| Vector DBs | Pinecone, Weaviate, Chroma, FAISS, Qdrant |
| Structured Output | Pydantic, Instructor, OpenAI function calling |
| Observability | LangSmith, Weights & Biases, Arize |
| Evaluation | RAGAS, DeepEval, custom LLM-as-judge |

---

# QUESTIONS THEY MIGHT ASK

## Quick-Fire (one-liners)

1. "How would you handle an agent that keeps looping?"
   → Circuit breakers, iteration limits, loop detection

2. "How do you ensure the agent doesn't hallucinate?"
   → RAG with citations, confidence scores, fact-checking tools

3. "How would you evaluate if your agent is improving?"
   → Golden datasets, LLM-as-judge, A/B testing, user feedback metrics

4. "What happens when an external API fails?"
   → Retry with backoff, fallback to cached data, graceful error messages

5. "How do you manage costs at scale?"
   → Token budgets per session, caching frequent queries, smaller models for simple tasks

---

## Deep-Dive (full answers)

### 6. "Walk me through how you'd debug an agent that gives a wrong answer."
> "First I make it reproducible — capture the exact input, the full trace, and the seed/temperature.
> Then I look at the trace as a pipeline and isolate the failing stage: Was retrieval wrong (bad
> chunks)? Was the prompt wrong (missing context)? Was the tool call malformed? Or did the model
> reason poorly over correct inputs? Each has a different fix — re-ranking and better chunking for
> retrieval, schema enforcement for tool calls, few-shot examples or a stronger model for reasoning.
> The key is that observability (LangSmith, tracing every step) lets me localize the failure instead
> of guessing. I treat it like any production bug: reproduce, isolate, fix, add a regression test to
> the golden set."

### 7. "How do you decide between a single large prompt vs. a multi-agent system?"
> "Start simple. A single prompt with tools handles most tasks and is far easier to debug, cheaper,
> and lower-latency. I only reach for multi-agent when there's a real need: distinct skill domains
> that benefit from separate system prompts, parallelizable subtasks, or context that's too large to
> share. Multi-agent adds coordination overhead, more failure modes, and higher cost — so the bar is
> 'does decomposition measurably improve quality or latency?' Premature multi-agent design is the
> agentic equivalent of premature microservices."

### 8. "How do you handle prompt injection and untrusted tool output?"
> "I treat all retrieved content and tool outputs as untrusted data, never as instructions. Concretely:
> keep system instructions separate from user/tool content, never concatenate raw documents into the
> instruction layer, and use structured boundaries so the model knows 'this is data to analyze, not
> commands to follow.' For high-risk tools (sending email, executing code, DB writes), I gate behind
> allowlists and human approval regardless of what the model 'decided.' I also sanitize and validate
> tool outputs before they re-enter context. The principle: the model's privileges should never escalate
> just because some text told it to."

### 9. "Your RAG system retrieves the right docs but the answer is still wrong. What's happening?"
> "That's a generation problem, not a retrieval problem — good signal that the bottleneck is downstream.
> Common causes: the context is there but buried (lost-in-the-middle — reorder so key chunks are at the
> start/end), the chunks are too fragmented to reason over (increase chunk size or add overlap), the
> prompt doesn't instruct the model to ground its answer and cite, or the model is too weak. I'd add an
> instruction to answer *only* from context and say 'I don't know' otherwise, require inline citations so
> I can verify grounding, and measure faithfulness with RAGAS. If it still fails, it's a model capability
> issue — upgrade the model for that step."

### 10. "How would you reduce latency in an agent that feels too slow?"
> "I profile first to find where the time goes — usually it's sequential LLM calls and tool round-trips.
> Levers, roughly in order: (1) parallelize independent tool calls and retrievals instead of running them
> serially; (2) stream tokens so the user sees output immediately even if the full answer takes longer;
> (3) use a smaller/faster model for routing and simple steps, reserving the big model for hard reasoning
> (model cascading); (4) cache embeddings and frequent query results; (5) cut unnecessary agent loops with
> tighter prompts. Perceived latency matters as much as actual — streaming a partial answer in 500ms beats
> a complete answer in 5s with a blank screen."

### 11. "How do you measure success for an agentic feature with no single 'correct' answer?"
> "I decompose subjective quality into measurable proxies and combine offline and online signals. Offline:
> LLM-as-judge scoring on rubric dimensions (relevance, faithfulness, completeness), plus a golden set with
> human-labeled references. Online: task completion rate, follow-up/clarification rate (high = confusing),
> thumbs up/down, and escalation-to-human rate. No single metric is trustworthy, so I triangulate. And I
> always keep a small human-eval loop to calibrate that the LLM judge correlates with real user judgment —
> otherwise you optimize a judge that's drifted from reality."

### 12. "A customer says the agent 'works in the demo but fails in production.' How do you approach it?"
> "This is the FDE core problem — the gap is almost always distribution shift between demo inputs and real
> ones. I'd instrument production to capture real failing traces, then cluster them: are they edge-case
> intents the demo never covered, longer/messier inputs, missing data in their actual systems, or latency/
> timeout issues under real load? Demos are curated; production is adversarial and long-tailed. I'd build a
> golden set *from real production failures*, not synthetic cases, and iterate against that. The fix is
> usually expanding coverage and hardening guardrails, not changing the core model."

### 13. "How do you keep an agent's behavior stable when you change the prompt or model?"
> "Version everything — prompts, model IDs, and tool schemas are config, not loose strings. Before any
> change ships, it runs against the full golden set and I diff the metrics; a regression beyond threshold
> blocks the change. For model upgrades I A/B test on live traffic with a fraction of users before full
> rollout. The mindset is that prompts and models are dependencies like any library — you don't upgrade
> them in production without a regression suite and a rollback path."

---

# YOUR EXPERIENCE TO HIGHLIGHT

Based on your past projects (Prophet forecasting, clinical dashboards, data pipelines):

- "In my forecasting pipeline, I built similar reliability patterns — retry logic, 
   validation checks, graceful degradation when data sources were unavailable."

- "I understand production systems. I've built pipelines that need to run reliably
   without human intervention, with proper error handling and monitoring."

- "I've worked with structured data and schemas extensively, which translates directly
   to enforcing structured outputs from LLMs."

Connect your experience to these pillars. You have more relevant background than you might think.
