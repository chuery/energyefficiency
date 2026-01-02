# Opower Energy Efficiency Behavioral Program
# Summary 
The Opower behavioral energy efficiency program proves to be an effective strategy that encourages households to reduce electricity consumption by leveraging social comparison and behavioral motivation—without costly infrastructure upgrades. Using detailed data from approximately 80,000 households over a 26-month period (July 2007–November 2010), this analysis quantifies the program’s impact on electricity use and assesses its overall financial and environmental benefits. Key findings include:
Electricity Reduction: The program reduces average household electricity consumption by about 0.5 kWh per day, amounting to 390 kWh saved per household over the study duration.
Robust Impact Estimates: Advanced statistical models controlling for household and temporal factors confirm that these savings are attributable to the Opower reports, demonstrating a clear causal effect.
Economic Benefits: At a cost of approximately $1 per household per month, the program yields substantial net benefits when accounting for reduced energy costs and the social cost of carbon. 
Our cost-benefit analysis of the Opower energy efficiency program demonstrates clear net benefits across the United States, Massachusetts, and California, despite differences in regional energy mixes. Assuming a program cost of $1 per household per month over 26 months and a social cost of carbon of $47.17 per tonne, the program’s average household energy savings of 0.5 kWh per day translate into substantial monetary and environmental gains. Regions with natural gas dominant energy sourcing, such as Massachusetts, realize the greatest net benefits, while the national average and California (with its cleaner energy mix) also show strong positive returns. 
These results underscore the program’s cost-effectiveness and scalability, especially when considering regional variations in electricity prices and emissions. Implementing or expanding behavioral energy efficiency programs like Opower can reduce demand, lower customer bills, and contribute meaningfully to emission reduction targets—all at a fraction of the cost of traditional efficiency investments. This aligns closely with both economic and sustainability goals, enhancing your company’s competitive positioning and regulatory compliance.
# Data Cleaning
Our analysis relies on two core datasets: household-level demographic and structural information and monthly energy usage data. The household data includes variables describing house characteristics, occupant characteristics, and program-specific variables. The energy data contains monthly data from July 2007 to November 2010 on electricity usage (in kWh/day), cooling degree days (CDD), and heating degree days (HDD) for approximately 80,000 households.
We begin by merging the energy and household datasets using the common `id` variable, ensuring that key household characteristics and treatment group indicators are retained in the combined dataset. Next, we generate a `post` indicator variable, which takes the value 1 for observations from October 2008 onward—the month when the first Opower home energy reports were distributed—and 0 otherwise. We also construct a `date` variable by converting the year and month columns into a standard datetime format to facilitate time-series analysis.
This cleaned and merged dataset forms the basis for all subsequent modeling and econometric analysis, allowing us to examine treatment effects while controlling for both time-varying weather patterns and fixed household characteristics.
# Data Analysis 
Figure 1: Average electricity use by month of sample (Treatment vs. Control groups)
<img width="512" height="292" alt="image" src="https://github.com/user-attachments/assets/77534878-fb8b-467e-9b7a-27b5ad231cdf" />

In accordance with the parallel trends assumption, we observe that prior to the introduction of the Opower reports in October 2008, average energy usage for both the treatment and control groups follows a similar trajectory. This suggests that, in the absence of treatment, the groups would have continued to evolve in parallel—supporting the validity of a difference-in-differences (DiD) approach. Following the launch of the home energy reports, we begin to see a clear divergence: households in the treatment group exhibit a noticeable and persistent reduction in daily electricity usage relative to the control group. This post-treatment difference provides preliminary evidence that the Opower reports had a measurable impact on household energy consumption behavior. 
Figure 2: Difference in electricity use by month of sample (Treatment vs. Control groups)

<img width="512" height="316" alt="image" src="https://github.com/user-attachments/assets/6dfc6746-52e4-4da2-a0c6-e84f18208940" />


After the Opower reports were introduced, the estimated treatment effects declined below zero, and the corresponding 95% confidence intervals no longer crossed zero. This suggests a statistically significant decrease in energy consumption due to the treatment, with the impact growing stronger over time.
For this analysis, we employed panel data methods, which leverage repeated observations of the same units—households in this case—over time. Unlike cross-sectional data, which captures each unit only once, panel data allows us to track changes within units, improving our ability to control for unobserved heterogeneity. We begin with pooled Ordinary Least Squares (OLS), which estimates a single best-fit line across all household-month observations, without accounting for unit-specific or time-specific effects. While simple, this approach assumes no unobserved household characteristics influence energy use, a limitation addressed in more advanced panel techniques.
Table 1: Pooled OLS difference-in-difference estimate of the program’s effect on electricity use
<img width="418" height="304" alt="Screenshot 2026-01-01 at 9 26 36 PM" src="https://github.com/user-attachments/assets/3344a569-180d-46a8-80a3-d27854a80eb4" />

