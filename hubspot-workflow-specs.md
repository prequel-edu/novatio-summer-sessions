# HubSpot Workflow Specs — Novatio Summer Sessions 2026

Portal: **49158869** (Novatio)
SMS tool: **SalesMsg**
Last updated: 2026-04-27

---

## Overview

Three separate workflows — one per session type. Each sends 4 SMS messages:
confirmation, 3 days before, 1 day before, 1 hour before.

Timing is driven by custom date properties set at form submission. No branching
by date needed — the workflow delays relative to the property value.

---

## Contact Properties (already created 2026-04-27)

| Property name | Type | Purpose |
|---|---|---|
| `story_session_date` | Date | Workflow delay anchor for Story |
| `story_session_date_display` | Text | e.g. "Tuesday, June 2 at 3pm MST" — used in SMS |
| `game_session_date` | Date | Workflow delay anchor for Game |
| `game_session_date_display` | Text | e.g. "Thursday, June 4 at 9am MST" — used in SMS |
| `social_session_date` | Date | Workflow delay anchor for Social |
| `social_session_date_display` | Text | e.g. "Wednesday, June 3 at 12pm MST" — used in SMS |

These are set by the landing page form JS when the parent picks a date pill.
`summer_sessions_interested_in` is also set (e.g. "Create Your Own Story – June 2").

---

## Suppression

Before enabling any workflow, add a suppression list or branch:
- Contacts where `hs_legal_basis` is unknown (no SMS consent) → skip SMS actions
- Contacts where `phone` is unknown → skip SMS actions

---

## Workflow 1: Create Your Own Story

**Name:** Summer Sessions 2026 — Create Your Own Story SMS

**Enrollment trigger:**
- Contact property `story_session_date` is known
- Re-enrollment: off (one signup per contact per session)

**Timezone for all delays:** America/Phoenix (MST, UTC-7, no DST)

---

### Action 1 — Confirmation SMS (send immediately)

**SalesMsg message:**
```
Hi {{contact.firstname}}! You're signed up for Create Your Own Story with Novatio on {{contact.story_session_date_display}}. It's free, one hour, all virtual. No prep needed. We'll text your Zoom link the day before. Questions? Just reply.
```

---

### Action 2 — Wait

Delay until: `story_session_date` minus 3 days at **10:00 AM MST**

---

### Action 3 — 3-Day SMS

**SalesMsg message:**
```
Hey {{contact.firstname}}, just a heads up — Create Your Own Story is coming up on {{contact.story_session_date_display}}! Your child will build an AI storybook from scratch. No prep needed. Zoom link coming tomorrow!
```

---

### Action 4 — Wait

Delay until: `story_session_date` minus 1 day at **10:00 AM MST**

---

### Action 5 — 1-Day SMS (include Zoom link)

**SalesMsg message:**
```
Tomorrow at 3pm MST! Here's your Zoom link for Create Your Own Story: [STORY ZOOM LINK]. No prep needed — just show up and the teacher takes it from there. See you then, {{contact.firstname}}!
```

> Replace `[STORY ZOOM LINK]` with the recurring Zoom link from Karissa before enabling.

---

### Action 6 — Wait

Delay until: `story_session_date` at **2:00 PM MST** (1 hour before 3pm session)

---

### Action 7 — 1-Hour SMS

**SalesMsg message:**
```
Starting in 1 hour, {{contact.firstname}}! Create Your Own Story begins at 3pm MST today. Join here: [STORY ZOOM LINK]
```

---

## Workflow 2: Build Your Own Video Game

**Name:** Summer Sessions 2026 — Build Your Own Video Game SMS

**Enrollment trigger:**
- Contact property `game_session_date` is known
- Re-enrollment: off

**Timezone:** America/Phoenix (MST)

---

### Action 1 — Confirmation SMS

```
Hi {{contact.firstname}}! You're signed up for Build Your Own Video Game with Novatio on {{contact.game_session_date_display}}. Free, one hour, all virtual. No coding experience needed. We'll text your Zoom link the day before. Questions? Just reply.
```

---

### Action 2 — Wait

Delay until: `game_session_date` minus 3 days at **10:00 AM MST**

