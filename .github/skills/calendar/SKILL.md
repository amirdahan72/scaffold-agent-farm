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

- **Always use ISO 8601 format**: `2026-03-09T09:00:00`
- **Always specify timezone** via the `timeZone` parameter or ISO 8601 offset (e.g., `+02:00`, `Z`)
- Call `GetUserDateAndTimeZoneSettings` if timezone is not known
- Use the same timezone for both start and end times
- **API responses return UTC times** — even when `timeZone` is set on the request, raw JSON `start.dateTime` / `end.dateTime` values may be UTC. Always add the user's UTC offset before displaying to the user.
- **`CreateEvent` defaults to PST** if no timezone offset is provided — always include an offset suffix (e.g., `+02:00`) or `Z`

## Known Issues

| Issue | Workaround |
|-------|------------|
| `FindMeetingTimes` returns empty / `OrganizerUnavailable` | Set `isOrganizerOptional: true`. If still empty, ask attendees to propose times. |
| `ListCalendarView` cross-user access failure | You can only view your own calendar. For others, use `FindMeetingTimes`. |
| Large calendar output exceeds context | Always use `select` to limit fields. Use narrow time windows (1-2 days). |
| Recurring event instances | Always use `ListCalendarView` (not `ListEvents`) to find specific instances. |
| `UpdateEvent` silently ignores wrong param names | Use `startDateTime` / `endDateTime`, **NOT** `start` / `end`. Wrong names return success but change nothing. Always verify the response times match your intent. |
| `DeclineEvent` fails if you're the organizer | Use `CancelEvent` instead. Only non-organizers can decline. |
| `DeclineEvent` with `sendResponse: false` and `comment` | Cannot include `comment` when `sendResponse` is `false`. Either send the response with comment, or decline silently without one. |
| UTC offset boundary confusion | When querying by day in a non-UTC timezone (e.g., IST/UTC+2), events near midnight may appear in the wrong day's results. Always convert event times to user's local timezone before classifying which day they belong to. |

## Rules

- **ALWAYS** confirm with the user before creating, updating, canceling, or deleting events
- **ALWAYS** use `ListCalendarView` (not `ListEvents`) for specific recurring event instances
- **ALWAYS** include a Teams meeting link by default (`isOnlineMeeting: true`)
- **ALWAYS** resolve timezone before creating events
- **NEVER** fabricate or guess event IDs — resolve from API responses
- **NEVER** create, cancel, or delete events without explicit user confirmation
- Default meeting duration is 30 minutes if not specified
