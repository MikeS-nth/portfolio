# Identifying Target Counties for a New Liquor Store in Iowa

This project was from an assignment for the General Assembly Data Science Immersive course that I completed.  We were given a dataset from the Iowa Alcoholic Beverages Division containing detailed information on wholesale transactions and Iowa liquor stores covering about 5 quarters.  

In the scenario, there were investors interested in opening a new liquor store in Iowa and wanted to know what locations they should target for in-depth market research.  The task was to use the dataset to develop recommendations for the investors on which specific geographic areas made sense for spending limited research resources.

Coming from a technology sales and business strategy background, I found this project particularly interesting.  One of the key challenges with the data was that it provided wholesale transactions from the state to the store, but no information on store-level sales to consumers or other costs.  This meant I was working with the equivalent of COGS for each store, but no way to confidently know revenue (let alone profit).  

I focused my geographic attention to the county level, since zip codes are terrible for anything other than sending letters and city-level is too granular and limiting for a largely rural state.  To home in on target counties, I created what I called an "opportunity score", which compares a county's population, per-capita income, per-capita liquor stores, and liquor store COGS (as a rough leading for the businesses' performance).  This allowed me to rank and sort each county by their opportunity score (higher is better), which is a deliverable for an investment team seeking to home in on target counties.

The code and report can be viewed in the jupyter notebook in this folder.  However, the final output table is generated using Bokeh, which does not seem to render on Github.  For best results, I recommend [reading the notebook via nbviewer](http://nbviewer.jupyter.org/github/MikeS-nth/portfolio/blob/master/Iowa_Liquor_Store_Siting/Liquor_store_siting_analysis.ipynb).

*Note:* because the source data file is >377MB, I am not hosting it on Github.  If you are interested in recreating the analysis, you can find you can access the same data from the [Iowa Alcoholic Beverages Division](https://abd.iowa.gov/).  I may consider hosting this specific file if enough people express interest.
