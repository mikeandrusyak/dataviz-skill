# Chart Types Reference

Detailed guide for 25+ chart types: when to use, pitfalls, and implementation notes.

---

## Bar Charts

**Use for**: Comparing values across discrete categories.

**Rules**:
- Always start y-axis at 0
- Sort bars by value (unless order has semantic meaning)
- Horizontal bars preferred for many categories (>6) or long labels
- Avoid grouped bars with >3 groups — use facets instead

**Variants**:
- **Grouped bar**: 2–3 groups per category
- **Stacked bar**: part-to-whole relationships (avoid for more than 3 segments)
- **100% stacked bar**: composition comparison only (not absolute values)
- **Lollipop**: cleaner alternative to bar for sparse data

**Pitfall**: Truncated y-axis exaggerates differences — always start at 0.

---

## Line Charts

**Use for**: Trends over continuous data (usually time).

**Rules**:
- Connect only when the variable is truly continuous
- ≤5 lines per chart — beyond that, use small multiples
- Label lines directly at the end (avoid legend lookup)
- Use `geom_smooth()` or loess for noisy data

**Variants**:
- **Area chart**: emphasizes volume/magnitude beneath the line
- **Stacked area**: part-to-whole over time (careful — hard to read middle segments)
- **Sparkline**: inline mini-chart, trend only, no axes

**Pitfall**: Non-zero y-axis is acceptable for line charts (unlike bar) but must be justified.

---

## Scatter Plots

**Use for**: Correlation / relationship between two numeric variables.

**Rules**:
- Add trend line (linear or loess) when correlation is the story
- Use transparency (`alpha`) for overplotting
- Label outliers directly
- Bubble chart = scatter + size encoding (3rd variable)

**Pitfall**: Correlation ≠ causation — don't imply it in the title.

---

## Histograms & Distributions

**Histogram**: Distribution of one numeric variable.
- Try multiple bin widths (Sturges, Freedman-Diaconis rules)
- Use `stat_bin()` or `geom_histogram(bins=30)` as starting point

**Density plot (KDE)**: Smoothed alternative; better for overlapping groups.

**Box plot**: Summary statistics (median, IQR, outliers). Use for comparing distributions across groups.

**Violin plot**: Combines box plot + density; better than box plot for multi-modal distributions.

**Ridgeline plot** (`ggridges` in R, `joypy` in Python): Overlapping densities for multiple groups; great for temporal or categorical comparisons.

---

## Pie & Donut Charts

**Use for**: Part-to-whole, when there are ≤5 slices and the total is meaningful.

**Rules**:
- Maximum 5 slices (use "Other" for the rest)
- Order slices by size, starting at 12 o'clock
- Label slices directly with percentage
- Donut charts are slightly better (center can hold a summary number)

**Pitfall**: Humans are poor at judging angles — use bar charts when precision matters.

---

## Heatmaps

**Use for**: Matrix data, correlation matrices, calendar data, geographic grid data.

**Rules**:
- Use sequential palette for single-direction data (white → dark blue)
- Use diverging palette when data has meaningful midpoint (red → white → blue)
- Annotate cells with values when precision matters
- Cluster rows/columns by similarity (hierarchical clustering)

---

## Treemaps

**Use for**: Hierarchical part-to-whole data.

**Rules**:
- Area encodes magnitude — scale correctly
- Avoid when hierarchy depth > 2 levels
- Add color as second variable (e.g., performance vs. size)

---

## Slope Charts

**Use for**: Showing change between exactly two time points for multiple categories.

**Construction**: Two vertical axes (before / after), lines connecting category values.

**When to prefer over bar**: When the direction and relative ranking of change is the story.

---

## Bubble Charts

**Use for**: Three numeric variables (x, y, size) + optional color (4th variable).

**Rules**:
- Scale bubble area by value (not radius): `r = √(value / π)`
- Avoid too many bubbles (>30) — use scatter instead
- Legend must explain size scale

---

## Sankey / Alluvial Diagrams

**Use for**: Flows between categories, e.g., survey responses, migration, budget allocation.

**Libraries**: `ggalluvial` (R), `plotly` Sankey (Python), D3 Sankey plugin.

**Pitfall**: Hard to read when there are many nodes — simplify by aggregating small flows.

---

## Choropleth Maps

**Use for**: Geographic distribution of a variable.

**Rules**:
- Use rates (per capita) not raw counts when regions vary in population
- Use sequential or diverging palette (never rainbow/jet)
- Add legend with clear breakpoints
- Consider cartogram when area distorts perception

---

## Small Multiples (Facets)

**Use for**: Comparing the same chart across multiple categories or time periods.

**Rules**:
- Keep the same scale across all facets
- Order facets meaningfully (by value, time, geography)
- Use `facet_wrap()` (R) or `alt.facet()` (altair)

**When to use**: When you have >3 groups that would create a cluttered single chart.

---

## Uncertainty Visualization

Always show uncertainty when data has it:
- **Error bars**: ±1 SD or 95% CI
- **Confidence bands**: around trend lines
- **Gradient plots**: opacity proportional to confidence
- **Hypothetical outcome plots (HOPs)**: animated scenarios

Libraries: `ggdist` (R), `matplotlib` errorbar, altair's `mark_errorband`.

---

## Accessibility Patterns

- Always provide alt text describing chart findings
- Use shape + color (not color alone) for categorical encoding
- Test with colorblind simulator (Coblis, Viz Palette)
- Minimum font size 11pt for labels
- High contrast (WCAG AA: 4.5:1 ratio for text)