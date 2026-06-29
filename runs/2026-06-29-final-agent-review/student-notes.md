# Student Notes

## What Was Confusing

1. The model returned syntactically valid JSON, which looked successful at first.
2. The JSON was semantically unsafe: the same tool appeared in allowed and blocked sets.
3. Strengthening the prompt did not fix the issue; the retry still produced conflicting sets.
4. A validator that only checks "can parse JSON" is too weak for Agent work.
5. The final report is easier to write when every earlier run already saved a small evidence summary.

## Course Feedback

1. Add a tiny Agent policy validator to the course scripts.
2. In the Agent chapter, say explicitly that local model output must pass schema and policy checks before any tool execution.
3. In the final-project chapter, require an evidence index that maps each conclusion to logs or run records.
4. Keep Agent implementation out of the first-stage required path; a planner smoke test plus validator is enough to teach the risk.
