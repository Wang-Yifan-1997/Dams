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

Question 2: what's the cost of dams
1. is there a way we can get the cost of dams, even for a subsample? like for the large dams in the recent years? I feel like if we try the benefit of dams, without speaking to the cost side, it would be a big miss.
2. with maybe a few hundred or thousand of dam construction cost. we do a prediction model (machine learning maybe) to predict all the remaining cost. 
4. except for the monetary cost of dams. there is a more `economic' impact cost: environment cost, Resettlement and livelihood destruction, Induced seismicity, Sediment starvation. Fishery destruction, Loss of flood pulse, Groundwater effects etc. some of the cost we can try to run a few regressions. and some of cost we have to borrow from other literature,
   4.1. we do a back of envelop calculation. we compare the cost of dams with the benefit of dams
   4.2. alternatively, we put everything to a model, and calculate the welfare
5. this exercise is much harder but i think it could distinct our paper with many others if we can do a rigorous calculation of the cost and say something about welfare and optimality. this would involve a bit more data collection

Question 3. how does government decide to build a dam
1. we need more background knowledge
2. one concern that people might ask is, if counties are going to grow faster, and then government decides to build a dam. this could explain why we see things going well after a dam. but that's purely driven by dam endogeneity. our DID regression results could alleviate this concern (if we see parallel trend). but it would be much stronger if we have a good economic intuition/or government policy as an IV

Question 4. when do dam works and when do it fail
1. we need to see every single dam's impact. here are two ideas to do this (of course these are initial ideas and we need to refine them later)
   1.1. for the firm level regression, we assign the firm level effect to each dam. this would be dam-firm pair effect. then aggregate these dam-firm pair effect to dam level
   1.2. for each dam, we draw a 1/2/5 km buffer. then for every dam, we draw 100 random points in the same province with 1/2/5 buffer as counterfactual control group. use synthetic control method or other matching method to estimate the treatment effect of dam for each dam.
2. then we will see the heterogenous effect of dams. next, we ask why some are more effective, and some may have a backfire
3. potential hypothesis:
   3.1. geographical reasons. maybe a dam requires sufficient and reliable water inflow to deliver consistent benefits
   3.2. demand: a electricity constrainted area would benefit more from hydropower stations compared to a place with abundant electricity. Irrigation dams work well when agriculture is genuinely water-limited.
   3.3. complementarities: a single dam is not going to work. a dam with road connection, grid connection are going to work. 

other randome thoughts - not todo tasks:
1. what model we should have in mind
2. what's the cost of dams, financial cost, other environmental cost, other indirect cost.
3. we might be able to know what's the follow up investment for dams (from government procurement data). then we will see when government choose to renew the dam, which somewhat speaks to the Rust (1987) optimal stopping / dynamic discrete choice literature, but put it in an environment economics topics, which is another way to make this paper popular.
4. dams are a form of government intervention. why do we need to have government intervention. why the market cannot solve it. if the benefit to cost ratio is larger than 1, then let's say there is big company, who can charge people money, and then build a dam. this market would exist if there is no market failure isn't. of course in the real world there is a lot of things going on but in this hypothetical thought experiment, what's the market failture prevents it to happen.
5. what's the optimal allocation of dams? what drives the optimality? there needs to be some clear tradeoff. and this optimality is determined both by spatial optimality and time optimality.
6. how does government decide to build a dam. there must be a decision rule.
7. why do we stop building dams? is it because of benefit smaller than cost, or political concerns or other things.
8. complementarity is currently a small section of question 4. but itself is an interesting question that might need more attention. it would be good to shape the paper as complementarity. and therefore a dam construction can be seen as a big push from one low equilibrium to high equilibrium (povery trap story and big push, which is common to justify industry policy)
