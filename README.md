# AI-Infrastructure-Evaluation

# Vin’s Questions (AI Infrastructure Evaluation)

## 1. How could we handle "agent got stuck" scenarios?

- Timeout на кожен крок агента (tool call / LLM call)
- Max depth / max steps (наприклад 10–20 ітерацій)
- Heartbeat / watchdog (якщо немає прогресу → kill)
- Retry policy (з обмеженням)
- Fallback: simpler model або predefined response

---

## 2. Any automatic timeout / circuit breaker patterns?

Реалізується на рівні сервісу:

- Timeout per request (LLM / tool)
- Circuit breaker (fail N разів → stop calls)
- Rate limiting + concurrency limiting
- Bulkhead isolation (окремі черги)

---

## 3. How does kgateway handle model failover?

- Primary model → fallback model
- Health checks моделей
- Retry with next provider
- Load balancing між endpoints

---

## 4. Can we automatically switch OpenAI → Claude → local?

- Абстрактний LLM layer
- Routing rules (latency / cost / errors)
- Priority list:
  1. OpenAI
  2. Claude
  3. Local (vLLM)

---

## 5. Could we seamlessly handle the response formats from these providers?

- Normalization layer
- Unified schema (text / tool_calls / json)
- Adapter per provider

---

## 6. Can we version the agents built from kagent?

- Git-based versioning
- Tagging (v1, v2)
- Config (YAML/CRD) versioning
- Immutable releases

---

## 7. Any blue/green or canary deployment patterns for agents?

- Blue/Green: два агенти (old/new)
- Canary: % трафіку на нову версію
- A/B testing
- Rollback

---

## 8. What's the fastmcp-python framework mentioned?

- Python framework для MCP серверів
- Просте створення tools
- Інтеграція з LLM
- Мінімум boilerplate

---

## 9. Is it the easiest path to MCP?

Для старту:

- Швидкий запуск
- Простий API
- Добре для MVP

Але:
- Не для складного продакшену (краще kagent + k8s)

---

## 10. About FinOps: how much control I can have?

Високий рівень контролю:

- Per request
- Per agent
- Per user

---

## 11. Token level / per agent level?

- Token tracking (input/output)
- Per-agent limits
- Per-request limits

---

## 12. Can I implement custom cost controls?

- Budget limits
- Stop execution при перевищенні
- Dynamic routing (cheap → expensive)
- Cost-aware scheduling

---

## 13. Per-agent budgets or token limits?

- Max tokens per run
- Max steps
- Budget per agent/session
- Depth control (chain length)

---

## 14. vLLM suitable for agents or single-shot inference?

vLLM:

**Добре:**
- single-shot inference
- high throughput

**Менш оптимально:**
- багато дрібних викликів (agent loops)

---

## 15. llm-d scheduler — helps when agent makes many LLM calls?

Корисний:

- Оркестрація багатьох LLM calls
- Batch / queue management
- Priority handling
- Оптимізація latency + cost

Особливо важливо коли:
- агент робить 10–20 викликів