In our pooled OLS analysis, the intercept captures the average energy usage of all households in the pre-treatment period, serving as the baseline. The coefficient on the treatment group indicator (T) reflects the average difference in energy usage between treated and untreated households, regardless of time. The Post variable estimates how much energy usage changed for all households in the post-treatment period, relative to the pre-treatment period. The interaction term (T:Post) captures the average treatment effect of the Opower reports—how much more (or less) treated households reduced their energy usage compared to untreated households in the post period, after controlling for overall time and group differences. When including entity (household) fixed effects and time fixed effects, we control for unobserved household-level characteristics and common temporal shocks. In our results, the entity effects average to approximately 0.0213, while the time effects average to –0.3136, capturing baseline variation across households and time periods, respectively.
To account for unobserved heterogeneity across households and time, we use a two-way fixed effects (TWFE) model, also known as the within estimator. This method computes separate best-fit lines for each unit and averages the resulting slopes to isolate the treatment effect. It controls for unit fixed effects (e.g., household-specific traits that are constant over time, such as insulation quality or behavioral tendencies) and time fixed effects (e.g., month- or year-specific shocks like weather anomalies or policy changes that affect all households simultaneously). The fixed effects absorb all unobserved, time-invariant characteristics of households and common temporal influences, allowing us to focus solely on within-household changes over time. The residual error term captures idiosyncratic shocks that vary both across households and over time. By removing between-unit variation, the fixed effects estimator helps isolate the causal effect of the Opower reports on energy usage. TWFE analysis is done with the following equation: 
$$Y_{i, i-1} = i + X_{it}$$
$$\alpha_i$$ : individual characteristics/entity effects for each household
$$\lambda_i$$ : time effects 
$$\X_{it}$$ : indicator variable for T 
$$\beta$$: coefficient of interest 
In the panel data setting, we compute clustered standard errors to account for serial correlation within households over time. While the sample includes many repeated observations per household, these observations are not truly independent, as required by traditional OLS assumptions. Without adjustment, standard errors would be underestimated, especially as the number of time periods grows, misleadingly suggesting greater precision. This approach effectively reduces the sample size, yielding more conservative and reliable standard error estimates and ensuring the validity of inference in the presence of repeated measurements. 

Table 2: Two-way fixed effect estimate of the program’s effect on electricity use
<img width="497" height="386" alt="Screenshot 2026-01-01 at 9 29 52 PM" src="https://github.com/user-attachments/assets/8b2093ba-7226-4b62-b010-0412a43f98c1" />

While the fixed effects model explicitly absorbs both household (entity) and time effects, the results are remarkably consistent with those from the simpler pooled OLS specification. This suggests that unobserved heterogeneity across households and common shocks over time do not strongly bias the pooled estimates in this setting. Both models yield a similar estimate of the treatment effect, with the fixed effects model indicating a reduction of approximately 0.517 kWh/day in electricity usage among treated households—consistent with the core findings of the analysis.

Figure 3: Event study plot 
<img width="567" height="334" alt="Screenshot 2026-01-01 at 9 30 28 PM" src="https://github.com/user-attachments/assets/742c7b3a-0e91-4baa-bae1-3405e678c147" />

In the event study plot, the treatment group initially exhibits slightly higher energy usage than the control group; however, these pre-treatment differences are not statistically significant, as the 95% confidence intervals intersect with zero. This supports the parallel trends assumption. Following the introduction of the Opower reports, the estimated treatment effects drop below zero, and the confidence intervals no longer include zero. This indicates a statistically significant reduction in energy usage attributable to the treatment, with the effect becoming more pronounced over time.

# Benefit-cost Analysis 
We conducted a cost-benefit analysis of the Opower program for the US, Massachusetts, and California, recognizing that regional differences in energy source compositions affect both the private cost of electricity production and carbon emissions. Assuming a treatment cost of $1 per household per month over 26 months and using a social cost of carbon of $47.17 per metric tonne (per economist Gilbert Metcalfe), we calculated the total costs and benefits per household. The program reduces daily electricity usage by about 0.5 kWh, which translates into carbon emissions reductions weighted by each region’s energy mix carbon intensity. These reductions, valued at the social cost of carbon, combined with savings from reduced electricity consumption, were compared against program costs. By focusing on individual households over the full program duration, this analysis highlights how differences in energy generation and policy across regions influence the net benefits of behavioral energy efficiency interventions like Opower.

| Energy Source | Carbon Emissions (tCO₂e/kWh) |
| :--- | :---: |
| Coal | 0.001048 |
| Natural Gas | 0.000435 |
| Nuclear | 0.000012 |
| Hydropower | 0.000024 |
| Solar | 0.000050 |
| Wind | 0.000007 |

Source: EIA, World Nuclear Association 
The cost of energy production by source is based on the Levelized Cost of Electricity (LCOE), a metric that assesses the lifetime cost of generating electricity from a specific power source. This calculation is performed separately for each region, reflecting differences in available technologies and local factors that affect energy production costs.

