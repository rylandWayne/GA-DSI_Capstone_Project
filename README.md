# California Water Conservation
- - - -
by Ryland Matthews

## Notebooks
> 1) Web scraping  
> 2) Data cleaning   
> 3) Exploratory Data Analysis  
> 4) Data Visualization  
> 4) Modeling   

##  Problem Statement
Which parts of California’s water conservation strategies are most effective at mitigating the state’s water shortage crisis?

## Background
I live in northern Nevada in the watershed of the Sierra Nevadas, which is a crucial component of the water supply for Nevada and California. Since moving back here, I’ve worked with [River Justice](https://knpr.org/desert-companion/2021-05/river-justice), organized by the Numu (Paiute in english) tribe around water conservation and quality. This year the group has cleaned up approximately 45 tons of trash along the Truckee River, and the group does other activism about water and wildlife conservation between Lake Tahoe and Pyramid Lake, where their reservation is located. Working with people who consider it a sacred calling to respect the balance between human use of our water resources and leaving water for local ecosystems, I have become much more interested in learning about the sources of our water, how it is used, and how it supports natural ecosystems.
I started this project looking for data on how water conservation is handled in Northern Nevada, but anything I could find was incongruous to the point that it would require months of data wrangling that would vastly exceed the time allotted for this project.
I was able to find much more complete datasets for California from their Water Board , Natural Resources Agency, and Department of Water Resources, which is reliant on our shared watershed and is a far larger water consumer than Nevada

## The Datasets
Water Supplier Conservation Compliance
This data covers most water suppliers in the state, whether they used direct oversight or self certification for meeting their water compliance standards, and how much they may have missed the standard by in 2016

Public Water System Operations Monthly Water Production and Conservation
With time stamped entries, this dataset covers total residential, commercial, and agricultural water usage by California’s water suppliers

[Water Shortage Emergency Response Actions](https://www.waterboards.ca.gov/water_issues/programs/conservation_portal/conservation_reporting.html#monthly_archive)
Released this year, this dataset covers various conservation strategies used by the participating subset of water suppliers, as well as counts for each supplier for the number of investigations on water waste they have carried out and waste notifications issued

In considering California’s entire water system, other data was gathered for this project about precipitation, hydro-power generation, land subsidence due to groundwater pumping well perforations, and voluntarily reported groundwater well levels. Due to the scope and interdisciplinary nature of this data, I ended up referring to California’s C2VSim hydrologic model, which has it’s own section below

## Water Conservation Strategies
California sets water conservation goals for it’s water suppliers and issues a 6-tier series of conservation stages for regions and counties that mostly focus on residential use

## Libraries Used
> pandas  
> numpy  
> seaborn  
> matplotlib  
> datetime  
> time  
> tqdm  
> statsmodels  
> sklearn   

## Exploratory Data Analysis
In my data analysis, most of the metrics for the state’s water conservation regulations ended up having low to very low correlation to the target variables of interest like total potable water produced and residential gallons per capita per day (R-GPCD), the latter being especially useful as it accounts for changes in the state’s population
I wanted to use models with interpretable variables predict these target variables, using LASSO regularization, self-certification vs. state monitoring of water suppliers gets zeroed out by the regularization penalty when predicting R-GPCD, and modeling how much a given supplier missed their conservation metrics on comes up with an R-squared value around zero. A similar thing happened when modeling water production over the last 7 years, with the regularization zeroing out the coefficient for the state’s program of tiered water conservation contingency stages
![](assets/README-e75297a7.png)
*Per capita water production is very steady across the most commonly used conservation stages*

Data analysis did provide some key insights on these programs, especially where they could be scaled up and expanded. On suppliers’ meeting their conservation standards, monitored suppliers appear to be about twice as effective on average at meeting compliance targets, but they make up about 7% of suppliers and on average serve about one third the number of Californians, only about 3% of the state population
The state seems very hesitant to escalate its water conservation stages for water suppliers. In the last 7 years the frequency that the different stages have been invoked are:
```
None — 20.00%
Stage 1 — 35.45%
Stage 2 — 32.23%
Stage 3 — 10.50%
Stage 4 — 1.66%
Stage 5 — 0.01%
Stage 6 — 0.15%
```

As the drought has prolonged, the state has even been decreasing the overall levels of conservation stages applied across the state
The state also only started publishing its Water Shortage Emergency Response Actions data this year, only dating back through last year. So far, only 116 suppliers are reporting in the first year. Of those, different strategies are being used, with 61 reporting that they’ve raised rates (data not provided for how much they’ve raised those rates), 68 are offering turf replacements and rebates, and 16 are trying desalination, in addition to other strategies like social media awareness campaigns.
In the last year, the suppliers reporting their emergency response actions have investigated 3014 water waste complaints and issued 3108 water waste notifications. Of these investigations, 2122 or about two thirds of all investigations were in the South Coast hydrologic region, which includes Los Angeles and San Diego and contains about 20 million people. Given that a part of the LAPD covering a single district in LA can issue close to 7-9000 jaywalking citations [per year](https://www.latimes.com/opinion/livable-city/la-oe-schultz-pedestrians-lapd-20180806-story.html), this seems like the state could significantly reorder it’s priorities as they continue further into prolonged drought


## C2VSim
To look at the total hydrological picture, I also referred to the [California Central Valley Groundwater-Surface Water Simulation Model](https://water.ca.gov/Library/Modeling-and-Analysis/Central-Valley-models-and-tools/C2VSim) both for a greater understanding of how California has been modeling the use and replenishment of their water resources, as well as considering how the methodology used for C2VSim might be strained when accounting for more recent agricultural and economic trends since it’s last comprehensive update in 2009. This is the model that the state uses to make decisions about it’s water program
![](assets/README-5bfcb935.png)
*Flows and weights rom the C2VSim Documentation*

The datasets used in this project showed very regular water production even through prolonged drought, and while admirable in its ability to provide for the people of California, it comes at the expense of the environment. Due to the nature of California’s water rights system regarding surface water and it’s almost entirely opaque and mostly unregulated use of groundwater, California uses the environment as a backstop for its water usage. This is leading to increasingly disastrous results, like the collapse of fish populations, disrupting food chains, and causing long term damage to the state’s groundwater storage capacity
C2VSim accounts for many of these things, as it uses land subsidence, where the land actually sinks as the groundwater below it is completed, to estimate the total available underground water storage capacity, as well as tracking the dropping levels of groundwater, which are requiring deeper drilling to reach. Subsidence is in many ways permanent, once the land has dropped the total space available for recharging groundwater diminishes, as well as causing infrastructural damage on the land above
![](assets/README-20a4bda9.png)
*Groundwater subsidence, inflow, and outflow models from the C2VSim Documentation*

C2VSim is also the state’s current tool for estimating groundwater pumping, but this has its limitations.  First, C2VSim uses a coarse grid that does not have much ability to account for topology, and so far has been used for modeling the water table in and around California’s central valley. Groundwater pumping is modeled based on calculated agricultural acreage and calculated evapotranspiration rates, differenced by the modeled availability of surface water. However, since the last major update to C2VSim in 2009, Californian farmers have started growing more water intensive crops like almonds and rice, which are often flooded as part of their cycle, as well as growing more alfalfa for increasingly exported cattle feed, and the model seems to mostly assume relatively efficient use of the groundwater pumped, not accounting for negligent waste

## Sustainable Groundwater Management Act
Currently, one of the main sources of frustration for concerned citizens, researchers, and policy makers is that no one can really say how much groundwater California is really using. Estimates range from 60% ([UC Davis](https://watershed.ucdavis.edu/files/biblio/2015Drought_PrelimAnalysis.pdf)) to 75% ([NASA](http://www.nytimes.com/2015/06/07/business/energy-environment/california-farmers-dig-deeper-for-water-sipping-their-neighbors-dry.html)). Following the adage “you can’t manage what you can’t measure” in 2014 California passed the SGMA, the first steps towards having more insight into how much groundwater is really being extracted. The Act also creates [Groundwater Sustainability Agencies](https://sgma.water.ca.gov/portal/gsa/all) (GSA’s), but only 343 have been created in the 7 years since it was passed. Each GSA is supposed to make a [Groundwater Sustainability Plan](https://sgma.water.ca.gov/portal/gsp/status), but so far the state only shows that 47 proposals have been submitted with only 2 approved. The stated goal of the SGMA is to reach a sustainable usage rate of the state’s groundwater resources by 2040, but the efficacy and urgency of the project remains in question
The state also provides a [data viewer](https://sgma.water.ca.gov/webgis/?appid=SGMADataViewer#currentconditions) for the SGMA, but so far the vast majority of wells remain unreported as seen here
![](assets/README-23f841a5.png)

And the dataset underlying that has yet to be made available for the public to access
![](assets/README-e298ddcf.png)
## Conclusion
With California’s water current water conservation strategies, there are a few steps made clear by the EDA that might have an impact on their mostly residential focused water suppliers:
* More oversight for the water suppliers that are farthest off their conservation targets
* Invoke higher water conservation stages, at least up to more regular use of stages 2 and 3
* Get more suppliers reporting their conservation incentives and more resources put into in investigating cases of water waste

Courses of action that could have much greater impact on the water crisis are going to be very difficult, but should be one of the state’s highest priorities. These include:
* Reforming water rights for surface water to stop the current “use it or lose it” system that encourages large water rights holders to waste water to maintain future rights at their current level
* Changing their curtailment processes for surface water which currently works bottom up, cutting off junior rights holders and giving further leverage to those with seniority
* Accelerating the timeline of the 2014 Sustainable Groundwater Management Act
* Changing economic incentives on high water usage crops that are exported, especially those like growing cattle feed that accelerate greenhouse gas emissions from the dairy and meat industries
* Include groundwater in the “reasonable and beneficial use” doctrine in Article X Section 2 of the California state constitution

## Resources for anyone trying to learn more about the severity of California’s water crisis
[California Central Valley Groundwater-Surface Water Simulation Model](https://water.ca.gov/Library/Modeling-and-Analysis/Central-Valley-models-and-tools/C2VSim)

[9 sobering facts about California’s groundwater problem](https://revealnews.org/article/9-sobering-facts-about-californias-groundwater-problem/)

[What Are Water Rights?](https://gizmodo.com/what-are-water-rights-1696881723)
