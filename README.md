# bot-skills

Skills for our OpenMausBot company bots, ready to import.

In OpenMausBot: bot settings → Skills → paste the skill's folder link, e.g.

```
https://github.com/cozyhublk/bot-skills/tree/main/erpnext
```

| Skill | What |
| --- | --- |
| [erpnext](erpnext/SKILL.md) | Customers, leads, quotations, orders, invoices, payments, journal entries, stock and reports in ERPNext with the `erp_*` tools (needs the erpnext MCP server) |
| [erpnext-invoices](erpnext-invoices/SKILL.md) | The invoice bot: sales invoices and credit notes, supplier bills, payments in and out, write-offs, finding missing payments, bank statement matching, customers' and suppliers' billing details (needs the erpnext-invoices MCP server) |
| [send-by-email](send-by-email/SKILL.md) | Send approved quotations, invoices and other documents to customers by email through ChatDesk's email inbox, PDFs attached (needs the chatdesk MCP server with `chatdesk_start_email`) |
| [facebook-page](facebook-page/SKILL.md) | A Facebook Page: draft and publish approved posts (now or scheduled), answer comments, check each post's results after 1 and 7 days, and a weekly report on what works (needs a facebook MCP server, company-box `facebook-mcp`) |
