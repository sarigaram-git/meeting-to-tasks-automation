# PRD: Meeting Notes to Action Items to Tasks

Author: Sariga

## Problem
- After a meeting, someone reads the notes, works out who agreed to do what by when, and creates each task in the tracker.
- This takes about 30 minutes per meeting (my own timing).
- Relative dates, passing assignments, cancelled items and half-agreed items are easy to get wrong.

## Users
- Team leads, project managers and PMO staff who run recurring meetings.

## Goal
- Turn a pasted transcript into tracked tasks with owner, due date and priority.
- No manual steps, and an auditable trail.

## Success metrics
- Time per meeting drops from about 30 minutes to 1 to 2 minutes (my own timings).
- Extraction accuracy on 5 fictional test transcripts (18 expected tasks):
  - Prompt v1: 17 of 18, after I fixed a meeting date I had entered wrongly in the test setup. The first run scored 13 of 18 because of that error.
  - Prompt v2: 18 of 18.
  - No invented tasks, wrong owners or wrong dates in v1 (corrected) or v2. v2 also extracted one borderline optional item (a teaser video, Low priority, no due date), which the answer key allows.
- Every task carries a supporting quote. Some quotes are short replies that do not name the task (see open questions).

## In scope
- Google Sheet as input: title, date, transcript, Status.
- Gemini free API tier extracts summary, decisions and action items as JSON.
- One ClickUp task per action item.
- Status changes from New to Processed, with task links written back.

## Out of scope
- Automatic transcript capture.
- Mapping owners to ClickUp assignees.
- Notifications.
- Real meeting data.
- Scheduled runs.

## Requirements
- Only rows with Status New are processed.
- Due dates are resolved from the meeting date in the sheet and stored at midday Dubai time.
- Tasks assigned to absent people are included.
- Cancelled items and vague ideas without an owner are ignored.
- Priority is High only when urgency is stated.

## Constraints
- Free tools only.
- Fictional data only.

## Risks and mitigations
- Wrong due dates: the meeting date comes from the sheet, and dates are stored at midday.
- Invented tasks: a rule against inventing, and an evidence quote per task.
- Duplicates: the Status column.
- Tentative commitments handled inconsistently: listed as a known limitation.
- Data privacy with real meetings: fictional data now, vendor review before any real use.

## Open questions and roadmap
- Evidence quotes should include the request, not only the reply.
- Add a tentative flag instead of dropping soft commitments.
- Map owner to assignee.
- Automatic transcripts from Zoom or Teams.
- Test on meetings not used while writing the prompt.
