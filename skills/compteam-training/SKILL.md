---
name: compteam-training
description: Find BJJ and martial arts clubs, public class schedules and membership prices through Compteam; view the member's or a linked child's classes and account status, manage RSVPs, or ask a club about an eligible free trial.
---

Use the Compteam MCP tools supplied by this plugin. Work within their returned capabilities and the signed-in person's account access.

## Find a club or plan

Use `search_clubs` to find participating clubs by name. Resolve an ambiguous club before selecting its `branch_slug`; this tool is a club-name search, not a complete geographic directory. Use `get_public_branch_details` for addresses, contact details, policies, and club-owned links.

Use `list_public_classes` with the selected branch slug and an explicit ISO week-start date. Use the user's date and timezone to resolve "today" or "next week" and retain each class's returned local date and time. Distinguish schedules from the person's bookings.

Use `list_public_membership_plans` for current advertised membership prices. Show the returned currency, billing interval, and any signup fee. These are discovery results; the plugin does not purchase memberships or charge payments. Public discovery does not require account connection.

## View a member's training

For personal classes or account status, use `connect_compteam_account` to verify connection, then `list_my_subjects` to resolve the person, authorized branches, and any linked children. Use the returned identifiers instead of guessing them. Ask which person or branch only when the request is ambiguous.

Use `list_my_classes` for the selected branch and week, passing `child_id` only for a child returned by `list_my_subjects`. Use `get_account_status` for the same subject's limited membership and payment status. Access to a club's public schedule does not grant access to its other members.

If a protected call asks for sign-in, direct the user to Compteam's OAuth connection flow. Resume after sign-in succeeds. Repeating the same tool call does not complete OAuth; never request passwords, access tokens, bank details, or OAuth codes in chat.

## Change attendance

Resolve the exact subject, branch, and class occurrence from current tool results. Before calling `set_class_rsvp`, show the person, club, class, local date/time, and intended action and ask for confirmation. The supported statuses are `attending` and `cancelled`.

After confirmation, submit once and report the actual result, including a waitlist outcome if returned. Do not describe a waitlist as a confirmed place. If the result is uncertain, re-read the class state before retrying or claiming success. Respect server eligibility, capacity, paid-class, and attendance-window restrictions.

## Ask about a free trial

Resolve the club and exact occurrence from its public schedule, then use `show_free_trial_form`. The server checks trial eligibility. Let the user provide the requested details in the form and confirm before submitting `submit_free_trial_inquiry`; never invent form values, consent, or a CAPTCHA token. If the form cannot be completed, provide the club's returned contact or registration link.

A submitted inquiry asks the club about a trial. It does not establish a booking or membership unless the tool explicitly reports that outcome. Do not send a duplicate inquiry after a timeout without checking the previous result.

## Explain limits

This bundle exposes public discovery and the account, schedule, RSVP, and free-trial workflows. It does not provide club administration, athlete registration, membership checkout, payment collection, medical advice, or arbitrary access to another member's records. Offer a returned Compteam or club link when the user needs an unsupported action.