---

### Action 3 — 3-Day SMS

```
Hey {{contact.firstname}}, Build Your Own Video Game is coming up on {{contact.game_session_date_display}}! Your child builds a real, playable Scratch game in one hour. No experience needed. Zoom link coming tomorrow!
```

---

### Action 4 — Wait

Delay until: `game_session_date` minus 1 day at **10:00 AM MST**

---

### Action 5 — 1-Day SMS

```
Tomorrow at 9am MST! Here's your Zoom link for Build Your Own Video Game: [GAME ZOOM LINK]. Your child's teacher will walk them through everything — no prep needed. See you then, {{contact.firstname}}!
```

> Replace `[GAME ZOOM LINK]` before enabling.

---

### Action 6 — Wait

Delay until: `game_session_date` at **8:00 AM MST** (1 hour before 9am session)

---

### Action 7 — 1-Hour SMS

```
Starting in 1 hour, {{contact.firstname}}! Build Your Own Video Game starts at 9am MST today. Join here: [GAME ZOOM LINK]
```

---

## Workflow 3: Meet Other Virtual Schoolers

**Name:** Summer Sessions 2026 — Meet Other Virtual Schoolers SMS

**Enrollment trigger:**
- Contact property `social_session_date` is known
- Re-enrollment: off

**Timezone:** America/Phoenix (MST)

---

### Action 1 — Confirmation SMS

```
Hi {{contact.firstname}}! You're signed up for Meet Other Virtual Schoolers with Novatio on {{contact.social_session_date_display}}. Free, one hour, all virtual. We'll text your Zoom link the day before. Questions? Just reply.
```

---

### Action 2 — Wait

Delay until: `social_session_date` minus 3 days at **10:00 AM MST**

---

### Action 3 — 3-Day SMS

```
Hey {{contact.firstname}}, Meet Other Virtual Schoolers is coming up on {{contact.social_session_date_display}}! Games, team challenges, and a group of virtual students who actually get it. Zoom link coming tomorrow!
```

---

### Action 4 — Wait

Delay until: `social_session_date` minus 1 day at **10:00 AM MST**

---

### Action 5 — 1-Day SMS

```
Tomorrow at 12pm MST! Here's your Zoom link for Meet Other Virtual Schoolers: [SOCIAL ZOOM LINK]. No prep needed — just jump in. See you then, {{contact.firstname}}!
```

> Replace `[SOCIAL ZOOM LINK]` before enabling.

---

### Action 6 — Wait

Delay until: `social_session_date` at **11:00 AM MST** (1 hour before 12pm session)

---

### Action 7 — 1-Hour SMS

```
Starting in 1 hour, {{contact.firstname}}! Meet Other Virtual Schoolers starts at 12pm MST today. Join here: [SOCIAL ZOOM LINK]
```

---

## Pre-Launch Checklist

Before enabling any workflow:

- [ ] Get Zoom links from Karissa — one recurring link per session type
- [ ] Replace all `[STORY ZOOM LINK]`, `[GAME ZOOM LINK]`, `[SOCIAL ZOOM LINK]` placeholders in Actions 5 and 7 of each workflow
- [ ] Confirm portal timezone is set to America/Phoenix in HubSpot settings
- [ ] Test with a real contact (use a personal phone number) — sign up on the landing page, verify all 4 texts fire at the right times
- [ ] Confirm SalesMsg rate limits won't be hit if a large batch signs up on the same day (HubSpot retries on rate limit errors, but monitor workflow history for the first week)
- [ ] Add suppression: contacts without phone or without SMS consent

---

## Notes

- Workflows do NOT set lifecycle stage or lead status — the existing Novatio v4 intake workflow (1801307655) handles that separately. Summer session signups should flow through the same intake.
- `lead_source_detail` is set to `summer-sessions-novatio-2026` on all form submissions — useful for filtering summer session contacts in lists/reports.
- If a parent signs up for all 3 sessions, all 3 workflows enroll simultaneously. Each fires independently on its own date property.
- Re-enrollment is off by default. If a parent re-submits the form with a different date, they will NOT get new texts unless re-enrollment is turned on or the workflow is manually re-enrolled.
