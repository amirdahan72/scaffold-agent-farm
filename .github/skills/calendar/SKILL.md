---
name: calendar
description: >
  Use this skill when the user asks to "schedule a meeting", "create an event", "book a meeting",
  "find a time", "check availability", "accept a meeting", "decline an invite", "cancel a meeting",
  "update a meeting", "reschedule", "what's on my calendar", "show my meetings", "forward a meeting",
  "find meeting rooms", or any Outlook calendar operation. Triggers include "calendar", "meeting",
  "schedule", "event", "invite", "availability", "free/busy", "meeting room".
---

# Calendar Skill

Manage Outlook calendar events — create, update, accept, decline, cancel, find meeting times, and check availability — powered by the `microsoft-outlook-calendar` MCP server.

## Overview

This skill uses the **microsoft-outlook-calendar MCP** (Microsoft Graph API) to perform calendar operations. All timestamps use ISO 8601 format. Online meetings (Teams) are created by default.

## MCP Capabilities Reference

### Event CRUD

| Tool | Description | Required Parameters | Key Optional Parameters |
|------|-------------|---------------------|------------------------|
| `ListEvents` | List events from user's calendar with optional filters. Returns master event for recurring meetings. | — | `startDateTime`, `endDateTime`, `meetingTitle`, `attendeeEmails`, `top`, `orderby`, `select` |
| `ListCalendarView` | List events with recurring instances expanded in a time range. **Use this for finding specific meeting instances.** | `userIdentifier` | `startDateTime`, `endDateTime`, `subject`, `top`, `orderby`, `select` |
| `CreateEvent` | Create a new calendar event. All events include a Teams meeting link by default. Default duration is 30 minutes. | `subject`, `attendeeEmails`, `startDateTime`, `endDateTime` | `bodyContent`, `location`, `recurrence`, `isOnlineMeeting`, `importance`, `showAs`, `sensitivity`, `timeZone` |
| `UpdateEvent` | Update an existing event — time, subject, body, location, attendees. | `eventId` | `subject`, `startDateTime`, `endDateTime`, `body`, `location`, `attendeesToAdd`, `attendeesToRemove`, `recurrence`, `importance`, `sensitivity`, `showAs` |
| `DeleteEventById` | Delete a calendar event by ID. | `eventId` | — |

### Invitations & Responses

| Tool | Description | Required Parameters | Optional Parameters |
|------|-------------|---------------------|---------------------|
| `AcceptEvent` | Accept a meeting invitation. | `eventId` | `comment`, `sendResponse` |
| `TentativelyAcceptEvent` | Tentatively accept a meeting invitation. | `eventId` | `comment`, `sendResponse` |
| `DeclineEvent` | Decline a meeting invitation. | `eventId` | `comment`, `sendResponse` |
| `CancelEvent` | Cancel a meeting (organizer only). Sends cancellation to all attendees. | `eventId` | `comment` |
| `ForwardEvent` | Forward an event to other recipients. | `eventId`, `recipientEmails` | `comment` |

### Availability & Scheduling

| Tool | Description | Required Parameters | Key Optional Parameters |
|------|-------------|---------------------|------------------------|
| `FindMeetingTimes` | Suggest times that work for all attendees based on availability. | — | `attendeeEmails`, `startDateTime`, `endDateTime`, `meetingDuration`, `maxCandidates`, `isOrganizerOptional`, `returnSuggestionReasons`, `minimumAttendeePercentage`, `timeZone` |
| `GetUserDateAndTimeZoneSettings` | Get user's timezone, date format, working hours, and language preferences. | — | `userIdentifier` |
| `GetRooms` | List all meeting rooms in the tenant. | — | — |

## Workflow

### Scheduling a New Meeting

1. **Gather details** from the user:
   - **Subject** (required)
   - **Date and time** (required) — resolve to ISO 8601 format
   - **Duration** (default: 30 minutes)
   - **Attendees** (required) — names or email addresses
   - **Location** (optional)
   - **Description/body** (optional)
   - **Recurrence** (optional)

2. **Resolve the timezone**:
   - If the user doesn't specify a timezone, call `GetUserDateAndTimeZoneSettings` to get their default
   - Use that timezone for `startDateTime` and `endDateTime`

3. **Confirm before creating**:
   ```
   Subject: <subject>
   When: <date> <start time> - <end time> (<timezone>)
   Attendees: <list>
   Location: <location or "Teams meeting">
   ```

4. **Create the event** using `CreateEvent`

### Finding a Meeting Time

1. Collect attendees and preferred time range
2. Call `FindMeetingTimes` with `attendeeEmails`, `meetingDuration`, `startDateTime` / `endDateTime`
3. If suggestions returned, present them to the user
4. If no suggestions (`emptySuggestionsReason`):
   - Retry with `isOrganizerOptional: true`
   - If still empty, suggest asking attendees to propose times
   - Optionally scan own calendar with `ListCalendarView` using `select: "subject,start,end,showAs"` to find gaps

### Viewing Calendar

1. Call `ListCalendarView` with `userIdentifier: "me"`, `startDateTime`, `endDateTime`
2. Always use `select` to limit fields: `select: "subject,start,end,showAs,location"`
3. Present in table format

### Responding to Invitations

1. Find the event via `ListCalendarView` with `subject` filter
2. Present details
3. Call `AcceptEvent`, `TentativelyAcceptEvent`, or `DeclineEvent`

