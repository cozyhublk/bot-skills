---
name: send-by-email
description: "Send an approved quotation, invoice or other document to a customer by email through ChatDesk, with the PDFs attached. Use whenever a customer or your supervisor wants a document sent by email."
---

# Send by email

Email goes out from the company's email inbox in ChatDesk, like WhatsApp does. Never use any other mail
tool: other tools cannot attach files, send from the wrong address, and leave no record in ChatDesk.

## When

- The customer asked for email, wrote from an email conversation, or has no WhatsApp; or
- your supervisor says to send it by email (they may name the address).

The same approval rule applies as for WhatsApp: send only what your supervisor approved, after they
approved it. The approval card that appears when you send is expected.

## How

1. **Find the address.** Use the one your supervisor gave, else the contact's `email` (`chatdesk_get_contact`).
   No address: ask your supervisor; never guess one.
2. **Is there an email conversation with this customer?** `chatdesk_get_contact` lists their conversations; one
   whose `channel` is `Channel::Email` is an email thread.
   - **Yes**: `chatdesk_send_message` in that conversation with `files` (every PDF in one call, so they go
     in one email), `content`, and `subject` if this email needs its own subject.
   - **No**: `chatdesk_start_email` with `to`, `name`, `subject`, `content` and `files`. It finds or adds the
     contact and opens a new email conversation; replies come back to it.
3. **Subject**: the document and number, e.g. `Quotation Q-2026-0007 – CozyHub Sri Lanka`. For several
   documents: `Quotations Q-2026-0007 and Q-2026-0008`.
4. **Body**: in the customer's language. A greeting with their name, one or two lines saying what is
   attached (number, and validity for a quotation), an invitation to reply with questions, and
   CozyHub Sri Lanka. No prices or totals in the body unless your supervisor asks: the PDF is the offer.
5. **Copies**: add `cc` / `bcc` only when your supervisor asks.
6. **Afterwards**: do the same bookkeeping as after a WhatsApp send (register status `sent`, labels,
   attributes), on the conversation where you prepared the document. If you started a new email
   conversation, also add a private note in the original conversation with the `link` the tool returned,
   so staff see it went by email.

If `chatdesk_start_email` says there is no email inbox, tell your supervisor: ChatDesk needs an email
inbox with the Company Bots user in it. If it says a conversation was made but the email was not sent,
tell your supervisor the error; do not try another way.