**Table 4:** Energy production costs and carbon emissions for general United States by energy source
| Energy Source | Cost ($/kWh) | Emissions (tCO₂e/kWh) | Share of Gen. |
| :--- | :---: | :---: | :---: |
| Natural Gas | 0.130 | 0.000435 | 42.5% |
| Coal | 0.150 | 0.001048 | 14.9% |
| Nuclear | 0.090 | 0.000012 | 17.8% |
| Wind | 0.092 | 0.000007 | 10.3% |
| Hydropower | 0.090 | 0.000024 | 5.5% |
| Solar | 0.146 | 0.000050 | 6.9% |

Source: EIA

United States Weighted Average Cost C: $0.118 per kWh
United States Weighted Average Emissions  E: 0.0003457 tonnes CO₂e/kWh

**Table 5:** Energy production costs and carbon emissions for Massachusetts by energy source
| Energy Source | Cost ($/kWh) | Emissions (tCO₂e/kWh) | Share of Gen. |
| :--- | :---: | :---: | :---: |
| Natural Gas | 0.167 | 0.000490 | 74.7% |
| Solar | 0.146 | 0.000048 | 10.9% |
| Biomass | 0.178 | 0.000230 | 4.4% |
| Hydroelectric | 0.090 | 0.000024 | 3.8% |
| Wind | 0.092 | 0.000012 | 0.9% |
| Petroleum | 0.167 | 0.000490 | 0.9% |
| Other | 0.090 | 0.000024 | 4.5% |

Source: EIA

**Table 6:** Energy production costs and carbon emissions for California by energy source
| Energy Source | Cost ($/kWh) | Emissions (tCO₂e/kWh) | % in CA (2023) |
| :--- | :---: | :---: | :---: |
| Natural Gas (Combined Cycle) | 0.167 | 0.000490 | 43.6% |
| Solar PV (Utility-scale) | 0.146 | 0.000048 | 18.4% |
| Hydroelectric | 0.090 | 0.000024 | 12.5% |
| Nuclear | 0.090 | 0.000012 | 8.2% |
| Wind (Onshore) | 0.092 | 0.000012 | 6.4% |
| Geothermal | 0.185 | 0.000079 | 5.1% |
| Biomass | 0.178 | 0.000230 | 2.4% |

Source: EIA
California Weighted Average Cost C: $0.138 (13.80¢/kWh)
California Weighted Average Emissions E:  0.000237 tCO₂e/kWh

**Table 7:** Benefit-cost analysis of Opower program for California, United States, Massachusetts per household over 26-month treatment duration 
Metric
| Metric | California | United States (Avg) | Massachusetts |
| :--- | :---: | :---: | :---: |
| Electricity price | $0.138/kWh | $0.118/kWh | $0.158/kWh |
| Emissions factor (tCO₂e/kWh) | 0.000237 | 0.000346 | 0.000387 |
| Energy saved (kWh) | 390 | 390 | 390 |
| Value of energy saved | $53.82 | $46.02 | $61.62 |
| Emissions reduced (tCO₂e) | 0.09243 | 0.13482 | 0.15097 |
| Value of emissions reduction | $4.36 | $6.36 | $7.12 |
| Total benefits | $58.18 | $52.38 | $68.74 |
| Program cost | $26.00 | $26.00 | $26.00 |
| **Net benefit** | **$32.18** | **$26.38** | **$42.74** |

California’s lower carbon intensity (~0.000237 tCO₂e/kWh) and moderate electricity cost ($0.138/kWh) result in a total benefit of approximately $58.18 per household, with net benefits of $32.18 after program costs. Massachusetts, with a higher electricity price ($0.158/kWh) and carbon intensity (~0.000387 tCO₂e/kWh), shows the largest net benefit of $42.74 per household. The national average falls between these values with $26.38 net benefit. This analysis highlights that regional differences in energy sourcing and pricing significantly influence the environmental and economic returns of behavioral energy efficiency programs like Opower.

# Conclusion
Our analysis demonstrates that the effectiveness and economic value of the Opower behavioral energy efficiency program vary significantly across regions due to differences in energy source composition, electricity pricing, and carbon intensity. California’s relatively low carbon intensity and moderate electricity costs yield meaningful net benefits per household, while Massachusetts’ higher energy prices and emissions result in even greater net gains. The national average benefits fall between these extremes. These findings underscore the importance of tailoring energy efficiency interventions to local energy profiles to maximize environmental and financial impact.
However, the use of fixed effects models introduces some limitations. By removing all time-invariant variation within units, fixed effects may also eliminate meaningful differences, potentially leaving mostly measurement error to explain remaining variation. Additionally, our analysis focuses primarily on short-to-medium term effects observed over the 26-month program period. Long-run impacts, which may differ substantially, require further study, especially when comparing day-to-day savings with broader, structural changes across different communities.
