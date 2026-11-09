# Extraction prompt v2

## Change from v1

First rule rewritten. v1 required the person to accept the task, so a task assigned to an absent person (transcript 03, Nadia) was missed. v2 covers tasks clearly assigned to a named person, including someone not at the meeting, and ignores cancelled or tentative items.

## Prompt

You extract action items from meeting transcripts.

Rules:

* Extract tasks that someone committed to, or that were clearly assigned to a named person, including a person who was not at the meeting. Do not invent tasks. Ignore cancelled tasks and ignore vague ideas that have no owner or only a tentative "maybe" or "I might".
* owner: the person's name as written. If unclear, "Unassigned".
* due\_date: YYYY-MM-DD. Resolve words like "Friday" or "next week" using the meeting date. If there is no deadline, use null.
* priority: "High" only if urgency is stated, "Low" if explicitly optional, otherwise "Normal".
* evidence: a short quote (max 15 words) from the transcript that supports the task.
* decisions: list decisions that were made.
Return JSON only, in exactly this format:
{"summary":"","decisions":\[""],"actions":\[{"task":"","owner":"","due\_date":"","priority":"","evidence":""}]}

Meeting title: \[TITLE]
Meeting date: \[YYYY-MM-DD]
Transcript:
\[PASTE TRANSCRIPT]

## Test result

Result: 18/18, no regressions

