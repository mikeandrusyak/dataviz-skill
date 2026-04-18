---
name: dataviz
description: >
  Expert skill for creating data visualizations — charts, graphs, dashboards, infographics,
  and interactive plots. Use this skill whenever the user asks to visualize data, create a chart
  or graph, plot a dataset, build a dashboard, explore data visually, or asks "how should I
  display this data?". Also trigger for tool/library selection questions (ggplot2, seaborn,
  altair, matplotlib, Vega-Lite, D3.js), questions about visual encoding, chart types,
  color choices, and perception/cognition in visualization. Even if the user just pastes a
  CSV or table and asks "what can I do with this?" — use this skill. Covers both code-based
  output (Python, R) and inline SVG/HTML artifacts rendered in chat.
version: 1.0.0
author: mikeandrusyak
license: MIT
repository: https://github.com/mikeandrusyak/dataviz-skill
---

# Data Visualization Skill

A comprehensive skill for transforming data into effective, accurate, and beautiful visualizations.
Covers chart selection, visual encoding, tool implementation, and design principles drawn from
visualization research and best practices.

---

## 1. Understand the Task First

Before writing any code or choosing a chart type, clarify:

1. **What is the data?** — shape, types, size (ask for a sample if not provided)
2. **What question should the chart answer?** — comparison, distribution, correlation, trend, part-to-whole, geospatial
3. **Who is the audience?** — expert/analyst vs. general public
4. **What is the output format?** — inline artifact (SVG/HTML), Python/R code, static image, interactive dashboard
5. **What tool/library?** — if not specified, choose based on context (see §3)

If the user says "visualize this" or pastes data without specifying, make a reasonable choice and show it — don't over-ask.

---

## 2. Chart Selection Guide

Match the **data relationship** to the best chart type:

| Relationship | Best Charts | Avoid |
|---|---|---|
| Comparison (categories) | Bar chart, Dot plot, Lollipop | Pie chart (>4 slices), 3D |
| Comparison (over time) | Line chart, Area chart | Bar chart for long time series |
| Distribution | Histogram, Violin, Box plot, Ridgeline | Pie chart |
| Correlation | Scatter plot, Bubble chart, Heatmap | Line chart |
| Part-to-whole | Pie (≤5 slices), Donut, Stacked bar, Treemap | 3D pie |
| Ranking | Bar chart (sorted), Slope chart | Unsorted bar |
| Trend | Line chart, Smoothed line, Sparkline | Bar for continuous data |
| Geospatial | Choropleth, Dot map, Proportional symbol | 3D maps |
| Flow/Process | Sankey, Alluvial, Chord diagram | — |
| Multivariate | Small multiples, Parallel coordinates, Facets | Overloaded single chart |

**Decision heuristic:**
- 1 variable → histogram or bar
- 2 variables (cat + num) → bar or box
- 2 variables (num + num) → scatter
- 3+ variables → color/size encoding + facets, or small multiples
- Time + numeric → line chart

For complex decisions, read `references/chart_types.md`.

---

## 3. Tool & Library Selection

| Context | Recommended | When |
|---|---|---|
| Python, quick/exploratory | **seaborn** | Fast, beautiful defaults, pandas-native |
| Python, declarative/grammar | **altair** (Vega-Lite) | Grammar of Graphics style, interactive |
| Python, full control | **matplotlib** | Custom layouts, publication figures |
| R, grammar of graphics | **ggplot2** | Layered, elegant, publication-ready |
| Inline HTML artifact | **D3.js / Vega-Lite / plain SVG** | Interactive, rendered in chat |
| Dashboard | **Plotly + Dash** (Python) or **Shiny** (R) | Multi-chart, interactive |
| No code (static) | SVG artifact via `show_widget` | Quick visual for chat |

**Default**: if the user hasn't specified a language and just wants to see something — produce an **inline SVG or HTML artifact** using the `show_widget` tool.

---

## 4. Visual Encoding Principles (Grammar of Graphics)

Every visualization maps **data attributes** to **visual channels**. Choose channels by their effectiveness ranking (Mackinlay 1986):

**Quantitative data** (best → worst):
Position → Length → Angle → Area → Color saturation → Color hue → Shape → Volume

**Categorical data** (best → worst):
Position → Color hue → Shape → Texture → Length

### Marks and Channels
- **Marks**: points, lines, areas, bars, symbols
- **Channels**: x, y position, color, size, shape, opacity, texture

### Key rules:
- Encode the **most important variable** with position (x or y)
- Use **color sparingly** — max 6–8 categorical colors
- Avoid encoding the same variable in multiple redundant channels (unless for accessibility)
- Size encodes area, not radius — scale by `√value` for circles

---

## 5. Design & Perception Principles