### Canceling a Meeting

1. Find via `ListCalendarView`
2. Confirm with user — cancellation notifies all attendees
3. Call `CancelEvent`

## Time & Timezone Handling

### Critical: The `timeZone` parameter is broken

The `timeZone` parameter on `CreateEvent`, `UpdateEvent`, and `ListCalendarView` is **silently ignored** by the MCP tool. All bare datetimes (without a UTC offset suffix) are interpreted as **Pacific Standard Time** regardless of what `timeZone` is set to. This was verified by eval tests (see `evals/RESULTS.md` in the personal-admin farm).

### Mandatory rules for all datetime parameters

1. **ALWAYS append `Z` to datetime strings** to force UTC interpretation:
   ```
   # ❌ WRONG — silently interpreted as PST, even with timeZone param
   startDateTime: "2026-04-20T15:00:00"
   timeZone: "Israel Standard Time"

   # ✅ CORRECT — explicitly UTC, renders as 15:00 IST (UTC+3 in summer)
   startDateTime: "2026-04-20T12:00:00Z"
   ```

2. **IST ↔ UTC conversion** (Israel):
   - **Summer (IDT, late March → late October):** UTC+3 — subtract 3 hours from IST to get UTC
   - **Winter (IST, late October → late March):** UTC+2 — subtract 2 hours from IST to get UTC
   - Example: 15:00 IST in summer → `12:00:00Z` · 15:00 IST in winter → `13:00:00Z`

3. **ListCalendarView day boundaries** — query wide, filter locally:
   The `startDateTime`/`endDateTime` on ListCalendarView also ignore Z-suffix — times default to PST. Since neither `timeZone` param nor Z-suffix produce reliable boundaries:
   ```
   # Query for Apr 20 IST — use bare datetimes (they default to PDT/PST)
   # PDT midnight Apr 20 = 07:00 UTC, which covers the full IST day
   startDateTime: "2026-04-20T00:00:00"
   endDateTime: "2026-04-21T00:00:00"
   # Then POST-FILTER results in agent code:
   # Keep events where (start.dateTime UTC + 3h) falls on Apr 20 IST
   ```
   The API range will be PST-aligned (wider than IST day), so some extra events may appear.
   **Always post-filter by converting each event's UTC start time to IST before assigning to a day.**

4. **Post-write verification** — after every `CreateEvent` or `UpdateEvent`, check the API response:
   - `originalStartTimeZone` should be `"UTC"` (not `"Pacific Standard Time"`)
   - If it says `Pacific Standard Time`, the Z-suffix was missing — delete and retry

5. **Display conversion** — API responses return UTC times. Always add the user's UTC offset before displaying.

## Known Issues

| Issue | Workaround |
|-------|------------|
| `FindMeetingTimes` returns empty / `OrganizerUnavailable` | Set `isOrganizerOptional: true`. If still empty, ask attendees to propose times. |
| `ListCalendarView` cross-user access failure | You can only view your own calendar. For others, use `FindMeetingTimes`. |
| Large calendar output exceeds context | Always use `select` to limit fields. Use narrow time windows (1-2 days). |
| Recurring event instances | Always use `ListCalendarView` (not `ListEvents`) to find specific instances. |
| `UpdateEvent` silently ignores wrong param names | Use `startDateTime` / `endDateTime`, **NOT** `start` / `end`. Wrong names return success but change nothing. Always verify the response times match your intent. |
| **`timeZone` param ignored on CreateEvent** | The `timeZone` parameter is silently ignored — bare datetimes default to PST. **Always use Z-suffix UTC datetimes.** (Eval: TC-02) |
| **`timeZone` param ignored on UpdateEvent** | Same bug as CreateEvent — `timeZone` is ignored on updates too. **Always use Z-suffix UTC datetimes.** (Eval: TC-05) |
| **ListCalendarView boundaries ignore Z-suffix too** | Z-suffix on `startDateTime`/`endDateTime` is also stripped — boundaries default to PST. **Query wide (bare datetimes), then post-filter events by IST in agent code.** (Eval: TC-06, TC-07, Run 2) |
| `DeclineEvent` fails if you're the organizer | Use `CancelEvent` instead. Only non-organizers can decline. |
| `DeclineEvent` with `sendResponse: false` and `comment` | Cannot include `comment` when `sendResponse` is `false`. Either send the response with comment, or decline silently without one. |

## Rules

- **ALWAYS** confirm with the user before creating, updating, canceling, or deleting events
- **ALWAYS** use `ListCalendarView` (not `ListEvents`) for specific recurring event instances
- **ALWAYS** include a Teams meeting link by default (`isOnlineMeeting: true`)
- **ALWAYS** use Z-suffix UTC datetimes (`2026-04-20T12:00:00Z`) for CreateEvent and UpdateEvent — never bare datetimes
- **ALWAYS** pre-convert day boundaries to UTC for ListCalendarView queries
- **ALWAYS** verify `originalStartTimeZone` is `UTC` in the API response after Create/Update
- **NEVER** rely on the `timeZone` parameter — it is silently ignored
- **NEVER** fabricate or guess event IDs — resolve from API responses
- **NEVER** create, cancel, or delete events without explicit user confirmation
- Default meeting duration is 30 minutes if not specified
