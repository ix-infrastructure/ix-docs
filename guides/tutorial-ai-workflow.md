# Tutorial: Mapping and Understanding Your Codebase with Ix

This tutorial walks you through using Ix from scratch — mapping a project, exploring its architecture, and understanding how everything connects. Each step builds on the last, whether you're using Ix yourself from the terminal or plugging it into an LLM like Claude.

---

## Setting the Scene

You have a project. Maybe it's something you built, maybe you just joined a team and inherited it. Either way, there are hundreds of files and you need to understand how everything fits together.

Let's walk through it.

---

## Step 1: Map Your Project

Start here. Always.

```sh
cd ~/my-project
ix map ./src
```

Ix walks through every source file, parses it with Tree-Sitter, and extracts the structure — every class, function, method, import, and call relationship. Then it runs community detection to group files into systems based on how they actually connect to each other.

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

You now have a bird's-eye view of the project. Not based on folder structure — based on how the code actually couples together. Files that talk to each other end up in the same system.

**Why this matters for you:** You immediately see the major pieces of the system without opening a single file. You know where to look.

**Why this matters for an LLM:** Instead of reading hundreds of files to build a mental model, the LLM gets the architectural layout in a few lines. It can reason about the system at the right level of abstraction instead of drowning in implementation details.

---

## Step 2: Search for What You're Interested In

Now you want to dig into something specific. Say you're interested in how payments work.

```sh
ix search PaymentProcessor --kind class
```

```
PaymentProcessor (class) — src/payments/processor.ts:12-89  [score: 100]
```

Ix searches the knowledge graph — not file contents. Results are ranked by how well they match, and you can filter by kind (class, function, method, file) to cut through noise.

**Try it yourself:**
```sh
ix search validate --kind function --path src/payments/
```

This scopes the search to functions named "validate" within the payments directory. Targeted, fast, no guessing.

---

## Step 3: Understand What You Found

You found `PaymentProcessor`. What is it? What does it do? What depends on it?

```sh
ix explain PaymentProcessor
```

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
    Central to payment flow — changes here affect 8 downstream consumers.
```

Without reading a single line of code, you now know:
- What methods this class has
- Where each one lives (line numbers)
- Who uses this class
- How important it is in the system

**Why this matters for you:** You're not guessing. You know what this class does, what it contains, and what depends on it — before you open the file.

**Why this matters for an LLM:** The LLM can answer questions about the class's role and relationships without reading the source. It only reads code when it needs to reason about specific implementation logic.

---

## Step 4: See the Structure

Want a quick layout of what's inside?

```sh
ix overview PaymentProcessor
```

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

`overview` is the quick glance. `explain` is the deep dive. Use whichever fits what you need in the moment.

---

## Step 5: Check What Would Break

You're thinking about changing `PaymentProcessor`. Before you touch anything:

```sh
ix impact PaymentProcessor --depth 2
```

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

This is the blast radius. Ix traces the dependency graph outward from your target and categorizes everything by risk level — what's directly coupled, what's one hop away, what's further out.

**Why this matters for you:** You know exactly what to test after making a change. No surprises.

**Why this matters for an LLM:** When you ask "can I refactor this?", the LLM can give you a real answer — not a guess. It knows the downstream impact.

---

## Step 6: Follow the Chain

Want to see how `processPayment` flows into the rest of the system?

```sh
ix trace processPayment --downstream
```

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

This is the call tree — what `processPayment` touches when it runs. You can also go upstream (what calls it) or find the path between two symbols:

```sh
# What calls processPayment?
ix trace processPayment --upstream

# How does AuthProvider connect to DatabaseClient?
ix trace AuthProvider --to DatabaseClient
```

---

## Step 7: Find the Important Parts

Which classes are the most central to the system?

```sh
ix rank --by dependents --kind class --top 5
```

```
1. PaymentProcessor    8 dependents
2. AuthProvider        6 dependents
3. DatabaseClient      5 dependents
4. UserService         4 dependents
5. Logger              4 dependents
```

These are the load-bearing walls. Changes here ripple the farthest. If you're onboarding, these are the classes to understand first.

You can rank by different metrics:
```sh
# Most-called functions
ix rank --by callers --kind function --top 10

# Largest files (most members)
ix rank --by members --kind file --top 10
```

---

## Step 8: Read the Code

Now — and only now — you read source code. But instead of opening whole files, you read just the piece you need:

```sh
ix read processPayment
```

```
src/payments/processor.ts:24-45

  24 │ async processPayment(request: PaymentRequest): Promise<PaymentResult> {
  25 │   const validation = await this.validateCard(request.card);
  26 │   if (!validation.valid) {
  27 │     return { success: false, error: validation.error };
  28 │   }
  29 │
  30 │   const charge = await this.chargeGateway(request);
  31 │   if (!charge.success) {
  32 │     return { success: false, error: charge.error };
  33 │   }
  34 │
  35 │   this.emit('payment.completed', { id: charge.id, amount: request.amount });
  36 │   return { success: true, chargeId: charge.id };
  37 │ }
```

You already know what this method does in the system, who calls it, what it calls, and what would break if you changed it. Reading the code is the final step — confirming implementation details — not the starting point.

---

## Step 9: Keep It Current

The graph is only useful if it stays up to date. Two options:

**Watch mode** — auto-ingests on file save:
```sh
ix watch
```

**Claude Code hooks** — if you're using Claude Code, Ix hooks auto-ingest every file you edit. No manual step. The graph stays current as you work.

When you come back tomorrow, the graph is still there. You don't start over. And when your LLM starts a new session, it can pick up exactly where the last one left off — it queries the graph instead of re-reading your entire codebase.

---

## Putting It All Together

Here's the flow:

```
ix map       → See the whole system (architecture)
ix search    → Find what you're looking for
ix explain   → Understand what it is and why it matters
ix overview  → Quick structural layout
ix impact    → Know what breaks before you change it
ix trace     → Follow the dependency chain
ix rank      → Find the most important pieces
ix read      → Read only the code you actually need
ix watch     → Keep it all current
```

Each command gives you a different lens on the same codebase. Start wide with `map`, narrow down with `search` and `explain`, analyze with `impact` and `trace`, and only read code when you need the specifics.

This works the same whether you're typing commands yourself or an LLM is using them on your behalf. The graph is the shared understanding that persists across sessions, across tools, and across people working on the same project.
