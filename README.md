# Meeting Notes to ClickUp Tasks

Paste a meeting transcript into a Google Sheet and it is turned into tracked ClickUp tasks with owners, due dates and priorities.

## Demo
- [demo video link]

![Gemini output in Make](screenshots/Module%202%20output.jpg)

![Tasks created in the ClickUp list](screenshots/Clickup%20list%20output.jpg)

## Problem
- After a meeting, someone reads the notes, works out who agreed to do what by when, and creates each task by hand.
- This takes about 30 minutes per meeting (my own timing).

## How it works
1. A row in the Google Sheet has Status set to New.
2. Gemini returns JSON with a summary, decisions and actions.
3. One ClickUp task is created per action item.
4. The row is marked Processed, with links to the tasks.

## Results
- Time per meeting: about 30 minutes down to 1 to 2 minutes (my own timings).
- Tested on 5 fictional meetings with 18 expected tasks.

| Prompt | Correct tasks | Notes |
|---|---|---|
| v1 | 17 of 18 | After fixing a meeting date I had entered wrongly in the test setup. The first run scored 13 of 18. |
| v2 | 18 of 18 | Fixed the task assigned to an absent person. |

- No invented tasks, wrong owners or wrong dates in v1 (corrected) or v2.
- v2 also extracted one borderline optional item (a teaser video), which the answer key allows.
- v2 was written after seeing the v1 results, so this shows the known failure was fixed, not that all meetings will pass.
- Details are in `docs/test-results.md`.

## Repository contents
- `prompts/`: the extraction prompts, v1 and v2.
- `sample-data/`: fictional transcripts, answer keys and the JSON output for each prompt.
- `docs/`: the PRD and the test results.
- `screenshots/`: screenshots of the Make output and the ClickUp list.
- `make-blueprint/`: the Make scenario, `scenario.json`.

## Setup
1. Create a Google Sheet with the columns Meeting Title, Date, Transcript, Status, Tasks Created.
2. Make the Date column plain text.
3. Import `make-blueprint/scenario.json` into Make.
4. Connect Google Sheets, Gemini (your own API key) and ClickUp.
5. Choose your ClickUp list.
6. Set a row Status to New.
7. Click Run once.

Never commit API keys.

## Build notes
- A wrong test setup caused wrong dates in the first run. The meeting date must come from the sheet.
- ClickUp dates were one day early until I stored them at midday Dubai time.
- Filtering inside the Google Sheets search lost the row number, so the filter sits between modules.
- The ClickUp free plan limits custom task types, so I used a plain list.
- The Status column prevents double processing.

## Limitations
- Small test set: 5 fictional meetings, 18 tasks, with answer keys I wrote myself.
- Due dates depend on the meeting date in the sheet.
- Meetings with no action items stay New.
- Tentative commitments are a judgement call.
- Fictional data only.
- Runs are manual, to stay within the free allowance.

## Production version

| Part | Free tool used here | Paid tools companies use |
|---|---|---|
| Automation | Make | Zapier, Workato, Power Automate, n8n Cloud |
| AI extraction | Gemini free tier | OpenAI API, Azure OpenAI, Anthropic API |
| Input | Google Sheet | Zoom, Teams, Otter, Fireflies, Read.ai |
| Task tracker | ClickUp free | Jira, Asana, Monday, Azure DevOps |

## What I would build next
- Evidence quotes that include the request, not only the reply.
- A tentative flag instead of dropping soft commitments.
- Owner to assignee mapping.
- Automatic transcripts.
- Testing on unseen meetings.
