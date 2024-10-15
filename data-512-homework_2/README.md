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

