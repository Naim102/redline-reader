# RedLine Reader

A small GenLayer Intelligent Contract that checks a contract or terms clause against a person's own "red lines" and returns a verdict with a short reason.

## Problem

People sign terms and agreements without reading every clause. Rule-based code cannot judge wording such as "renews automatically unless you cancel 30 days before". RedLine Reader lets a user write plain-language red lines (for example "No automatic renewal") and get a consistent, explainable verdict on a clause.

## Verdicts

| Verdict | Meaning |
|---|---|
| `SAFE` | The clause does not break any red line |
| `REVIEW` | The clause is unclear or may break a red line |
| `REJECT` | The clause clearly breaks a red line |

Output format: `VERDICT: <SAFE|REVIEW|REJECT> | REASON: <one short sentence>`

## Why GenLayer

Judging qualitative text needs an LLM, and LLM answers are not identical on every run. GenLayer validators check the leader's answer against fixed criteria (`gl.eq_principle.prompt_non_comparative`), so the network can agree on an acceptable, explainable result instead of an exact string match.

## Files

- `redline_reader.py` - the contract (class `RedLineReader`)

## Contract API

| Method | Type | Inputs | Description |
|---|---|---|---|
| constructor | - | none | Starts with an empty result |
| `check_clause(clause, red_lines)` | write | `clause` (max 1500 chars), `red_lines` (max 500 chars) | Runs the check and stores the verdict |
| `get_last_result()` | view | none | Returns the last stored verdict string |

Input validation: empty inputs and over-length inputs raise an error and the transaction does not store a result.

## Run in GenLayer Studio

1. Open https://studio.genlayer.com
2. Create a new file named `redline_reader.py` and paste the contract. Line 1 must be the runner comment, exactly as in the file.
3. Go to Run and Debug and deploy. There are no constructor arguments.
4. Wait for the deploy transaction to show ACCEPTED.
5. Open `check_clause`, fill in both fields, and click Send Transaction.
6. When the transaction is ACCEPTED, run `get_last_result`.

## Example inputs

Clause: `Either party may end this agreement with 30 days written notice.`
Red lines: `No automatic renewal`
Expected: a `SAFE` verdict.

Clause: `This subscription renews automatically every year unless you cancel 30 days before the renewal date.`
Red lines: `No automatic renewal`
Expected: a `REJECT` or `REVIEW` verdict.

Empty `clause`: the transaction fails with `clause and red_lines must not be empty`.

Exact wording of the reason may differ between runs.

## Testing status

Manually tested by the author in GenLayer Studio: deploy, the two example clauses above, and the empty-input check. There is no automated test suite yet.

## Limitations and assumptions

- v1 checks one clause against one list of red lines.
- LLM output can vary; the reason text will not be identical each time.
- The clause is treated as untrusted text and the prompt tells the model not to follow instructions inside it, but this reduces, not removes, prompt-injection risk.
- This is not legal advice.
- Only the last result is stored; each new check overwrites it.
- Depends on the pinned runner hash in line 1. If a future Studio version rejects it, copy the current hash from the official GenLayer docs ("Your First Contract").

## License

MIT License. See the LICENSE file.
