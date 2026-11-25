---
name: facebook-page
description: "Run the company's Facebook Page with the facebook_* tools: draft and publish approved posts (now or scheduled), answer comments, track every post's results after 1 and 7 days, and write a weekly report on what works. Use for any Facebook post, comment, Page statistics or social media report."
---

# Facebook Page

The `facebook_*` tools act on one Page. Everything they publish is public at once, so the rule is
simple: **your supervisor sees the exact post first, and every public tool waits for an approval card.**
Read tools need no approval.

| Public (approval card) | Read |
| --- | --- |
| `facebook_publish`, `facebook_edit_post`, `facebook_delete_post`, `facebook_comment`, `facebook_hide_comment` | `facebook_page_info`, `facebook_list_posts`, `facebook_get_post`, `facebook_list_comments`, `facebook_post_insights`, `facebook_page_insights`, `facebook_post_log` |

## 1. Before you write

1. `facebook_post_log` (last 30 days) and `facebook_list_posts` (last 10): what was said lately, so you do
   not repeat a subject or an opening, and the **topics already in use**.
2. Your latest weekly report (section 5), if any: which topics, formats and times worked.

## 2. Draft the post

Write it in the language your supervisor asks for (the Page's usual language otherwise). Then show
your supervisor, in chat, exactly what will be published:

```
Text:       <the full post text>
Photos:     <paths>            or  Link: <url>            (not both)
First comment: <text or none>
When:       now  |  2026-12-01 18:30 (company time)
Topic:      <topic>    Campaign: <campaign or none>
Why:        <one line: why this subject, format and time>
```

How to write it:
- **The first line carries the post.** Facebook shows about two lines before "See more". Lead with the
  news, the benefit or a question, not with greetings or the company name.
- Short paragraphs, plain words, at most one emoji per paragraph, 0-3 hashtags.
- One call to action (comment, visit, message us), or none.
- Facts only from what your supervisor gave you or a source you name. No prices, offers or dates
  unless your supervisor gave them. No customer names, photos or messages without their consent.
- Photos must be files in the company folder (e.g. `marketing/`). Use only pictures the company may
  use. If you have none, ask; never post a placeholder.
- A source link goes in **First comment** (immediate posts only), unless the post is about the link
  itself; then use **Link** for the preview card.
- **Topic**: reuse a topic from the post log when one fits, written the same way ("offer", not
  "Offers" one week and "promo" the next). Reports group by topic, so consistent labels matter more
  than precise ones. A new topic is fine for something new. **Campaign**: the product, event or season,
  when there is one.
- **When**: from the weekly report's best times, if it has enough posts; otherwise ask. A time
  goes in `scheduled_at` with the company's time-zone offset, 10 minutes to 30 days ahead.

Change the draft until your supervisor approves it. Approval in chat is for the content; the card is
for the action.

## 3. Publish

Call `facebook_publish` **once** with exactly the approved content, `topic`, `campaign`, and in
`notes` the "Why" line. The approval card appears; after it is approved:

- **Success**: tell your supervisor the `permalink` (or the scheduled time).
- **`first_comment_error`**: the post is live. Add the comment with `facebook_comment` on the
  `post_id` (another card). Do not publish again.
- **An error**: read it. Before trying again, check `facebook_list_posts` (or `scheduled: true`) that the
  post is not there, so it never goes out twice. Photo or permission errors: tell your supervisor.

Edit or delete a post only when your supervisor asks; `facebook_edit_post` changes only the text.

## 4. Check each post's results

When asked, or in your daily routine:

1. `facebook_post_log` → the posts published **1 day** and **7 days** ago.
2. `facebook_post_insights` with their ids (up to 25 per call).
3. For each, work out: **views** (`post_media_view`), **people reached** (`post_total_media_view_unique`),
   **engagements** = reactions + comments + shares + clicks, and **engagement rate** = engagements ÷
   people reached (show as %). Compare with the median of the Page's last 20 posts at the same age.
4. Tell your supervisor in a few lines only what stands out: a post at least twice the median, or under
   half of it, with your best reason (subject, first line, photo, time). Nothing stands out: say so in one
   line.

Numbers keep growing for days; compare posts only at the same age (1 day with 1 day, 7 with 7).
A metric under `unavailable` is one Meta no longer gives: mention it once and carry on with the rest.

## 5. Weekly report

1. `facebook_page_insights` for the last 7 days and for the 7 days before (`since`/`until`), `period: day`:
   views, people reached, post engagements, follows gained and lost.
2. `facebook_post_log` + `facebook_post_insights` for every post of the last 28 days.
3. Write the report:
   - **The week in numbers**: each Page total, with the change against the week before (+/- %).
   - **Posts this week**: one line each: date, topic, format, people reached, engagement rate, link.
   - **What works** (28 days): average people reached and engagement rate by **topic**, by **format**
     (text, link, photo, photos), by **day of the week** and by **time of day** (morning, afternoon,
     evening, company time). Say "too few posts to tell" for any group with fewer than 3 posts; never
     draw a lesson from one post.
   - **Best 3 and weakest 3 posts** of the 28 days, each with the likely reason.
   - **Next week**: 3 concrete suggestions (subject, format, day and time), each tied to a finding above.
4. Send it to your supervisor in this chat. Their decisions on the suggestions guide the next drafts.

## 6. Comments

1. `facebook_list_posts` (last 7 days) → `facebook_list_comments` for each post with comments.
   Skip comments with `from_page: true` and comments you already handled (the Page replied under them:
   their `replies` count went up and a reply is from the Page).
2. Sort each new comment:
   - **Question** or **praise**: draft a short, friendly reply in the commenter's language.
   - **Wants to buy, book or complain**: draft a reply inviting them to message the Page (or the
     company's WhatsApp, if your supervisor gave it); never discuss an order, price or complaint in public.
   - **Spam, scam links or abuse**: propose hiding it.
3. Show your supervisor the list: comment, link, your proposed reply or "hide". After their OK, send each
   with `facebook_comment` (on the comment id, so it is a reply) or `facebook_hide_comment`, one card each.
   Comments are never deleted.

## 7. When something fails

- **"code 190"** / "the Page token no longer works": stop using the Facebook tools and tell your
  supervisor; an administrator must reconnect the Page.
- **"lacks a permission"**: tell your supervisor which action failed; do not try another way.
- Never publish through any other tool, browser or account.
