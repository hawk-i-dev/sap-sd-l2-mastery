# Day 1 — Understand the order-to-cash flow

**Goal:** Given a sales order number, you should be able to explain what has happened, what should happen next, and where to start investigating if the process stops.

Allow about **60–75 minutes**. Today is about tracing the process; we’ll examine the configuration behind each step on later days.

## 1. Learn the document chain

For a standard sale of physical goods, picture this flow:

```text
Customer request → Sales order → Outbound delivery → Goods issue → Invoice → Accounting
```

| Step | Business meaning | What an L2 consultant checks |
|---|---|---|
| Sales order | Records what the customer wants and the agreed terms | Customer, material, quantity, price, requested date, blocks |
| Outbound delivery | Organizes what will be shipped | Delivered quantity, shipping point, picking and delivery status |
| Post goods issue | Confirms goods have left the business | Goods issue status and inventory/accounting impact |
| Billing document | Charges the customer | Billing relevance, quantity, price, invoice status |
| Accounting posting | Records the financial result | Whether the billing document posted successfully |

This is a **common flow**, not a rule for every sale. SAP also supports billing directly with reference to an order, depending on the scenario. [SAP billing documentation](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/e322becd165844e5868e590bc8efafaf/6ea89f4943e94ba082090dc4b5df126b.html)

## 2. Learn the organizational context

A **sales area** is the combination of **sales organization + distribution channel + division**. Think of it as *who sells, how they sell, and which product group they sell*. The plant and shipping point matter when the order is fulfilled. [SAP Learning: organizational units](https://learning.sap.com/courses/exploring-sap-s-4hana-sales-essentials/identifying-organizational-units-in-sap-s-4hana-sales_ddb48d1e-1f57-46bd-8d13-d8e4ef6d0560)

When comparing a failing order with a successful one, check whether they use the same sales area. “Same customer and material” alone does not mean the system will process them identically.

## 3. Use document flow as your first map

**Document flow** shows the preceding and subsequent documents connected to an order. It lets you see, for example, whether a delivery or invoice exists before you investigate why one is “missing.” You can inspect the whole order or an individual item. [SAP Help: displaying document flow](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/7b24a64d9d0941bda1afa753263d9e39/6aa7c6535e601e4be10000000a174cb4.html)

For every ticket, first establish:

1. **Expected result:** What did the user expect, and by when?
2. **Actual result:** What document or status exists now?
3. **Scope:** One order, one item, one customer, or many?
4. **Point of failure:** At order, delivery, goods issue, billing, or accounting?
5. **Evidence:** Document numbers, exact error text, status, and a comparable working case.

This prevents you from changing pricing or configuration when the real issue is a delivery block—or investigating billing when no delivery has been created.

## 4. Your first L2 ticket

A user reports:

> “Sales order 50001234 was delivered yesterday, but the customer has no invoice.”

Write your investigation in **5–8 steps**. Start with what you would inspect in document flow. Then explain how you would distinguish these possibilities:

- No billing document has been created.
- A billing document exists but has an error.
- Billing succeeded, but the customer did not receive the invoice.

You do **not** need to know every transaction code yet. Focus on the evidence you would gather and the order in which you would check it. Send me your answer, and I’ll review it before Day 2.
