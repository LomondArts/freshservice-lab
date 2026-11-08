# Freshservice Help Desk Lab

A practice IT help desk I built in a **Freshservice Enterprise trial** (November 2026) after earning the **Freshservice Expert Certification** (Freshworks University). The aim was to practise the day-to-day work of an IT support intern: ticket intake and routing, queue maintenance, user communication, SLA reporting and automation.

All people, emails and tickets are fictional (`@example.com`). Companion project: [ticketing-project](https://github.com/LomondArts/ticketing-project), a Python/pandas tool that produces the same reports from exported ticket data.

**Tools:** Freshservice (tickets, templates, canned responses, Workflow Automator, Analytics, AI Copilot)

---

## 1. Setup
Created a small IT organisation to work with:
- **Agents:** Alex Morgan (Service Desk) and Jordan Lee (Security)
- **Requesters:** 5 fictional employees across Finance, Sales, Marketing, Engineering and People (HR)
- **Groups:** Service desks and Security
- **Ticket categories:** Password reset, Access request, Wi-Fi / network, Software install, Laptop hardware, Email / calendar, VPN, Phishing report (matching the categories in my Python project)

![Agents](screenshots/01a-agents.png)
![Security group](screenshots/01d-security-group.png)
![Ticket fields](screenshots/01e-ticket-fields.png)

## 2. Ticket templates
Built templates for the two most repetitive request types so common fields fill in with one click.

| Template | Prefilled |
|---|---|
| VPN issue | Subject, Category: VPN, Urgency: High, Priority: High, Group: Service desks |
| Password reset / lockout | Subject, Category: Password reset, Urgency: High, Priority: High, Group: Service desks |

![VPN template](screenshots/02a-vpn-template.png)

## 3. Ticket intake and routing
Logged 10 realistic tickets from different requesters and channels (email, portal, chat, phone), then set category, priority and group, and assigned or left them in the queue.

![New ticket from template](screenshots/03a-new-ticket-from-template.png)
![Ticket list after intake](screenshots/03b-ticket-list-after-intake.png)

## 4. Queue maintenance
- **Saved views** for *Unassigned – open* and *High priority – open*
- **Merged a duplicate:** two users reported the same VPN "authentication failed" error. Kept the first ticket (INC-7) as primary and merged INC-10 into it
- **Bulk assignment** to rebalance an uneven workload

![Unassigned view](screenshots/04a-view-unassigned.png)
![Merge – choose primary](screenshots/04c-merge-choose-primary.png)
![Bulk assign](screenshots/04e-bulk-assign-error.png)

## 5. User communication
- Created **canned responses** for acknowledging a request and asking for missing information
- Used "Request more information" on a vague ticket ("Something's wrong with my computer") and set it to **Pending**
- Added a **private note** to the merged VPN ticket: two users with the same error suggests a VPN/identity issue rather than individual devices

![Canned response](screenshots/05a-canned-response-more-info.png)
![Request more info](screenshots/05c-inc9-request-more-info.png)
![Private notes](screenshots/05d-inc7-merge-and-private-notes.png)

## 6. SLAs and reporting
Reviewed the SLA targets per priority, then built a **Service Desk Review** dashboard in Freshservice Analytics with four widgets matching standard help-desk reports:

| Widget | Equivalent in my Python project |
|---|---|
| Requests per category | `volume_by_category()` |
| Open tickets by agent | `workload()` |
| Backlog by priority | `backlog()` |
| Average time past SLA, by priority | `sla_summary()` |

![SLA targets](screenshots/06a-sla-targets.png)
![Service desk dashboard](screenshots/06b-service-desk-dashboard.png)

## 7. Automation
Built a **Workflow Automator** rule: *when a ticket is raised and Category is Phishing report → set Priority to High and assign to the Security group.* Tested it by raising a new phishing ticket at Low priority with no group.

![Workflow canvas](screenshots/07b-phishing-rule-canvas.png)
![Before the rule](screenshots/07c-inc11-before-rule.png)
![After the rule](screenshots/07d-inc11-after-rule.png)

## 8. AI (Freshservice Copilot)
Explored the built-in Copilot features (writing assistant, reply suggester, summaries, similar-ticket suggester) and tested an AI-suggested reply on the phishing ticket.

The draft was polite and asked for useful details, but it **left out the safety guidance** a phishing reply needs: don't click links or pay anything, say whether you already clicked, and forward the email as an attachment rather than replying to the sender. I'd use AI for the first draft and make the security decisions myself.

![Copilot features](screenshots/08a-ai-copilot-features.png)
![AI reply draft](screenshots/08b-ai-reply-draft.png)

## 9. Resolving and closing
Replied to users asking them to confirm the fix, then set tickets to **Resolved** (waiting for confirmation) or **Closed** (finished).

![Confirm fix](screenshots/09a-inc5-confirm-fix.png)
![Send and resolve](screenshots/09c-inc8-send-and-resolve.png)

---

## What I learned
- **Test agents need activated accounts.** My agents used `@example.com` addresses, so they never verified. They didn't appear on the Workload page, and a bulk assignment reported that 2 tickets could not be assigned. Analytics still showed their tickets.
- **Created dates can't be backdated,** which protects a ticket's history. To practise spotting stalled tickets I used SLA due dates instead.
- **Planned end date isn't the SLA due date.** The *Overdue* filter only looks at the SLA "Due by" date.
- **Changing an SLA policy doesn't rewrite history.** Tickets that already breached stay recorded as breached.
- **Merged tickets drop out of volume counts.** After merging, "Received tickets" counted the VPN problem once.
- **Analytics refreshes on a schedule.** Resolved tickets were still counted until the next refresh, so check the "Data updated" time before reporting numbers.
- **Templates can override data you want from the requester.** A template that sets Department to IT hides the requester's real department in reports.
- **Automation rules only apply to new tickets.** Existing tickets still need triaging by hand.
- **Resolution notes are required** before a ticket can be resolved, which keeps a record of the fix for next time.
- **Check reports against the tickets.** The dashboard showed one ticket still unassigned and an unexpected "Urgent" priority, both of which I traced and fixed.
- **AI saves time but needs checking,** especially for security sensitive replies.
