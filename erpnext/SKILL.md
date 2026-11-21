---
name: erpnext
description: "How to record customers, leads, quotations, orders, invoices, payments and journal entries in ERPNext with the erp_* tools, and read stock, prices, unpaid invoices and reports. Use whenever the accounting app is ERPNext."
---

# Working in ERPNext

ERPNext is the system of record (ERPNext). Its tools start with `erp_`. Document IDs (for example
`SAL-QTN-2026-00012`, `ACC-SINV-2026-00034`) are the numbers: never make up your own. Give people the `link` a
tool returns (or `erp_link`) when they need to open a document.

## Rules

- **Search before you create.** Customers and leads are stored with the full number (`+94771234567`). Search
  with the last 9 digits of the mobile number (`771234567`), which matches every way of writing it, then by name.
  Several customers with the same number may already exist: pick the one with recent documents
  (`erp_get_customer`), and ask if it is unclear. Never add a customer who is already there.
- **Leads and customers.** Someone who asks but has not bought is a lead (`erp_create_lead`). Set `utm_source`
  only to one of `erp_list_documents "UTM Source"` (WhatsApp, Telegram, …), else leave it out. A quotation may go
  to a lead, but before an order turn the lead into a customer: `erp_create_customer` with `from_lead`.
- **Everything starts as a draft.** `erp_create_*` makes drafts; nothing reaches the books. Fix a draft with
  `erp_update_draft` (a new `items` list replaces all rows).
- **`erp_submit` only after your supervisor approved that document** in this chat (number, customer, total).
  A submitted document is final and cannot be changed by you; if it is wrong, tell your supervisor: people cancel
  it in ERPNext.
- **A draft prints with a DRAFT mark.** A draft's PDF is only for your supervisor to check. After `erp_submit`,
  run `erp_download_pdf` again and send that file, never the draft one.
- **Orders and invoices come from submitted documents.** `from_quotation` needs a submitted quotation,
  `from_sales_order` a submitted sales order.
- **Payments:** `erp_create_payment` only when your supervisor says the money arrived (how much, how, the
  reference). A payment proof from a customer is not enough. Bank payments need `reference_no` and
  `reference_date`. If it says the mode of payment has no default account, tell your supervisor: someone must set
  the bank account on that mode of payment in ERPNext.
- **Prices** come from the selling price list (`erp_search_items` shows them). Use another rate only when the
  supervisor or the approved quotation says so. If a tool says an item has no price, ask.
- **Stock:** `in_stock` is what is on the shelf now; `projected` subtracts what is already ordered. Do not promise
  a delivery date when `projected` is below the quantity asked for: say so to your supervisor.
- Dates are `YYYY-MM-DD`. Amounts are in the company's currency.
- **Link the work:** `erp_add_comment` on the document with the ChatDesk conversation link, and the document
  number as a ChatDesk attribute (`quote_no`, `invoice_no`).

## Common jobs

| Job | Tools, in order |
| --- | --- |
| Quotation | find or add the customer / lead → `erp_search_items` → `erp_create_quotation` → `erp_add_comment` → `erp_download_pdf` (folder `quotations`, draft for checking) → private note with the PDF + approval → `erp_submit` → `erp_download_pdf` again → send that PDF |
| Order confirmed | lead? `erp_create_customer` with `from_lead` first → `erp_create_sales_order` with `from_quotation` and `delivery_date` → approval → `erp_submit` |
| Invoice | `erp_create_sales_invoice` with `from_sales_order` (or customer and items) → `erp_download_pdf` (folder `invoices`, draft) → approval → `erp_submit` → `erp_download_pdf` again → send that PDF |
| Who owes us | `erp_unpaid_invoices` (`overdue_only` for follow-ups), `erp_get_customer` for one customer |
| Money received | `erp_create_payment` against the submitted invoice (`amount` for part of it) → approval → `erp_submit` |
| Day's accounts | `erp_run_report` "Sales Register" and "General Ledger" (`from_date`, `to_date` = the day), "Accounts Receivable" (`report_date`); `erp_list_documents` "Payment Entry" with `[["posting_date","=","<day>"],["docstatus","=",1]]` |
| Stock | `erp_search_items`, or `erp_run_report` "Stock Balance" (`from_date`, `to_date`) |
| Journal entry | `erp_list_documents` "Account" with `[["is_group","=",0]]` to find account IDs → `erp_create_journal_entry` (debits = credits) → approval → `erp_submit` |

For any other document, `erp_list_documents` (filters like `[["status","=","Overdue"]]`) and `erp_get_document`
read it. You cannot delete or cancel anything; drafts that are no longer needed: tell your supervisor.

If a tool fails with a permission error, do not look for another way in: tell your supervisor which tool and
document, so a person can fix the bot user's roles in ERPNext.
