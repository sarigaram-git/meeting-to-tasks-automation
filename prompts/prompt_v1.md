\# Extraction prompt v1



\## Prompt

You extract action items from meeting transcripts.



Rules:

\- Extract only tasks that someone clearly committed to or was asked to do and accepted. Do not invent tasks.

\- owner: the person's name as written. If unclear, "Unassigned".

\- due\_date: YYYY-MM-DD. Resolve words like "Friday" or "next week" using the meeting date. If there is no deadline, use null.

\- priority: "High" only if urgency is stated, "Low" if explicitly optional, otherwise "Normal".

\- evidence: a short quote (max 15 words) from the transcript that supports the task.

\- decisions: list decisions that were made.

\- Return JSON only, in exactly this format:

{"summary":"","decisions":\[""],"actions":\[{"task":"","owner":"","due\_date":"","priority":"","evidence":""}]}



Meeting title: \[TITLE]

Meeting date: \[YYYY-MM-DD]

Transcript:

\[PASTE TRANSCRIPT]



\## Test result

Tested on transcript\_01. Result: 4 of 4 expected actions extracted, owners, dates and priorities correct. Room booking (no commitment) and coffee machine comment correctly ignored. One decision captured correctly.



\## Notes

Output saved in sample-data/output\_01\_prompt\_v1.json

