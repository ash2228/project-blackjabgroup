# Smart Delivery Note – Shopify Theme Task

## Overview
This project implements a **Smart Delivery Note** feature for a Shopify development store using a **theme-only approach (Liquid + JavaScript)**. The feature allows customers to optionally add delivery instructions, with visibility and behavior tied to the cart total.

The implementation is compatible with **Dawn and Dawn-like themes** and avoids the use of apps or checkout extensions.

---

## Feature Summary
- Delivery note input shown **only when cart total exceeds INR 999**
- Available on the **product page** and visible again on the **cart page**
- Character limit of **120 characters**
- **Live character counter**
- Value persisted using **Shopify cart attributes**
- Delivery note visible in **Shopify order admin**
- Fully responsive and accessible
- Works across dynamic cart updates (add to cart, quantity changes)

---

## Why Cart Attributes Were Used
Cart attributes were chosen because they:
- Are global to the cart (not tied to a specific product)
- Persist across pages (product → cart → checkout)
- Are stored directly on the order and visible in the admin
- Avoid duplication when multiple products are involved

### Trade-offs vs Line Item Properties
| Cart Attributes | Line Item Properties |
|-----------------|----------------------|
| Global to order | Tied to individual products |
| Ideal for delivery instructions | Better for per-product customization |
| Simple persistence | Requires attaching data per item |
| Visible in admin summary | Scattered across line items |

Given that delivery instructions are order-level information, cart attributes were the correct choice.

---

## Debug Scenario: Delivery Note Disappears After Quantity Change

### Why the Issue Occurred
Dawn updates the cart dynamically using asynchronous `fetch` requests and partial DOM re-renders.  
Early implementations relied on page-load state or network interception, which caused stale cart totals or recursive requests.

This led to:
- Delivery note visibility not re-evaluating after cart updates
- Race conditions, request conflicts (409), and rate limiting (429) in some approaches

### How It Was Debugged
- Reproduced the issue by changing quantities on product and cart pages
- Inspected network requests and confirmed Dawn’s reliance on async cart mutations
- Added targeted console logging to trace cart state and UI updates
- Identified recursion and timing issues when reacting to network calls

### Final Fix
The final solution reacts only to **user-driven DOM events**:
- Product page: Add to Cart form submission
- Cart page: Quantity input changes

After each action, the cart is re-read once using `/cart.js` to recompute visibility.  
This avoids fetch interception, recursion, and race conditions while remaining fully Dawn-compatible.

---

## AI Usage
AI tools were used to assist with ideation, debugging strategies, and documentation structure.  
All final architectural decisions and fixes were reviewed and validated manually.

See **AI-USAGE.md** for full transparency.

---

## UX Improvement (Next Step)
A future enhancement would be:
- Displaying a small helper message such as  
  *“Delivery note available for orders above ₹999”*  
  when the cart total is below the threshold, to better guide users.

---

## Notes
- Implemented on a Shopify **development store**
- No real customer data or payment methods used
- Theme-only solution as required
