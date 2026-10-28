# AI Proposal Triage System Design

## 1. AI vs. Rules
The AI only handles Step 3: reading the proposal PDF and pulling out facts (prices, dates, licence numbers) into a clean JSON object. Plain code handles everything else: validating schemas, running pass/fail checks, calculating scores, and sorting the final list. 

I drew the line here because LLMs are great at parsing messy text, but bad at math and prone to hallucinating numbers. Code is 100% predictable and secure. If a contractor puts a fake trick note inside their proposal, the AI might read it as text, but it can never alter the code's math.

## 2. Consistency and Explainability
Because extraction uses strict schema rules and all scoring runs via fixed math in code, running the same proposal twice will always yield the exact same score. 

Reviewers can see an itemized score breakdown for all five criteria. Every entry in `ranking.json` includes a short reason explaining the position, and reviewers can click any score to verify the extracted numbers against the source PDF text.

## 3. Human in the Loop
The system uses human oversight at key checkpoints rather than fully automating decisions:
- **Flagged queue:** If data is missing or looks suspicious, it goes to human review instead of auto-failing.
- **Ineligible checks:** Code flags failing bids (like missing a mandatory licence), but a human verifies them before any notices go out.
- **Manual overrides:** Reviewers can edit incorrect AI extractions, which immediately triggers a score recalculation.
- **Final choice:** The software only provides a recommendation; a human always makes the final award decision.

All raw text, AI outputs, scoring versions, and manual overrides are saved in an audit log.

## 4. Changing Criteria
Admins configure rules, weights, and criteria through admin screens so nothing is hard-coded. 

If criteria change mid-way, the system bumps the version and marks proposals for recalculation. Because the factual data is already stored as JSON, the code just runs the new math formulas instantly without calling the AI API again. If a brand-new criterion is added that wasn't in the original schema, only that specific field triggers a re-extraction.

## 5. Risks and Safeguards
- **Bad data and hallucinations:** The prompt tells the LLM to leave missing info blank (`null`) instead of guessing. The UI shows side-by-side comparisons with the PDF text so humans can spot errors quickly.
- **Prompt injection:** Proposal text is safely isolated inside XML tags so the AI treats it strictly as passive data, never as commands. Scoring happens safely in code anyway.
- **Bias:** Scores are based purely on objective math and admin-defined rules, avoiding subjective grading.
- **Incomplete bids:** Missing required info routes bids to a human review queue rather than throwing them out automatically.
- **Cost and speed:** AI is only called once per proposal for extraction. All sorting, filtering, and scoring run locally in milliseconds.