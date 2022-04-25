# Ownership Data

This directory contains all of the intermediate care facility data that was acquired by BuzzFeed News in the seven states with at least 50 for-profit ICFs. Additional information about where the files came from is below.

## Final

This directory contains the final ownership for each state after it has been processed through the ownership notebooks. For more about that process and the outputs please look at the README for the main repository.

## Intermediate

These files were created by BuzzFeed News. Additional details about each file are below.

### Supplemental ownership information

- `california-data-for-qcor.csv`: This CSV was created by scraping the [Cal Health Find Database](https://www.cdph.ca.gov/programs/chcq/lcp/calhealthfind/pages/home.aspx) on Aug. 10, 2021.
- `closed-texas-facilities.csv`: This CSV was created through research conducted by BuzzFeed News reporters about the ownership of facilities in Texas that were closed prior to our receiving the directory.
- `ohio-ownership-from-cost-reports.csv`: Ohio provided cost reports to BuzzFeed News on Dec. 4, 2021 (for 2020) and Jan. 4, 2022 (for 2016-2019). These reports contain detailed data about the financials of intermediate care facilities. BuzzFeed News used those files to compile ownership data for all the intermediate care facilities in the state.
- `wv-icf-info.csv`: BuzzFeed News downloaded a CSV of all of the intermediate care facilities in the state from the West Virginia Department of Health and Human Services's [Health Care Facility Search](https://ohflac.wvdhhr.org/Apps/Lookup/FacilitySearch) page on July 9, 2021.

### Files used for cleaning

- `la-facility-match.csv`: This is a manual cleaning file that matches the facility name in the files provided by the Louisiana State Department of Health to the names in the QCOR dataset.
- `unmatched-nc-federal-icfs-resolution.csv`: This file is a crosswalk between the differing information found in QCOR with the information found in the files provided by North Carolina.

## Raw

These files were provided to BuzzFeed News directly from state governments after reporters made Freedom of Information Act requests inquiring about the ownership of intermediate care facilities. Additional details about the state and date of each file are in the table below.

|File|State|Date Received|
|----|-----|-------------|
|Data Request JT 08122021 rev.xlsx|LA|08/19/2021|
|california-missing-facility-list-ICF-DDS.xlsx|CA|10/8/2021|
|Texas ICFIID Directory as of 9.20.21.xlsx|TX|10/8/2021|
|2021-10-15-nc-icfmr.xlsx|NC|10/15/2021|
|2021-11-08-ohio-ICF Licensee with Medicare ID -Public Request_.xlsx|OH|11/8/2021|
|Data\_Request\_JT\_12072021.xlsx|LA|01/04/2022|
|ICF-DD\_Facility\_Closures\_and_Ownership\_FINAL.xlsx|CA|01/13/2022|