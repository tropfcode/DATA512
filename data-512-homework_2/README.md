## Project Goal

The goal of this assignment is to explore the concept of bias in data using Wikipedia articles. This assignment will consider articles on political figures from different countries. Please see [full assignment description](https://docs.google.com/document/d/12Y4lPd5ORyK3s1vv-MQgF7-bYDpbpmKFKvSUgPmiLSs/edit?tab=t.0) for more detail.

## License and TOU of source data

[Source](https://2024-wpds.prb.org/) for `population_by_country_AUG.2024.csv`

[Source](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) for `politicians_by_country_AUG.2024.csv`

Any user of this code base most adhere to the [Wikimedia Terms of Use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use) policies due to use of data procured from the Wikimedia Foundation.

## Relevant API Documentation

This project uses the [Objective Revision Evaluation Service(ORES)](https://www.mediawiki.org/wiki/ORES) Machine Learning Service provided by Wikimedia. The [ORES API](https://www.mediawiki.org/wiki/API:Info) is used to leverage the service and conduct analysis on the data in this project. Any code leverages ORES is licensed under [Creative Commons](https://creativecommons.org/licenses/by/4.0/).

## Requirements and Installation Instructions

### Creating Virtual Environment and Installing Python Packages
Python version 3.8 or greater is required.

The following instructions create a virtual environment (isolated region for Python packages), activates the environment for use, and installs required python packages:

> virtualenv -p python3 HW2

> source HW2/bin/activate

> pip install -r requirements.txt

### Credentials as Environment Variables

In order to leverage the [ORES API](https://www.mediawiki.org/wiki/API:Info) in the jupyter notebooks of this package, a [Wikipedia account and Authentication Token](https://api.wikimedia.org/wiki/Authentication) must be created. Detailed instructions for how to do that can be found in the `Get Your Access Token` section of `notebooks/wp_ores_liftwing_example.ipynb` of this repo.

Once an Authentication Token has been created, place the username associated with the newly created Wikipedia account and Authentication token in `creds.sh` (replace whatever values may be there).

Once this is done, make sure to run the following command in the terminal before continuing on: `source activate creds.sh`.

## Data Products

All data can be found in the `datasets` directory.

### Origin Data

Below are raw datasets found in this repo that can be directly downloaded.

* population_by_country_AUG.2024.csv
  A dataset procured from the [World Population Data Sheet](https://2024-wpds.prb.org/). Consists of two columns: `Geography` and `Population`.
  The `Geography` column actually represents regions in a hierarchical order. A region is designated in <ALL CAPS> and everything under that region in <lower-case> is a country part of that region.
  `Population` is in millions. So if a row as a `Population` value of 4.4, it is 4.4 million people.

* politicians_by_country_AUG.2024.csv
  A [Wikipedia based dataset](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) with three columns: `name`, `url`, `country`.
  The `name` column is the name of the politician the article is about.
  The `url` is the url to the Wikipedia article about the politician.
  The `country` is the country the politician was assigned.

### Secondary Data Products

`wp_politicians_by_country.csv` is a data product produced by the `notebooks/data-creation.ipynb` notebook. It consists of six columns:

1. `country`: Country that a politician belongs to.
2. `region`: Region country resides.
3. `population`: Population of country in millions.
4. `article_title`: Title of Wikipedia article, also used to derive politician name.
5. `revision_id`: A Wikipedia identification number used to determine most recent version of article that was revised. Used as argument for ORES API.
6. `article_quality`: Prediction value from [ORES](https://www.mediawiki.org/wiki/ORES).

### Special Data Considerations

Some special considerations about the data include: No politician from a North American country in the `politicians_by_country_AUG.2024.csv` dataset, countries labeled with a population of 0.0, and politicians that are part of more than one country. This and more should be in consideration when leveraging any of the data found in this repo. Special attention must be made properly clean and responsibly use the data in this repo.

## Notebook Descriptions

All notebooks for this repo can be found in the `datasets` directory.

`data-creation.ipynb` obtains the `revision_id` for each of the Wikipedia articles/politicans, conducts ORES on each article, then merges the results with population data to produce the secondary data product `wp_politicians_by_country.csv`.

`analysis.ipynb` uses `wp_politicians_by_country.csv` to conduct the six analyitcs asked by the HW2 instructions.

## Research Implications

### Assignment reflections

There are two primary things that come to mind with this assignment -- how labor intensive careful data processing can be and how much assumptions can alter results.

Regarding careful data processing, it is understandable why many people decline to thoroughly document and processing their data. The time and attention needed to carefully follow every User Agreement, cite data sources, careful document data schemas (and their meaning), among many other things is immensly time consuming and tedious. Unquestionably though, it is important to go through the necessary work in the process of data provenance and governance. There are many implications associated with how data is described and documented -- from analysis decisions to conclusions made from results.

Assumptions are made anytime analysis is conducted, especially using data from online sources. When conducting analysis for this project, it was quite interesting to see how results changed based upon assumptions made. For example, the countries with the worst article quality change if we do not take into account countries under a certain population threshold. Another example would be agreeing to which region a country may belong. Certainly there is active dispute if certain countries are labeled as belonging to certain regions. If the designations were to change among a few countries, then results at the region level could drastically change. Finally, it should never be assumed that any data one is working with has been properly cleaned. From repeat Wikipedia articles being attributed to multiple countries to countries have a population of zero, it would be a bad assumption to make that the data has not faults to consider.

### Three Questions

Below will be answers to three questions from the HW2 instructions.

1. Q: What (potential) sources of bias did you discover in the course of your data processing and analysis?

   A: One form that stood out was the lack of politicans from North America. Although North America was a region in one of the datasets, a lack of politicians from North America removed that region as part of the analysis. One may end up with quite different conclusions if North America was included.

2. Q: What biases did you expect to find in the data (before you started working with it), and why?

   A: There was an expectation to see reflections of personal bias as an American in the data -- particularly regarding article quality in Western Countries. Analysis showed that the regions with the highest article quality were from Western (European) countries. This bias was expected ahead of time due to the bias of journalistic intergrity and free speach that exists in Western countries. Whether that is true or not can certainly be debated, however the results align with the bias going into the analysis.

3. Q: What might your results suggest about the internet and global society in general?

   A: There are some potentially worrisome conclusions that can be leveraged from the results of the analysis conducted in this homework. One interpretation could be that Western countries value free speach and journalistic integrity in comparison to other regions. Personally, that is a stretch from the type of analysis that was conducted, but it is interesting to ponder regarding the responsibility of those that do such analysis and making sure that interpretation is handled with care.
   Ultimately, these results are a microcosm of the world and should be looked at as a proxy for perhaps general article generation across the globe. It is clear that Western countries enjoy Wikipedia.

