# Test results: meeting action item extraction

## Test setup
- 5 fake meeting transcripts (sample-data/transcript_01 to 05), each with a hand-written answer key.
- 18 expected action items in total.
- Transcripts include deliberate traps: relative dates, soft commitments, a shared task, an absent assignee, a cancelled task, a correction mid-meeting, a reassignment, and small talk.
- Prompt run in Gemini (free), one transcript at a time, JSON output only.
- Each run used the meeting title and date below.

| Transcript | Title | Meeting date |
|---|---|---|
| 01 | Project Falcon Weekly Sync | 2026-11-09 |
| 02 | Vendor Onboarding Review | 2026-11-02 |
| 03 | Warehouse Ops Standup | 2026-11-04 |
| 04 | Marketing Launch Prep | 2026-11-12 |
| 05 | Client Escalation Call | 2026-11-05 |

Scoring columns: Expected tasks, Fully correct (right task, owner, due date and priority), Wrong date, Missed, Invented, Wrong owner. Borderline items are not counted as errors.

## Prompt v1, first run
| Transcript | Expected | Fully correct | Wrong date | Missed | Invented | Wrong owner |
|---|---|---|---|---|---|---|
| 01 | 4 | 4 | 0 | 0 | 0 | 0 |
| 02 | 3 | 1 | 2 | 0 | 0 | 0 |
| 03 | 4 | 1 | 2 | 1 | 0 | 0 |
| 04 | 4 | 4 | 0 | 0 | 0 | 0 |
| 05 | 3 | 3 | 0 | 0 | 0 | 0 |
| Total | 18 | 13 | 4 | 1 | 0 | 0 |

Note: the 4 wrong dates in transcripts 02 and 03 were a test setup error. The meeting date line in the prompt was left at the value used for transcript 01 (2026-11-09), so relative dates like "today", "tomorrow" and "Friday" were resolved against the wrong date.

## Prompt v1, corrected run
After fixing the meeting date and title for transcripts 02 and 03:

| Transcript | Expected | Fully correct | Wrong date | Missed | Invented | Wrong owner |
|---|---|---|---|---|---|---|
| 01 | 4 | 4 | 0 | 0 | 0 | 0 |
| 02 | 3 | 3 | 0 | 0 | 0 | 0 |
| 03 | 4 | 3 | 0 | 1 | 0 | 0 |
| 04 | 4 | 4 | 0 | 0 | 0 | 0 |
| 05 | 3 | 3 | 0 | 0 | 0 | 0 |
| Total | 18 | 17 | 0 | 1 | 0 | 0 |

17 of 18 correct (94%). The one miss: in transcript 03, the task assigned to Nadia, who was absent. The v1 rule required the person to have accepted the task, so an absent person's task was skipped.

## Prompt v2
Change: first rule rewritten so that tasks clearly assigned to a named person are extracted even if that person was not at the meeting. Cancelled tasks and tentative ideas with no owner are ignored.

| Transcript | Expected | v1 corrected | v2 |
|---|---|---|---|
| 01 | 4 | 4 | 4 |
| 02 | 3 | 3 | 3 |
| 03 | 4 | 3 | 4 |
| 04 | 4 | 4 | 4 |
| 05 | 3 | 3 | 3 |
| Total | 18 | 17 (94%) | 18 (100%) |

v2 fixed the missed absent-person task with no regressions on the other four transcripts. No invented tasks, wrong owners or wrong dates in v1 (corrected) or v2.

## Borderline items (not counted as errors)
- Transcript 02, Khalid: "I'll try to finish the forms this week". Not extracted in v1 or v2.
- Transcript 04, Carlos: optional teaser video. Extracted as Low priority with no due date in v1 and v2.

## Findings
1. Wrong due dates in the first run came from the test setup, not the model. The automation must take the meeting date from the sheet's Date column.
2. Tasks assigned to absent people were missed by v1 and fixed in v2.
3. Cancelled tasks, small talk, and vague ideas with no owner were correctly ignored in every run.
4. Corrections and reassignments mid-meeting (transcript 05) were handled correctly: the final deadline and final owner were used.
5. In v2, evidence quotes for transcript 05 are short replies (for example "Yes, today.") that do not say what the task is.

## Limitations
- Small test set: 5 fake transcripts, 18 tasks. Answer keys were written by me.
- v2 was written after seeing v1 results, so 18/18 on the same transcripts does not prove it generalises. It shows the known failure was fixed without breaking the other cases.
- Due dates depend on the meeting date entered in the sheet. A wrong or blank date gives wrong due dates.
- Tentative commitments ("I'll try", "if things are quiet") are handled inconsistently. This is a judgement call, not a clear right answer.
- All data is fictional.

## Next steps
- v3 idea: evidence should include the request as well as the reply.
- v3 idea: add a "tentative" flag instead of dropping soft commitments.
- Add a check that the meeting date is filled in before the automation runs.
- Test v2 on new transcripts not used while writing the prompt, through the finished automation.
