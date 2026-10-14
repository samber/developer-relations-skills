Work order, worst first:

1. **F3** — P0, auth page leak live-format API key. Security beat all else, low reader count no matter. Fix 20 min + key rotation.
2. **F2** — P1, batch-ingest sample call renamed method, guaranteed break, busiest guide, 18,000 readers/mo. Fix 30 min.
3. **F4** — P1, C++ quickstart never compiled, no CI, unverified not confirmed broken, only 700 readers/mo. Fix ~3 eng-weeks (stand up CI).
4. **F5** — P2, 210 samples no expected output, corpus-wide, 120,000 readers/mo, hurt trust/verifiability. Fix ~6 eng-days.
5. **F1** — P2, 61 fences no language tag, cosmetic only (no correctness/security hit), 40,000 readers/mo. Fix ~2 hr scripted.

Rule used: P0 always first (security trump reach). Within same priority tier, confirmed/guaranteed break + reach outrank unverified/low-reach (F2 > F4). Within P2, functional/trust impact + reach outrank pure cosmetic (F5 > F1). Reader count alone never override severity tier.

Next: knock out F3 now — rotate key, strip from page.