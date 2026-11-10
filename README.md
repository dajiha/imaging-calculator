# ΔF/F Calculator

A free, browser-based tool for turning raw fluorescence traces into **background-subtracted, photobleaching-corrected fractional changes (ΔF/F₀)**, with publication-ready plots and data export.

It is designed for voltage imaging (GEVIs such as ASAP), calcium imaging, and any time-lapse fluorescence measurement.

**No installation. Your data never leave your computer:** all calculations run locally in the browser.

## Use it

- **Online:** `https://<your-username>.github.io/<repo-name>/` (after you enable GitHub Pages; see below)
- **Offline:** download `index.html` and double-click it.

![screenshot](screenshot.png)

## Workflow

1. **Paste F(t).** Copy columns straight from Excel, Prism, ImageJ/Fiji or a CSV. Use one column per ROI or trace. The first column can be time, or you can give a frame interval instead. An optional header row supplies the trace names.
2. **Paste the background F_bg(t).** Same layout as F(t): a time column plus one background column (applied to all traces) or one per trace. If the time points differ from F(t), the background is linearly interpolated. You can also paste values only, or enter a constant. Draw the background ROI on a **dark, cell-free area** near your ROI, not on a cell: a fluorescent background bleaches and distorts the correction. A good background trace is nearly flat.
3. **Choose the photobleaching correction.**
   - *Linear* fit (reports the bleaching **slope**, %/time and R²) or *mono-exponential* fit (reports τ).
   - Fit only the frames you specify, ideally pre-stimulus or unstimulated frames.
   - The fitted trend is either **subtracted** or **divided** out.
4. **Set F₀.** Use the mean of the baseline frames, or enter a value manually.
5. **Calculate & plot.** You get ΔF/F₀ in % or as a fraction, with an optional sign inversion (−ΔF/F) for indicators that dim when activated.

### Calculation

```
F_c(t)    = F(t) − F_bg(t)
B(t)      = a + b·t                    (linear;  b = bleaching slope)
          = A·exp(−t/τ)                (exponential; log-linear fit)
F_corr(t) = F_c(t) − [B(t) − B(t₁)]    (subtract)
          = F_c(t) · B(t₁) / B(t)      (divide)
F₀        = mean F_corr over baseline frames  (or user value)
ΔF/F₀(t)  = (F_corr(t) − F₀) / F₀
```

> **Which correction should I use?** *Subtract* removes the bleaching trend as an additive drift. *Divide* treats bleaching as a loss of fluorophore. This means a response of a given size stays the same ΔF/F₀ throughout the recording, which is usually more appropriate for long recordings with heavy bleaching.

## Features

- Multiple traces at once, each with one of **10 colorblind-friendly colors**
- Adjustable **line thickness**, optional title and zero line, show/hide individual traces
- **Photobleaching-trend panel** (under section 2) that plots raw F, background, F − bg and the bleach fit as soon as you paste, and flags a background that isn't flat
- **Quality-check panel** that overlays the bleach fit on each background-subtracted trace
- **Export:**
  - `dFF_results.csv`: time, raw F, background, F − bg, bleach fit, corrected F and ΔF/F₀ for every trace
  - `dFF_summary.csv`: analysis settings plus the per-trace F₀, slope, R², τ and peak/min/mean ΔF/F₀
  - Graph as **SVG** (editable in Illustrator/Inkscape) or **PNG** (3× resolution)

## Publish on GitHub Pages

1. Create a new repository and upload `index.html`, `README.md` and `LICENSE`.
2. Go to **Settings → Pages → Build and deployment**, set the source to *Deploy from a branch*, and choose branch `main` with folder `/ (root)`.
3. After about a minute the app is live at `https://<your-username>.github.io/<repo-name>/`.

## Citation

If this tool helps your work, please cite the repository URL.

## License

MIT. See [LICENSE](LICENSE).
