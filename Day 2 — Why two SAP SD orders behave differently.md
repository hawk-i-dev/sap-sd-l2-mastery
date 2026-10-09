# Day 2 — Why two SAP SD orders behave differently

**Goal:** Given a working order and a failing order, identify the fields that could explain the difference before proposing a configuration change. Allow **60–75 minutes**.

Yesterday you correctly named availability, credit, and plant as useful checks. Today we’ll put them in an investigation order.

## 1. The four layers to compare

| Layer | Ask | Examples to compare |
|---|---|---|
| **Sales context** | Are these orders being sold the same way? | Sales area, order type |
| **Master data** | Did the system receive the same inputs? | Sold-to, ship-to, material sales data, customer-material data |
| **Order item control** | Will the items be processed the same way? | Item category, plant, schedule line category |
| **Result** | Where do their outcomes differ? | Confirmed quantity/date, blocks, delivery status |

A **sales area** is *sales organization + distribution channel + division*. Two orders with the same customer and material can still have different sales contexts. [SAP Learning: sales organizational units](https://learning.sap.com/courses/exploring-sap-s-4hana-sales-essentials/identifying-organizational-units-in-sap-s-4hana-sales_ddb48d1e-1f57-46bd-8d13-d8e4ef6d0560)

## 2. Partners: “customer” can mean different roles

Do not compare only the sold-to party. Check these roles:

- **Sold-to:** places the order
- **Ship-to:** receives the goods
- **Bill-to:** receives the invoice
- **Payer:** pays it

The same business partner can fill all four roles, or different partners can fill them. A different ship-to may affect delivery details; a different bill-to or payer can affect later billing and payment processing. [SAP Learning: partner functions](https://learning.sap.com/courses/fundamental-customizing-in-sap-s-4hana-sales/applying-the-partner-function-concept_aeb6130b-532f-422d-8202-e9a8c1fa9966)

## 3. Three controls at the sales order item

Remember this chain:

```text
Order type + material item category group
              ↓
         Item category
              +
       Material MRP type
              ↓
      Schedule line category
```

The full standard item-category determination can also consider **item usage** and a **higher-level item**. The sales document type, item category, and schedule line category help control how the order is processed. The schedule line category influences functions such as availability checking and requirements transfer. [SAP Help: item category determination](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/7b24a64d9d0941bda1afa753263d9e39/dc89c95360267214e10000000a174cb4.html), [SAP Help: schedule line categories](https://help.sap.com/docs/PRODUCT_ID/a376cd9ea00d476b96f18dea1247e6a5/cb64b65334e6b54ce10000000a174cb4.html?locale=en-US&state=PRODUCTION&version=LATEST)

**L2 lesson:** If two items have different item or schedule line categories, investigate *why they were determined differently*. Do not assume that manually changing a field fixes the underlying cause.

## 4. The plant matters

The plant is where the item is to be fulfilled. In standard determination, possible sources include the **customer-material information record, ship-to party, and material master**. Configuration or enhancements can change the outcome, so inspect the plant actually stored on each order item. [SAP Help: plant determination](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/c9b5e9de6e674fb99fff88d72c352291/173867f400cd407a882ab70451092dde.html)

If one order confirms quantity and another does not, a different plant is an important clue. It is **not yet proof** of the root cause: you still need to check availability and the schedule lines.

## 5. Your Day 2 L2 case

A user reports: **“The same customer ordered the same material twice. Yesterday’s order confirmed 10 units; today’s order confirmed zero.”**

| Field | Working order | Failing order |
|---|---|---|
| Sold-to | C100 | C100 |
| Material | M500 | M500 |
| Order type | Standard | Standard |
| Sales area | 1000 / 10 / 00 | 1000 / 10 / 00 |
| Ship-to | S100 | S200 |
| Plant on item | P100 | P200 |
| Ordered quantity | 10 | 10 |
| Confirmed quantity | 10 | 0 |

**Your task:** Reply with:

1. The **two strongest clues** in the table.
2. **Five checks in order** to investigate the zero confirmation.
3. One sentence distinguishing a **confirmed fact** from a **possible cause** in this case.

Do not change master data or configuration yet. First establish why the failing item uses plant P200 and what its schedule line and availability check show.
