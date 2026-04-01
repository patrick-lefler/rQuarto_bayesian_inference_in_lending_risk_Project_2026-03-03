# [Title]

**Author:** Patrick Lefler  
**Published:** 2026-03-03 
**Project 3:** Project-09
**Tools:** R | Quarto | Plotly | ggplot2 | kableExtra

---

## Overview

Traditional lending risk models rely on frequentist point estimates and p-values to assess borrower risk. This method simplifies parameter uncertainty into a binary decision. It often misses the distributional information that credit decision-makers need. When a pricing committee wonders if the debt-to-income ratio impacts interest rates, a p-value below 0.05 responds to a question that wasn't even asked. It overlooks the more relevant question: how large is the effect, and how confident can we be in that size? </br> 

This project reframes two key lending risk questions using a Bayesian approach. It treats parameter uncertainty as an important output, not just a nuisance. Two Bayesian regression models are fitted using the `loans_full_schema` dataset from the `openintro` package with `rstanarm`. The first model estimates the relationship between debt-to-income ratio and interest rate. The second tests if homeownership status creates significant rate differences among borrower segments. </br> 

The posterior distributions are characterized with <code> bayestestR</code>, using three diagnostic tools. First, the 89% Highest Density Interval shows parameter-level uncertainty. Second, the Region of Practical Equivalence (ROPE) checks if effects fall within a range that isn’t very important. Third, the Probability of Direction (pd) is a simple tool for measuring directional confidence. These tools replace the single-threshold logic of null hypothesis testing with a more nuanced view of the data. </br> 

The results show that Bayesian summaries reveal important distinctions that frequentist outputs miss. The debt-to-income ratio consistently relates positively to interest rates. The width of the credible interval and the partial ROPE overlap show that there's significant uncertainty in the effect size. This nuance is vital for loan pricing functions. Homeownership differences show that a high Probability of Direction can happen even with small effect sizes. This challenges the usual link between statistical significance and actionability.

---

## Key insights
The Bayesian framing solves the right problem for credit practitioners. The abstract and conclusion nail the core argument: a p-value answers a question no pricing committee ever asked, while a posterior distribution directly quantifies what they actually need — the plausible range and magnitude of an effect given observed data. The DTI model's 89% HDI sitting entirely outside the ROPE makes this concrete, not just philosophical. 

The OWN-vs-MORTGAGE finding is the project's sharpest analytical moment. Naively, zero housing debt should signal the best credit profile, but OWN borrowers land between RENT and MORTGAGE on rates — and you call this out explicitly. The 98% ROPE overlap and pd of only 0.95 for OWN vs. MORTGAGE means the effect is directionally credible but practically negligible, which is exactly the kind of nuance that a binary significance test would flatten into "not significant" and walk away from.

The three-tool diagnostic stack (HDI, ROPE, pd) is well-chosen and well-sequenced. Each tool answers a distinct question — where does the parameter sit, does the size matter for business, and how confident are we in the sign — and the describe_posterior() unified summary ties them together cleanly. The pedagogical progression from individual diagnostics to the unified table is effective for a portfolio-level audience that needs to see both the reasoning and the bottom line.

One structural gap: the models are univariate by design, and acknowledging that more explicitly would strengthen the conclusions. DTI and homeownership are almost certainly correlated (mortgage holders carry housing debt that inflates DTI), so the isolated effects could shift materially in a multivariate specification. A brief note framing these as marginal-association baselines rather than causal estimates — or flagging a multivariate extension as future work — would preempt the most obvious methodological pushback from a sophisticated reader.

---

## Repository structure

```
├── bayesian_lending_risk.qmd   # Main Quarto source document
├── bayesian_lending_risk.html  # Rendered HTML output
|-- brand.yml  #quarto branding document
|-- data
    |-- loans_full_schema.csv   # downloaded data
└── README.md
```

---

## Reproducing the analysis

### Prerequisites

R 4.3 or later with the following packages:

```r
library(bayestestR)  # Posterior description & hypothesis testing
library(ggplot2)     # Visualisation
library(insight)     # Uniform parameter extraction
library(kableExtra)  # kable table
library(openintro)   # Source of loans_full_schema
library(rstanarm)    # Stan-backed Bayesian regression
library(scales)  # decimal formating
library(sessioninfo) # session information
library(tidyr)       # Reshaping
library(tidyverse)   # Data wrangling
library(dplyr)       # Data wrangling
```

Quarto 1.4 or later. Install from [quarto.org](https://quarto.org).

## Design standards

- **Theme:** Quarto Sandstone (Bootswatch)
- **Brand:** Custom palette defined in `_brand.yml` — off-white background, dark grey text, blue/red/green accent ramps
- **Typography:** Roboto (Google Fonts, via `_brand.yml`)
- **Visualizations:** `ggplotly()` for density overlays, `plot_ly()` / `add_bars()` for Kelly histograms and trajectory panels
- **Tables:** `kbl()` + `kable_styling()` with striped, hover, condensed, responsive bootstrap options
- **No Shiny, no OJS** — document renders to a standalone HTML file that works from the filesystem or any web host

## Related projects

None

---

## License

MIT License. You are free to use, adapt, and republish this analysis with attribution.

---

## References

Bray, A., Çetinkaya-Rundel, M., & Hardin, J. (2023). *openintro: Data sets
and supplemental functions from OpenIntro textbooks*. R package.
https://CRAN.R-project.org/package=openintro

Cohen, J. (1988). *Statistical power analysis for the social sciences* (2nd
ed.). Lawrence Erlbaum.

Goodrich, B., Gabry, J., Ali, I., & Brilleman, S. (2023). *rstanarm: Bayesian
applied regression modeling via Stan*. R package.
https://mc-stan.org/rstanarm/

Kruschke, J. (2014). *Doing Bayesian data analysis: A tutorial with R, JAGS,
and Stan* (2nd ed.). Academic Press.

Makowski, D., Ben-Shachar, M. S., & Lüdecke, D. (2019). bayestestR:
Describing effects and their uncertainty, existence and significance within
the Bayesian framework. *Journal of Open Source Software, 4*(40), 1541.
https://doi.org/10.21105/joss.01541

McElreath, R. (2018). *Statistical rethinking: A Bayesian course with examples
in R and Stan*. Chapman & Hall/CRC.

---

*Built with [Quarto](https://quarto.org/) · [R](https://www.r-project.org/) ·
[Stan](https://mc-stan.org/)*
