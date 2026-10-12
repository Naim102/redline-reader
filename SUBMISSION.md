# Contribution submission text

**Title:** RedLine Reader - plain-language red-line checks for contract clauses

**Use case:** A user lists personal red lines in plain language (for example "No automatic renewal") and submits a contract or terms clause. The Intelligent Contract returns SAFE, REVIEW or REJECT with a one-sentence reason.

**Role of GenLayer consensus:** Judging whether a clause breaks a red line is a qualitative decision that normal contract code cannot make, and LLM answers vary between runs. The contract uses `gl.eq_principle.prompt_non_comparative` so validators check the leader's answer against fixed criteria (required format, valid verdict, reason that matches the clause and red lines) rather than requiring identical text.

**Scope:** Single file, no external APIs, tokens or payments. Includes input validation (empty and over-length inputs are rejected) and a prompt that treats the clause as untrusted text.

**Testing:** Manually tested in GenLayer Studio (deploy, a SAFE-type clause, a REJECT-type clause, empty input). No automated tests yet.

**Known limitations:** One clause per check, only the last result is stored, LLM wording varies, prompt-injection risk is reduced but not eliminated, not legal advice.
