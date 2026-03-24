# Tutorial: How an AI Uses Ix (Step by Step)

This walkthrough shows exactly how an LLM assistant uses Ix to understand a codebase — and why it saves thousands of tokens compared to reading files directly.

## The Scenario

You open Claude Code in a new project for the first time. The project is a payment processing system with 200+ files. You ask: *"How does payment processing work?"*

Without Ix, Claude would need to `Grep` for "payment", read dozens of files, and piece things together. That's thousands of lines of code burned as context tokens.

With Ix, here's what happens:

---

## Step 1: Map the codebase

```sh
ix map ./src
```

**What this does:**
1. Walks every file in `./src`
2. Parses each file with Tree-Sitter (TypeScript, Python, Java, etc.)
3. Extracts every class, function, method, import, and call relationship
4. Builds a versioned knowledge graph in ArangoDB
5. Runs community detection to identify architectural regions

**Output:**
```
System: payment-core (confidence: 0.94)
  ├── src/payments/processor.ts
  ├── src/payments/validator.ts
  ├── src/payments/gateway.ts
  └── src/payments/types.ts

System: auth (confidence: 0.91)
  ├── src/auth/provider.ts
  ├── src/auth/middleware.ts
  └── src/auth/tokens.ts

System: api-routes (confidence: 0.88)
  ├── src/routes/payments.ts
  ├── src/routes/users.ts
  └── src/routes/webhooks.ts
```

**Tokens saved:** Instead of reading 200 files (~50,000 tokens), the AI now knows the system structure in ~200 tokens.

---

## Step 2: Find the entry point

```sh
ix search PaymentProcessor --kind class
```

**Output:**
```
PaymentProcessor (class) — src/payments/processor.ts:12-89  [score: 100]
PaymentProcessorTest (class) — test/payments/processor.test.ts:5-120  [score: 60]
```

**Tokens saved:** One targeted result instead of grepping through every file and reading matches.

---

## Step 3: Understand the class

```sh
ix explain PaymentProcessor
```

**Output:**
```
PaymentProcessor (class) — src/payments/processor.ts:12-89

  Role: Core payment orchestrator
  Importance: High (8 dependents)

  Contains:
    - processPayment (method) :24-45
    - validateCard (method) :47-62
    - chargeGateway (method) :64-78
    - handleWebhook (method) :80-88

  Used by:
    - PaymentRoute.createPayment
    - WebhookHandler.onPaymentEvent
    - RefundService.initiateRefund

  Why it matters:
    Central to payment flow — 8 downstream consumers depend on this.
```

**Tokens saved:** Full structural understanding in ~150 tokens. Without Ix, the AI would read the entire file (~500 tokens) plus grep for usages across the codebase (~2000+ tokens).

---

## Step 4: See what it contains

```sh
ix overview PaymentProcessor
```

**Output:**
```
PaymentProcessor (class)
  System path: payment-core → processor

  Methods (4):
    processPayment    :24-45
    validateCard      :47-62
    chargeGateway     :64-78
    handleWebhook     :80-88

  Key items:
    processPayment — called by 5 consumers
    chargeGateway — calls external gateway
```

---

## Step 5: Check blast radius before making changes

```sh
ix impact PaymentProcessor --depth 2
```

**Output:**
```
PaymentProcessor — 8 entities at risk

  Critical (direct dependents):
    PaymentRoute.createPayment — src/routes/payments.ts:30
    WebhookHandler.onPaymentEvent — src/routes/webhooks.ts:15
    RefundService.initiateRefund — src/services/refund.ts:22

  High (one hop):
    AppRouter — src/routes/index.ts:8
    WebhookRouter — src/routes/webhooks.ts:5

  Medium (two hops):
    Server.start — src/server.ts:12
```

**Tokens saved:** The AI knows exactly what will break — no need to trace imports across files manually.

---

## Step 6: Follow a specific dependency chain

```sh
ix trace processPayment --downstream
```

**Output:**
```
processPayment (method)
  ├── validateCard (calls)
  │   └── CardValidator.validate (calls)
  ├── chargeGateway (calls)
  │   └── StripeGateway.charge (calls)
  │       └── stripe.paymentIntents.create (calls)
  └── EventEmitter.emit (calls)
      └── WebhookHandler.onPaymentEvent (calls)
```

---

## Step 7: Find the hotspots

```sh
ix rank --by dependents --kind class --top 5
```

**Output:**
```
1. PaymentProcessor    8 dependents
2. AuthProvider        6 dependents
3. DatabaseClient      5 dependents
4. UserService         4 dependents
5. Logger              4 dependents
```

These are the classes where changes have the most ripple effects.

---

## Step 8: Read specific code when needed

```sh
ix read processPayment
```

**Output:**
```
src/payments/processor.ts:24-45

  24 │ async processPayment(request: PaymentRequest): Promise<PaymentResult> {
  25 │   const validation = await this.validateCard(request.card);
  26 │   if (!validation.valid) {
  27 │     return { success: false, error: validation.error };
  28 │   }
  ...
  45 │ }
```

Only now — after understanding the architecture, dependencies, and impact — does the AI read actual source code. And it reads only the specific method it needs, not entire files.

---

## Step 9: Keep the graph current

```sh
ix watch
```

Or if using Claude Code, edited files are auto-ingested via hooks — no manual step needed.

---

## Token Savings Summary

| Task | Without Ix | With Ix |
|------|-----------|---------|
| Understand project structure | ~50,000 tokens (read files) | ~200 tokens (ix map) |
| Find a class | ~5,000 tokens (grep + read) | ~50 tokens (ix search) |
| Understand a class | ~2,500 tokens (read file + usages) | ~150 tokens (ix explain) |
| Check blast radius | ~10,000 tokens (trace imports manually) | ~200 tokens (ix impact) |
| Follow dependencies | ~8,000 tokens (read file chains) | ~100 tokens (ix trace) |
| **Total for this workflow** | **~75,000 tokens** | **~700 tokens** |

That's a **99% reduction** in context tokens for the same understanding. This means:
- More room in the context window for actual work
- Faster responses
- Better answers because the AI isn't overwhelmed with raw code
- Works across sessions — the graph persists, so session 2 starts where session 1 left off
