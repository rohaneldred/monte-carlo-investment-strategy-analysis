# monte-carlo-investment-strategy-analysis
Overview

- I modelled a fictional pension company with an initial asset pool and yearly liability payments. I used Monte Carlo simulation to estimate the probabilities of bankruptcy and negative surplus for different investment strategies. I then changed variables, such as initial assets and liability discount rate to investigate their effect on optimal investment strategy.

Modelling and methodology

- I modelled this pension company as having:
  - Initial assets A_0
  - Liability payments L at the end of each year over N-year long investment futures (so at 1 year, 2 years, 3 years, .... ,     N-1 years,  N years after the start of each investment future)
  - A proportion of assets w remaining at the end of each year that are invested in equities (where the remaining proportion     of the assets, 1-w, are invested in bonds)
  - Bonds having a yearly return given by a normal distribution with mean mu_B and standard deviation sigma_B
  - Equities having a yearly return given by a shifted lognormal distribution due to geometric Brownian motion. This             lognormal distribution inputs the standard normal distribution Z (mean 0, variance 1), the annual drift parameter mu_E       and annual volatility parameter sigma_E. 
  - A liability discount rate r. This is the yearly fractional increase in the amount of money that would be worth the same      amount (e.g. for my initial value of r, r = 0.04, this means £100 now would have the same value as £104 in a year). This     means, since the yearly liability payments are constant (in terms of £), at a given time future liabilities are worth        less than present liabilities.
- N, L, mu_B, mu_E, sigma_B, sigma_E were kept constant. Later on, w, A_0 and r were changed. Here are the initial values used of all of these quantities:
  - N = 20
  - A_0 = £80,000,000
  - L = £5,000,000
  - w = 0.2
  - mu_B = 0.035
  - sigma_B = 0.05
  - mu_E = 0.07
  - sigma_E = 0.18
  - r = 0.04
- In other words, each year the assets were multiplied by 1 + the overall return (calculated from the equity and bond returns), then L was taken off. The surplus for a given year was calculated by subtracting the sum of the present values of all future liabilities payments from the calculated assets value for that year.  
- In some investment futures, after a few years with low or negative returns, the assets became less than L before L was taken off. If this happened, the assets were treated as being 0 for that year and all subsequent years, and the surplus for each subsequent year was just calculated using the formula:
- Surplus = -(Number of £ of past unpaid payments (not taking into account present value) + present value of future payments)

Plotting a graph of investment futures and undertaking Monte Carlo simulation

- I plotted a graph of the trajectory of the assets and surplus for M = 30 investment futures. A few of them ended up with zero assets (bankruptcy). Among the investment futures with negative surplus at some point, some of them involved recovery without the company going bankrupt, while others involved bankruptcy.
- I then used Monte Carlo simulation to simulate M = 100,000 investment futures. I estimated the probability of bankruptcy to be 0.10046 and the probability of negative surplus at some point in the investment future to be 0.16601 (this is greater than the probability of bankruptcy since some investment futures had negative surplus at some point, but later recovered and never had negative assets). The investment futures with bankruptcy are a subset of the investment futures with negative surplus at some point.

Changing w

- I calculated Monte Carlo estimated probabilities of bankruptcy and negative surplus for 100 evenly spaced w values from 0 to 1, then plotted these probabilities against w, one graph for bankruptcy probability and one graph for negative surplus probability. I then plotted a graph of probability of bankruptcy against w for 100 evenly spaced w values in the 0.15<=w<=0.25 range and another graph of the probability of negative surplus against w for 100 evenly spaced w values the 0.1<=w<=0.2 range since these ranges were where the minimum probabilities of bankruptcy and negative surplus were, respectively. By plotting these graphs, I could see more accurately the optimal value of w for reducing the probabilities of bankruptcy (around 0.2), or negative surplus (around 0.15). With every w value, the same N by M array of randomly-generated yearly equity and bond returns were used (so the same set of 100,000 20-year market scenarios were simulated every time) for efficiency and to reduce noise in the graphs.

Changing A_0 and r

- I then plotted the optimal values of w for reducing bankruptcy and negative surplus (the values of w with the minimum probabilities of bankruptcy and negative surplus) against a changing A_0 (ranging from £75,000,000 to £85,000,000) while holding everything else constant. For higher A_0, the optimal w value for reducing the probabilities of bankruptcy and negative surplus decreased, probably because lower returns would have been needed to avoid bankruptcy or negative surplus, making bonds more favourable despite their lower returns (since the high risk in equities makes it less favourable). The decrease in optimal w for a given increase in A_0 seemed to be less in both graphs for higher A_0 than for lower A_0, although this was hard to tell since both graphs were relatively noisy. 
- Afterwards, I set A_0 back to £80,000,000 and plotted the optimal values of w for reducing negative surplus against a changing r (ranging from 0.03 to 0.05). Changing r does not affect the probability of bankruptcy since it only affects the surplus (not assets) values. Perhaps surprisingly, higher r values increased optimal w value for reducing negative surplus, possibly because lower r (and increased value of future liabilities) would have decreased surplus, possibly making it favourable to invest less in equities and avoid large losses (which are more likely when equity allocation is high). However, it was slightly hard to tell the exact trend since the graph appeared quite noisy. In both the changing A_0 and r experiments, 100 values of w are being considered to determine the best of them, with the range for w being chosen to ensure that the every optimal w value plotted will be within this range (from previously plotting these graphs with w ranging from 0 to 1, then seeing which range the optimal w values are in over the graphs)

Limitations

- My project has many limitations, some of them being:
  - This fictional company isn't entirely representative of a real company. A real company would probably having additional      assets coming in and varying liabilities throughout each year, not just constant liabilities at the end of each year.
  - Only a finite (100,000) number of simulations were used each time, creating a small uncertainty in the probabilities of      negative surplus and bankruptcy.
  - Only 100 values of w were considered when calculating optimal w value, and only 100 values of A_0 and r were considered      when these quantities were changed, decreasing the smoothness of the graphs
  - Since the w values with minimum probabilities of bankruptcy/negative surplus were being considered, nearby w values will     have similar probabilities since the gradient of a graph near a minimum point is small. This means even though there is      a small uncertainty in Monte Carlo probability estimations, this can lead to a large uncertainty in optimal w value for      each A_0 or r, which is the main reason the graphs of optimal w against A_0/r were noisy. 
