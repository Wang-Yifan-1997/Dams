dams to do

data:
dam registry: location, year, size, function (detailed informaiton for one province, need to match with hydropower station with other data); follow up investment (need to search online, not sure if this is useful)
manufacturing firm (revenue, tfp, patents (maybe less useful), pollution, energy consumption, firm entry, industry code)
county level: statistics yearbook, nighttime light, migration
water monitor station and water flow data (we need to have a think what can we do withi this data)

descriptive statistics: where are the dams, years, size, types
we need both maps, figures (bar charts), and tables (mean, median, min, max, std, p25, p75)
replicate all the above analysis with zhejiang province and all other province

Question 1: what's the role of dams.

DID specification:
1. from zhejiang province: flood control (43); electricity (109); water supply (303); irrigation (440); transportation (2); aquaculture (122); others (22)
2. county level DID: test the impact of dams on firm entry, revenue, tfp (labor productivity for ASIE), pollution for the building of new dams. let's say we focus on a specific period of time (1998-2020) where most of the outcome can be observed
3. treatment = 1 if new dam > 0 after time t
4. treatment = 1 if a certain type of new dam > 0 after time t. we might see bigger dams has higher impact 
5. treatment = 1 if a certain size of new dam > 0 after time t
6. treatment = 1 if a certain size & type of new dam > 0 after time t
7. controlling the existing number of dams before the time window (before 1998), or controling the existing dam size, or weight the number/size of dams by the number of years since the dam has established to capture old dams might be less important since it's not functioning anymore
8. interacting the existing number/size of dams before the time window with the treatment dummy. we might expect to see decreasing return to scale of building dams

robustness:
1. replicate all the above analysis with zhejiang province and all other province (some regression can only be run with Zhejiang Provinve)
2. replicate all the above analysis with province-year fixed effect
3. replicate all the above analysis with SDID, TWFE, and all other staggered DID methods (some regressions can only be run with TWFE)

long difference specification (where we can go a litlle bit further back): 
1. nighttime light data seem to exist 1992. we can do: reg change of nighttime light (1998-1992) on the increase of dams (1998-1992).
2. population census data seems to exist in 1982 (3rd) and 1990 (4th). then reg change of population (1990-1982) on the increase of dams (1990-1982)
3. if we believe there is lagged effect, then we can do a local projection approach: run the following regressions and collect the coefficient: reg change of outcome at t on the change of dams at t, at t-1, at t-2 etc to see the impact

firm level analysis:
1. for each firm, if we know the location, calculate 1/2/5 km buffer. every row is one firm
2. define treatment = 1 if there is existing dams/new dams
3. define treatment = 1 if there is existing dams/new dams for specific types or size
4. see if firm's outcome are affected by dam establishedment
5. ideally this would need a firm panel data (like ASIE, tax survey data etc.)
6. question to think: what do we learn from these firm level regression, except it can serve as a robustness
7. it might be able to answer: do existing firm produce/pollute more, do large firms respond differently compare to small firms? do firms in different industry respond differently? what's the tfp response?
8. we can essentially replicate all the county level analysis with the firm level analysis, but we need to think what's the additional insights we can get from here.

less causal but it helps us to pin down parameters for model
1. instead of regression these outcome variables on dam dummy, we just regression it on the number of dams, size of dams, or log log relationship. we might see decreasing return to dams. then after a certain point, the benefit-to-cost ratio will be smaller than 1 if we also know what's the cost. this tells us what's the optimal stopping point. otherwise it's less interesting if it's optimal to always build dams

Question 2: 


other randome thoughts - not todo tasks:
1. what model we should have in mind
2. what's the cost of dams, financial cost, other environmental cost, other indirect cost.
3. we might be able to know what's the follow up investment for dams (from government procurement data). then we will see when government choose to renew the dam, which somewhat speaks to the Rust (1987) optimal stopping / dynamic discrete choice literature, but put it in an environment economics topics, which is another way to make this paper popular.
