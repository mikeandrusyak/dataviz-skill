# Libraries Quick-Start Reference

Syntax cheatsheets for the most common visualization libraries.

---

## Python: seaborn

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_theme(style="whitegrid", palette="colorblind")

# Bar chart
sns.barplot(data=df, x="cat", y="val", errorbar=None)

# Line chart
sns.lineplot(data=df, x="date", y="val", hue="group")

# Scatter
sns.scatterplot(data=df, x="x", y="y", hue="cat", size="weight")

# Histogram
sns.histplot(data=df, x="val", bins=30, kde=True)

# Box plot
sns.boxplot(data=df, x="group", y="val")

# Violin
sns.violinplot(data=df, x="group", y="val", inner="quart")

# Heatmap
sns.heatmap(corr_matrix, annot=True, cmap="Blues", fmt=".2f")

# Facets
g = sns.FacetGrid(df, col="category", col_wrap=3)
g.map(sns.histplot, "value")

# Finishing touches
plt.title("Title", fontsize=14, fontweight="bold")
plt.xlabel("X label (units)")
plt.ylabel("Y label (units)")
sns.despine()
plt.tight_layout()
```

---

## Python: altair (Vega-Lite)

```python
import altair as alt

# Base chart
base = alt.Chart(df)

# Bar
base.mark_bar().encode(
    x=alt.X("category:N", sort="-y"),
    y=alt.Y("value:Q"),
    color="group:N",
    tooltip=["category", "value"]
).properties(title="Title", width=500, height=300)

# Line
base.mark_line(point=True).encode(
    x="date:T",
    y="value:Q",
    color="group:N"
).interactive()

# Scatter
base.mark_point().encode(
    x="x:Q", y="y:Q",
    color="cat:N", size="weight:Q",
    tooltip=alt.Tooltip(["x", "y", "cat"])
)

# Histogram
base.mark_bar().encode(
    alt.X("value:Q", bin=alt.Bin(maxbins=30)),
    y="count()"
)

# Heatmap
base.mark_rect().encode(
    x="col_var:O",
    y="row_var:O",
    color=alt.Color("value:Q", scale=alt.Scale(scheme="blues"))
)

# Facets
chart.facet(facet="category:N", columns=3)

# Layering
alt.layer(line_chart, scatter_chart)

# Concatenation
alt.hconcat(chart1, chart2)
```

---

## Python: matplotlib

```python
import matplotlib.pyplot as plt
import matplotlib as mpl

fig, ax = plt.subplots(figsize=(8, 5))

# Bar
ax.bar(x, height, color="#0173B2", edgecolor="white")

# Line
ax.plot(x, y, linewidth=2, color="#0173B2", label="Group A")
ax.fill_between(x, y_low, y_high, alpha=0.2)  # confidence band

# Scatter
ax.scatter(x, y, c=color_vals, cmap="viridis", s=size_vals, alpha=0.7)

# Error bars
ax.errorbar(x, y, yerr=err, fmt="o", capsize=5)

# Subplots
fig, axes = plt.subplots(2, 3, figsize=(12, 8), sharex=True, sharey=True)

# Styling
ax.set_title("Title", fontsize=14, fontweight="bold")
ax.set_xlabel("X (units)", fontsize=11)
ax.set_ylabel("Y (units)", fontsize=11)
ax.tick_params(colors="#555555")
ax.spines[["top", "right"]].set_visible(False)
ax.grid(axis="y", alpha=0.3)
ax.legend(frameon=False)
plt.tight_layout()
```

---

## R: ggplot2

```r
library(ggplot2)
library(dplyr)

# Core structure: ggplot(data, aes(...)) + geom_*() + scale_*() + theme_*()

# Bar chart
ggplot(df, aes(x = reorder(category, value), y = value, fill = group)) +
  geom_col() +
  coord_flip() +
  scale_fill_brewer(palette = "Set2") +
  labs(title = "Title", x = NULL, y = "Value (units)") +
  theme_minimal()

# Line chart
ggplot(df, aes(x = date, y = value, color = group)) +
  geom_line(linewidth = 1) +
  geom_ribbon(aes(ymin = lower, ymax = upper, fill = group), alpha = 0.2) +
  scale_color_brewer(palette = "Set1") +
  theme_minimal() +
  theme(legend.position = "bottom")

# Scatter + trend
ggplot(df, aes(x = x, y = y, color = category)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm", se = TRUE) +
  scale_color_viridis_d() +
  theme_minimal()

# Histogram + density
ggplot(df, aes(x = value, fill = group)) +
  geom_histogram(aes(y = after_stat(density)), bins = 30, alpha = 0.5, position = "identity") +
  geom_density(aes(color = group), linewidth = 1) +
  theme_minimal()

# Box / violin
ggplot(df, aes(x = group, y = value)) +
  geom_violin(fill = "lightblue", alpha = 0.6) +
  geom_boxplot(width = 0.1) +
  theme_minimal()

# Heatmap
ggplot(df, aes(x = col_var, y = row_var, fill = value)) +
  geom_tile() +
  scale_fill_distiller(palette = "Blues", direction = 1) +
  geom_text(aes(label = round(value, 2)), size = 3) +
  theme_minimal()

# Facets
ggplot(df, aes(x = x, y = y)) +
  geom_point() +
  facet_wrap(~ category, scales = "free")

# Save
ggsave("output.png", dpi = 300, width = 8, height = 5)
```

---

## Vega-Lite (JSON / standalone)

```json
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "data": {"url": "data.csv"},
  "mark": "bar",
  "encoding": {
    "x": {"field": "category", "type": "nominal", "sort": "-y"},
    "y": {"field": "value", "type": "quantitative"},
    "color": {"field": "group", "type": "nominal", "scale": {"scheme": "tableau10"}},
    "tooltip": [{"field": "category"}, {"field": "value"}]
  },
  "title": "Chart Title",
  "width": 500,
  "height": 300
}
```

---

## Interactive SVG/HTML in Chat (show_widget)

When building inline artifacts, prefer clean SVG with D3-style coordinate math or pure HTML+CSS for simple charts. Example structure:

```html
<svg viewBox="0 0 600 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Axes -->
  <line x1="60" y1="350" x2="560" y2="350" stroke="#ccc"/>
  <line x1="60" y1="50" x2="60" y2="350" stroke="#ccc"/>
  <!-- Bars / data elements -->
  <!-- Labels -->
  <!-- Title -->
  <text x="300" y="30" text-anchor="middle" font-size="16" font-weight="bold">Title</text>
</svg>
```

Use CSS variables for colors to respect the app's theme.