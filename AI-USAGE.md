# AI Usage Transparency

## AI Tools Used
- **ChatGPT (OpenAI)** – primary assistant for reasoning, debugging, and structuring the solution
- **Grok (xAI)** – secondary reference for validating Shopify/Dawn behaviors and alternative approaches

AI usage was intentional, iterative, and reviewed at every step.

---

## High-Level Prompts Used
- “How to implement a delivery note using cart attributes in Shopify Dawn?”
- “Why does a custom cart feature disappear after quantity change in Dawn?”
- “How does Dawn handle asynchronous cart updates?”
- “What are safe ways to react to cart changes without apps?”

These prompts were used to explore approaches, not to blindly copy solutions.

---

## What AI Suggested
AI tools suggested:
- Using **cart attributes** for persistence
- Fetching `/cart.js` to determine cart state
- Debouncing user input to reduce requests
- Intercepting `fetch` requests to detect cart updates (initial idea)

---

## What Was Modified or Rejected

### Rejected: Blind Fetch Interception
Intercepting `window.fetch` caused:
- Recursive cart reads
- Request conflicts (409)
- Rate limiting (429)
- Broken Add to Cart behavior

This approach was rejected as unstable for Dawn themes.

### Accepted with Modification
- Cart state is re-evaluated **only after user-driven DOM events**
- Cart reads are passive and non-recursive
- Visibility logic is decoupled from cart mutations

---

## Where Human Judgment Was Required
- Choosing cart attributes over line item properties
- Identifying unsafe AI-suggested patterns
- Debugging race conditions via browser devtools
- Designing a solution compatible with Dawn’s real-world behavior

AI accelerated exploration, but final decisions and fixes were human-driven.

---

## Summary
ChatGPT and Grok were used as thinking aids and accelerators.  
The final implementation reflects Shopify best practices and manual validation rather than a copy-pasted AI solution.
