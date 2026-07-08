# LaTeX pass — fold WP2 (synthetic family) + WP3 (expanded fault injection) into the paper

Closes the gap the targeted review found: the artifact (on `main`) now contains WP2/WP3,
but the paper described their content as *future work*. These four edits report the result
instead. All applied to local `ACM/` (gitignored → sync to Overleaf); anchors verified
against the current `.tex`.

## ⚠️ AUTHOR ACTION BEFORE SUBMISSION — confirm the numbers under K

The campaign numbers below (87 mutants; 73 behavioral; 32 property-detected = 44%; LR 4×,
TK 5×) were computed in the authoring environment with **`ld_exec`**, a faithful
re-implementation of the K-ESBMC scan semantics (K/`krun` was not installed). `ld_exec` is
validated — it reproduces the paper's **RQ1 reference done-bit traces (Table 2)** exactly and
agrees with **NuSMV** and all WP2 labels (`k-esbmc/rung11_wp3/validate_exec.py`). But the
paper states these as **K-ESBMC** results, so before submission **regenerate them with the
canonical oracle**:

```bash
for p in k-esbmc/rung10_wp2/family/*.ld; do
  python3 k-esbmc/rung6/differential.py "$p" "${p%.ld}.json" "${p%.ld}.props.yaml"
done   # under K (krun)
```

The **qualitative** claims are robust to small shifts (all five operators now fire; LR/TK no
longer inert; the gap stays well under half). If a mutant or two classifies differently under
`krun`, update the three integers in Table~\ref{tab:e3:family} and the "44%" figure to match.

---

## Edit 1 — `_sec_e3.tex`, §4.1 Benchmarks (introduce the family)
Appended to the Benchmarks paragraph (after "…affects only *when* a timer fires (§…)."):
> Because the public suite exercises the construct fragment only incidentally — its one timer
> is dead code (§4.4) — we complement it with a controlled *synthetic family* of 15 small
> programs (bringing the corpus to 28), each targeting one construct or interaction … with a
> property that *observes* that construct. A coverage matrix (with the artifact) records that
> every construct and every property kind is exercised; this family drives §4.4.

## Edit 2 — `_sec_e3.tex`, §4.4 (new paragraph + `tab:e3:family`)
Added after "Why this matters": a **"A systematic campaign on the synthetic family"**
paragraph reporting 87 mutants, LR 4× / TK 5× (was 0 / 1), 73 behavioral, 32 (44%)
property-detected, plus the per-operator table `tab:e3:family` (CP/CO/LR/GC/TK). Contrast:
27% on public benchmarks vs 44% on the purpose-built family — both under half.

## Edit 3 — `_sec_disc.tex`, §5 Fault injection (future-work → done)
Rewrote the paragraph: the broader campaign (latch/timer-heavy family, all operators firing)
is now *reported*, not deferred; the only remaining extension is mutating a verifier's
*translation code* directly.

## Edit 4 — `_sec_disc.tex`, §5 Coverage & external validity (touch)
"Our conclusions rest on 13 programs" → "13 differential benchmarks — complemented by a
15-program synthetic family (corpus 28) for construct coverage and fault injection (§4.4)".

---

## Consistency checks (done)
- `tab:e3:family` defined once, referenced twice (§4.4, §5); `sec:e3:setup` / `sec:e3:mutation`
  refs resolve. No dangling `\ref`/`\cite`/`\label` introduced.
- No new bib entries needed.

## Also flagged by the targeted pass (not applied to paper)
- **Delete `fig_kld_overview.tex`** — orphan file, never `\input` (the live figure is
  `fig_kesbmc_overview.tex`), and it still carries the old "K-LD" naming. Removed from local
  `ACM/`; **delete it in Overleaf too.**
- *(Optional)* `Cavada2014` (nuXmv) is in the `.bib` but cited nowhere though Related Work
  mentions "SMV/nuXmv-based flows" — consider a `\cite{Cavada2014}` there.
