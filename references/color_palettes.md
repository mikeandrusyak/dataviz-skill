# Color Palettes Reference

Ready-to-use palettes for data visualization. All palettes are colorblind-tested.

---

## Qualitative (Categorical) Palettes

### Colorblind-safe (seaborn default)
```python
# seaborn
palette = "colorblind"
# Hex values:
colors = ["#0173B2", "#DE8F05", "#029E73", "#D55E00", "#CC78BC",
          "#CA9161", "#FBAFE4", "#949494", "#ECE133", "#56B4E9"]
```

### Tableau 10
```
#4E79A7  #F28E2B  #E15759  #76B7B2  #59A14F
#EDC948  #B07AA1  #FF9DA7  #9C755F  #BAB0AC
```

### ColorBrewer Set2 (soft, good for academic)
```
#66C2A5  #FC8D62  #8DA0CB  #E78AC3  #A6D854  #FFD92F  #E5C494  #B3B3B3
```

### For 3–4 categories
```
#1f77b4  #ff7f0e  #2ca02c  #d62728
```

---

## Sequential Palettes (ordered/magnitude data)

### Blues (light → dark)
```
#EFF3FF  #BDD7E7  #6BAED6  #2171B5  #084594
```

### Greens
```
#EDF8E9  #BAE4B3  #74C476  #31A354  #006D2C
```

### Oranges
```
#FEEDDE  #FDBE85  #FD8D3C  #E6550D  #A63603
```

### Viridis (perceptually uniform, colorblind-safe)
```python
import matplotlib.cm as cm
colors = cm.viridis  # or 'plasma', 'inferno', 'magma'
```

---

## Diverging Palettes (deviation from a midpoint)

### Red–Blue (RdBu)
```
#B2182B  #D6604D  #F4A582  #FDDBC7  #F7F7F7  #D1E5F0  #92C5DE  #4393C3  #2166AC
```

### Red–Green (colorblind-UNSAFE — avoid)
```
# Do NOT use red/green — ~8% of population has red-green color blindness
```

### Purple–Orange (colorblind-safe alternative)
```
#7B3294  #C2A5CF  #F7F7F7  #A6DBA0  #008837
```

---

## HUSL Color Space

HUSL provides perceptually uniform hues — useful when generating custom palettes.
```python
import seaborn as sns
palette = sns.husl_palette(n_colors=6)
```

Website: https://husl-colors.org/

---

## Colorblind Simulation

Test your palette at:
- **Coblis**: https://www.color-blindness.com/coblis-color-blindness-simulator/
- **Viz Palette**: https://projects.susielu.com/viz-palette
- **Adobe Color**: https://color.adobe.com/create/color-accessibility

Most common impairments:
- **Deuteranopia** (green-blind): ~6% of men
- **Protanopia** (red-blind): ~1% of men
- Both confuse red and green → use blue/orange instead

---

## R Quick Reference

```r
# ColorBrewer in ggplot2
scale_color_brewer(palette = "Set2")
scale_fill_brewer(palette = "Blues")

# Viridis
scale_color_viridis_c()   # continuous
scale_color_viridis_d()   # discrete

# Custom
scale_color_manual(values = c("#0173B2", "#DE8F05", "#029E73"))
```

## Python Quick Reference

```python
# seaborn
sns.set_palette("colorblind")
sns.color_palette("Blues", as_cmap=True)   # sequential

# matplotlib
plt.set_cmap("viridis")

# altair
alt.Color("col:N", scale=alt.Scale(scheme="tableau10"))
alt.Color("col:Q", scale=alt.Scale(scheme="blues"))
```