### Gestalt Principles (apply to layout)
- **Proximity**: group related elements
- **Similarity**: use same color/shape for same category
- **Continuity**: align elements on a grid
- **Enclosure**: use borders/backgrounds for grouping

### Cognitive load
- Working memory holds ~4 items — don't use more than 4–5 categories in one chart
- Use **preattentive attributes** (color, size, motion) to guide attention
- Labels > legends whenever possible (direct labeling reduces lookup time)

### Decluttering checklist
- [ ] Remove chartjunk: unnecessary gridlines, backgrounds, borders
- [ ] Remove or minimize legends (use direct labels)
- [ ] Use muted axis colors (gray, not black)
- [ ] Start bar charts at zero; only break axis for line charts with justification
- [ ] Use consistent scales across facets/small multiples

### Color guidelines
- **Qualitative** (categories): use ColorBrewer or HUSL palettes, ≤8 colors
- **Sequential** (ordered): single-hue gradient (light → dark)
- **Diverging** (deviation from midpoint): two-hue gradient through neutral
- Always test for **colorblind accessibility** (deuteranopia: avoid pure red/green)
- Recommended palettes: `seaborn`'s `colorblind`, ColorBrewer, HUSL

### Avoid misleading practices
- **Proportional ink**: area/length must be proportional to value
- **No truncated axes** on bar charts (line charts: context-dependent)
- **No dual axes** unless clearly labeled and intentional
- **No 3D charts** — they distort perception (tilt angle bias)
- **Show uncertainty** where relevant (confidence intervals, error bars)

---

## 6. Implementation Workflow

### For code output (Python / R):

```
1. Load & inspect data (shape, dtypes, nulls)
2. Clean/transform as needed (melt, pivot, aggregate)
3. Choose chart type (§2)
4. Write minimal working code with chosen library (§3)
5. Apply design principles (§5): labels, colors, declutter
6. Add title + axis labels + source annotation
```

**Always include:**
- Descriptive title
- Axis labels with units
- Source / data credit (if applicable)
- Alt text description (accessibility)

### For inline SVG/HTML artifacts:

Use `show_widget` with clean SVG or HTML+JS. Follow the frontend-design skill for styling.
Load `references/chart_types.md` for complex chart-specific SVG patterns.

---

## 7. Code Templates

### Python – seaborn bar chart
```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_theme(style="whitegrid", palette="colorblind")
fig, ax = plt.subplots(figsize=(8, 5))
sns.barplot(data=df, x="category", y="value", ax=ax)
ax.set_title("Title Here", fontsize=14, fontweight="bold")
ax.set_xlabel("Category")
ax.set_ylabel("Value (units)")
sns.despine()
plt.tight_layout()
plt.show()
```

### Python – altair scatter
```python
import altair as alt

chart = alt.Chart(df).mark_point().encode(
    x=alt.X("x_col:Q", title="X Label"),
    y=alt.Y("y_col:Q", title="Y Label"),
    color=alt.Color("category:N"),
    tooltip=["x_col", "y_col", "category"]
).properties(
    title="Chart Title",
    width=500, height=350
).interactive()
chart
```

### R – ggplot2 line chart
```r
library(ggplot2)

ggplot(df, aes(x = date, y = value, color = group)) +
  geom_line(linewidth = 1) +
  geom_point(size = 2) +
  scale_color_brewer(palette = "Set1") +
  labs(
    title = "Title Here",
    x = "Date", y = "Value (units)",
    color = "Group", caption = "Source: ..."
  ) +
  theme_minimal() +
  theme(legend.position = "bottom")
```

---

## 8. Evaluation Checklist

Before presenting a visualization, verify:

- [ ] Chart type matches the data relationship (§2)
- [ ] Most important variable encoded with position
- [ ] Color palette is colorblind-friendly
- [ ] Axes start at zero (bar charts) or are clearly justified
- [ ] Title describes the finding, not just the data ("Sales rose 40% in Q3" > "Sales by Quarter")
- [ ] No unnecessary chart elements (gridlines, borders, legends if labels exist)
- [ ] Uncertainty shown if the data warrants it
- [ ] Works at small size (thumbnail legibility)

---

## 9. Reference Files

Load these files when you need more detail:

- `references/chart_types.md` — Deep guide for 25+ chart types with SVG patterns, use cases, pitfalls
- `references/color_palettes.md` — Hex codes for ColorBrewer, HUSL, seaborn palettes; colorblind-safe combinations
- `references/libraries.md` — Quick-start syntax for ggplot2, seaborn, altair, matplotlib, Vega-Lite, D3

Load the relevant reference file when:
- The user asks for a less common chart type (Sankey, ridgeline, waffle, etc.)
- Color selection is central to the task
- The user wants syntax for a specific library not shown in §7