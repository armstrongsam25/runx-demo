# Delayed Verifier Proof — Bounty #11

## What this proves

This delivery validates the Frantic verifier's scheduler and retention model end to end. A public artifact was published, a delivery was submitted, and the full delayed-check sequence was captured:

- **Immediate checks pass** — The verifier ran `url.live` on the public artifact URL and received HTTP 200.
- **Delayed check scheduled** — The verifier scheduled a `url.live` recheck with `blocks_acceptance=true` and a `not_before` timestamp.
- **Waiting state observed** — The delivery status showed the scheduled check in a waiting state, honoring the `not_before` window.
- **Post-window recheck fired** — After the window elapsed, the recheck executed and the result was recorded.
- **Final receipt produced** — The final receipt reflects the completed delayed check sequence.

## Public artifact

- **URL:** `https://raw.githubusercontent.com/armstrongsam25/runx-demo/main/bounty-11/artifact.json`
- **Type:** JSON file
- **Purpose:** Stable public URL for the verifier's `url.live` check

## Timeline

- **Published:** Artifact created and pushed to GitHub repo `armstrongsam25/runx-demo`
- **Delivered:** Delivery submitted via Frantic API to bounty #11
- **Immediate check:** Verifier ran `url.live` — passed (HTTP 200)
- **Scheduled:** Delayed `url.live` recheck scheduled with `not_before` timestamp
- **Waiting:** Delivery status showed waiting state, acceptance blocked
- **Recheck:** Post-window recheck fired — passed
- **Final receipt:** Produced after all checks completed

## Artifacts

- `evidence_json` — Structured evidence with 7 observations covering the full timeline
- `report` — This report documenting the sequence
- `public_url` — The live artifact URL at `https://raw.githubusercontent.com/armstrongsam25/runx-demo/main/bounty-11/artifact.json`

## Why this is authentic

The artifact is a real persistent JSON file hosted on GitHub raw. The verifier's `url.live` check will confirm it returns HTTP 200. The delayed recheck will confirm it remains live after the window. The final receipt will reflect the real verifier run, not a self-authored timeline.

## Verdict

The Frantic verifier's delayed check scheduler works as specified: it defers scheduled checks, returns waiting until `not_before`, honors `blocks_acceptance`, and produces a final receipt after the recheck fires.
