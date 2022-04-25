# Data Analysis: Performance of BrightSpring ICFs under KKR

This is the data, code, and methodologies supporting the BuzzFeed News story Profit, Pain, and Private Equity, published April 25, 2022. Please read that [article](https://www.buzzfeednews.com/article/kendalltaggart/kkr-brightspring-disability-private-equity-abuse) and the accompanying [data story](https://www.buzzfeednews.com/article/johntemplon/kkr-buzzfeed-news-data-analysis), which contain important context, before proceeding.

## Background

An **intermediate care facility** or **ICF** is a live-in healthcare setting, often a home, where clients with intellectual and developmental disabilities are served 24/7. Large institutional ICFs run by a state can have as many as 1,000 clients, but that is exceedingly rare; more than 80 percent of the currently open ICFs in the US have eight beds or fewer.

ICFs are supposed to be inspected by their state licensing agency when they are first opened and then at least every 15 months thereafter. The state can also make additional visits at its discretion after someone complains about conditions at the facility. Often, when inspectors find problems, they return to make sure the issues are fixed. The reports generated from these inspections are called **surveys**. There are two different types of surveys: **Life Safety**, which focuses on the physical space of the facility, and **Health**, where inspectors focus on client health and well-being. Life Safety surveys focus on things such as whether the doors are clear and if the smoke alarms work. Health surveys look at whether, for instance, residents are receiving the right medicine, nutrition, and staffing levels, or examine any allegations of abuse and neglect.

Inspectors mark **deficiencies** in the survey when they find that a facility has failed to provide proper care or proper living arrangements for residents. There are two types of deficiencies: "standard" and the more serious "condition." Each ICF operates under a set of rules, known as "conditions of participation", that are set by the Centers for Medicare & Medicaid Services (CMS). An inspector issues a **condition** deficiency when they determine that a facility has violated one of those conditions of participation. **Standard** deficiencies are less serious issues that an inspector finds during a survey.

## Structure

* data
	* ownership
		* raw
		* final
	* qcor
* notebooks
	* ownership
	* analysis
* README.md
* .gitignore
* Pipfile

## Data

There is no national database of intermediate care facility ownership. In order to narrow down the possible universe, BuzzFeed News focused its analysis on the seven states that have at least 50 for-profit intermediate care facilities, according to the CMS. They are:

- California
- Indiana
- Louisiana
- North Carolina
- Ohio
- Texas
- West Virginia

### Survey data

CMS hosts a [database](https://qcor.cms.gov/main.jsp) called Quality, Certification and Oversight Reports (QCOR), with the results of all of the state inspections of intermediate care facilities. BuzzFeed News last obtained that data on April 8, 2022. We are examining only data up to Dec. 31, 2021, because of delays in inspection data being submitted and appearing on the QCOR site. 

When asked about the validity of the QCOR data, CMS said that “QCOR is drawn from uploaded survey activity input by the State Survey Agencies. It is from the same source as CASPER and other CMS reports. The difference is in the presentation and timeliness of the data (there can be a slight lag for data posted on QCOR, which is updated weekly). It is based on the same foundational survey entry system, and therefore as reliable as our other survey data sources, with the differences being in the level of detail, frequency of updates, and simplified display.”

The number and types of surveys conducted were [affected](https://www.cms.gov/files/document/qso-20-31-all-revised.pdf) by the COVID-19 pandemic. The data we collected includes for each **survey**:

- Provider ID
- Date of the survey
- Survey Type (whether it was a standard or complaint survey)
- Survey Subtype (Life Safety or Health)
- Number of deficiencies

BuzzFeed News added a unique `survey_key` field to the dataset in order to track deficiencies across surveys. The key is in the format of `Provider ID-Date-Number`.

For the **deficiencies**, we also know:

- Level (standard or condition)
- Tag (a code classifying the specific infraction)
- Description (a longer explanation of the tag)

### Facility ownership data

BuzzFeed News reached out to the health departments of the seven states with the most for-profit intermediate care facilities to obtain detailed ownership data for all ICFs open as of March 1, 2017, or later. We chose this date in order to have two years of ownership data prior to KKR's acquisition of BrightSpring. The results of that effort can be found in the `data/ownership/raw` directory. The states provided the data in different ways, but each included at least the name of the legal owner and a way to merge the dataset with the information found on QCOR. (See the README in that directory for more information about the data received from each state.)

### Transforming the data

The files in the `notebooks/ownership` directory walk through the process of standardizing the dataset for each state in our analysis. The notebooks combine the data received via FOIA, additional research done by BuzzFeed News, and CMS data obtained from QCOR into a final ownership dataset.

### Final ownership data set

The final ownership data set includes the following data points for each facility. Each of the fields comes from the QCOR dataset except for `legal_owner` and `is_brightspring`, which BuzzFeed News added through the ownership notebooks referenced above.

**From QCOR**
- name
- provider_id [a six-character alphanumeric string ID for a facility]
- type
- state
- address
- particip_date [when the facility started participating in Medicaid]
- certified\_beds [the number of beds certified by Medicaid]
- ownership\_type [either for-profit, non-profit, or government]
- termination\_date [when the facility was closed, if it has been]

**Added by BuzzFeed News**
- legal\_owner [the company that owns the facility, according to state records]
- is\_brightspring [whether the facility is owned by BrightSpring]

All of the standardized data can be found in the `data/ownership/final` directory.

## Analysis

BuzzFeed News consulted with four social scientists about the methodology. Three — Alison Morantz (Stanford University), George Garcia (Stanford University), and Charlene Harrington (University of California, San Francisco) — have experience analyzing intermediate care facilities specifically. A fourth, David Stevenson (Vanderbilt University), has done similar analyses for nursing homes.

The main thrust of the BuzzFeed News analysis focuses on *conditions* issued by state inspectors in *standard* surveys. 

BuzzFeed News focused on *conditions* because those citations represent the most serious issues inspectors found at a facility. Standard deficiencies can represent a wide range of potential issues, from life-threatening to poor paperwork. We also had additional concerns that standard deficiencies would double count conditional deficiencies in some cases — for instance, receiving a standard deficiency and a condition deficiency for the same staffing issue.

BuzzFeed News focused on *standard* surveys because they are scheduled to occur on a regular basis (at least once every 15 months) for every home in the nation. This cadence, along with the fact that inspectors are completing the same type of analysis for every home, seemed to be the most apples-to-apples comparison available in the data. 

In addition, BuzzFeed News looked at *complaint* surveys as a supplemental part of the analysis. Complaint surveys occur at the discretion of the state licensing agency and not at regular intervals. In addition, BuzzFeed News found that there is a relationship between the size of the facility and the number of complaint surveys received. Therefore, when analyzing complaint surveys, BuzzFeed News only analyzed smaller (4-8 bed) facilities and looked at the number of conditions received on a per bed day instead of a per survey basis.

Finally, KKR [announced](https://www.businesswire.com/news/home/20190305005975/en/KKR-Completes-Acquisition-of-BrightSpring-Health-Services) it had completed the acquisition of BrightSpring Health Services on March 5, 2019. The analysis is based on a timeframe that uses that date as a point of delineation.

### Compare KKR-owned BrightSpring to its peers

1. Load the facility data, including BuzzFeed News’ additional columns. This results in 3,946 facilities that were found in the QCOR dataset in these seven states.
2. Load the survey data from QCOR.
3. Select all standard Health surveys (i.e., excluding Life Safety surveys and complaint surveys) dated March 5, 2019, to Dec. 31, 2021. This results in a subset of 6,861 surveys that were conducted in the seven states.
4. Calculate the total number of conditions issued by state surveyors on standard Health surveys during this time period in BrightSpring and non-BrightSpring facilities. 
5. Calculate the number of conditions in Standard surveys per 100 surveys for BrightSpring and non-BrightSpring owners — and compare the rates.

The `analyze-standard-survey-conditions.ipynb` notebook walks through this process.

**Finding:**
> BrightSpring had more conditions per 100 standard surveys in six of the seven states analyzed. It had fewer conditions per 100 standard surveys in North Carolina.

6. Calculate for smaller facilities (4-8 beds) the number of conditions in complaint surveys per 10,000 bed days open (*during time period examined*) for BrightSpring and non-BrightSpring owners — and compare the rates. This results in a universe of 3,646 facilities and 7,464 surveys.

The `analyze-complaint-survey-conditions.ipynb` notebook walks through that step.

**Finding:**
> BrightSpring had more conditions per 10,000 bed days open in six of the seven states analyzed. It had the same number of conditions per 10,000 bed days open in Indiana.

### Supplemental Analysis

In addition to the analysis above, the `additional-survey-statistics.ipynb` notebook walks through the calculation of other survey data points included in the story.

## BrightSpring and KKR Response

KKR issued a [statement](https://www.documentcloud.org/documents/21660515-kkr-statement-for-publication-2022-04-07) saying, “We vehemently disagree with the grossly misleading narrative you presented.” BrightSpring sent a similar [statement](https://www.documentcloud.org/documents/21660516-brightspring-statement-for-publication-2022-04-07) calling BuzzFeed News’ findings “inaccurate, misleading, and fundamentally flawed.” It said the data analysis was unsound because the underlying records were unreliable and were collected during a pandemic.

Despite the statements from CMS about the timeliness and accuracy of QCOR, BrightSpring said that the data was from a flawed source: “There is no assurance your data, especially in comparative terms, are up to date or reflect the accepted industry accuracy standards for reporting, but you fail to acknowledge that critical limitation, which would mislead readers.”

In addition, BrightSpring said, “The time frame you focused on was March 2019 to December 2021. Much of this period was in the middle of the COVID-19 pandemic at a time when regular surveys were largely suspended or delayed. Some of the ongoing surveys were emergency only or depended on the specific surveyor’s access to things like PPE. Drawing conclusions and/or extrapolating based on that unique and unprecedented time frame is inappropriate for myriad reasons, and your data analysis is fundamentally flawed and lacks critical context.”

BrightSpring also questioned our decision to limit the analysis of complaint surveys to smaller (4-8 bed) facilities: “The methodology you create in your study of a tailored 4-8 bed size category (QCOR data is split in categories of 1-8 beds and 8-16 beds for their “small facility” category) is unique to BuzzFeed and inconsistent with the accepted standards for evaluating quality results.”

## Technical Notes

All of the analyses above are coded in Python 3, using the libraries listed in the Pipfile.

## Licensing

All code in this repository is available under the [MIT License](https://opensource.org/licenses/MIT). All data files are available under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) license.

## Questions / Feedback

Contact John Templon at [john.templon@buzzfeed.com](mailto:john.templon@buzzfeed.com).

Looking for more from BuzzFeed News? [Click here for a list of our open-sourced projects, data, and code](https://github.com/BuzzFeedNews/everything).