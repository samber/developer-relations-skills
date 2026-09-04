# Recording brief

The page the recorder reads before shooting. It ships with the script; the script is not finished without it.

## Starting states

One row per beat, copied from the script's Starting state column and made executable.

| Beat | Enter this state with                               | Verified |
| ---- | --------------------------------------------------- | -------- |
| 1    | `git checkout v2.3.0 && ./scripts/seed.sh && clear` | yes      |
| 5    | `git checkout step-2 && code handler.ts:12`         | yes      |

Every state must be reachable by one command from a cold machine, and rehearsed from a machine that has _not_ already run the demo - that is where hidden caches, warm containers and leftover env vars hide. A beat whose state cannot be re-entered forces a full re-shoot when take 4 fails.

## Machine hygiene

- Notifications, calendar alerts, chat badges: off at the OS level, not minimized.
- A recording profile for the browser and the editor: no personal bookmarks, no autofill history, no unrelated tabs, no signed-in personal accounts.
- Terminal 24pt or larger, editor 20pt or larger, high-contrast theme, window sized to 16:9.
- Shell prompt shortened; no directory paths containing customer or employer names.
- Secrets: use throwaway keys that will be rotated after recording. Assume every frame is a screenshot someone keeps.
- Everything slow happens before recording: installs, builds, image pulls, index builds, account verification.

## Off-limits list

Copy the script's `Off-limits` header line here and make it concrete per surface:

- Customer names
- Tenant IDs
- Internal hostnames
- Unreleased features
- Pricing
- Ticket numbers
- Colleague avatars in a chat sidebar

For a B2B or enterprise audience this is not politeness - a recording is forwardable and durable, and it will be replayed in a security or procurement review long after the release it described.

## Take plan

- Record segment by segment, not in one pass. Segment boundaries are the natural cut points and the re-shoot unit.
- Between takes, re-enter the beat's starting state rather than continuing from wherever the failed take left the machine.
- Mark a fluffed take out loud ("cut, again from beat 6") so the edit finds it without scrubbing.
- Audio quality outranks video quality: a viewer forgives a soft image and leaves over a hissy room.
- Record 3-5 seconds of room tone once - the edit needs it to patch gaps.

## After the shoot

The script already contains everything below; this list exists so nothing gets re-derived from the video.

| Artefact            | Source in the script                                                                  |
| ------------------- | ------------------------------------------------------------------------------------- |
| Chapter list        | Segment titles + final timecodes (first `00:00`, three or more, each ≥10s, ascending) |
| Description         | Opening beat's problem statement, prerequisites line, links named in beats            |
| Captions            | Auto-generate, then correct against the narration column - never re-transcribe        |
| Caption fix list    | The identifiers, flags and package names flagged in the metadata block                |
| Short-form clips    | Beats marked clip-able                                                                |
| Companion page      | Beats in order, rewritten as a how-to                                                 |
| Claims to re-verify | Version numbers, benchmark figures, pricing statements                                |

## Publication checks

1. Captions present and corrected - required for prerecorded audio under WCAG Level A, and the only way most viewers read technical terms.
2. Chapter list satisfies the platform's minimums.
3. Every link named in narration is actually in the description.
4. The video plays intelligibly with the screen off (the audio-only pass, re-run on the real recording).
5. Code on screen is readable at phone size.
6. No off-limits item appears in any frame - scrub at 4x with the off-limits list open.
