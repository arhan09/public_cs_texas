# Code and Data for "Relinquishing Riches" -- Covert and Sweeney (2021)

## Steps for recreating the results from the raw data files
1. Download the data at this [Dropbox link](https://www.dropbox.com/s/oj2ezsqjd78tct5/public_20210706.zip?dl=0). 
   - A description of the data is provided below. Note that that the parcel shape files described at the end are proprietary and are not provided. 
2. Clone / download this repository somewhere on your computer. In order to run the files, **you must rename the directory from `public_cs_texas` to `texas` after downloading it**.
3. Create a `data.txt` file in the root code directory. This file should just have a single file path to direct to where the data folder has been placed on your hard drive, such as `C:/Dropbox/texas/public`. 
4. Navigate to `code` folder in this repository on your command line and type `make`.
   * Mac users should already have make.
   * Windows users can install [chocolatey](https://chocolatey.org/install), and then type `choco install make` on the command line
      * Note that make cannot be run on Windows computers if there is a space in the file path. In this circumstance, the user can create a [junction](https://www.sevenforums.com/tutorials/278262-mklink-create-use-links-windows.html).

Note: `make` will automatically install all of the required R packages (listed in `code/package_installation.R`).

---
# Data description 
## Raw Data
* `addenda.csv`: While GLO state leases are uniform, RAL leases often have additional addenda clauses. These RAL lease contract terms can be found in the public contracts [online](http://www.glo.texas.gov/history/archives/land-grants/index.cfm), and were inputted and categorized manually in August 2018.  This file represents our human-entered and cleaned dataset of the the underlying lease addenda data
* `company_aliases.csv`: Our database of company name strings and associated consistent company names, or "aliases"
### `leases`
* `June2020`: This folder contains lease shapefiles downloaded in June, 2020 from the General Land Office (GLO) [GIS database](http://www.glo.texas.gov/land/land-management/gis/).  This includes two shape files, one for "Active" leases, those still in primary term, or held by production, as of June 2020, and "Inactive" leases, which are the *most recently active* lease on a parcel as of June 2020.  The "Inactive" lease shapefile does not include older inactive leases on parcels that have had two or more inactive leases. 
* `LeaseLandFile.csv`: This file matches lease information (a `Lease_Number`) to associated parcel information (a `ControlNum`).  The GLO sent us this file in February of 2017. 
* `lease_parcel_scraping`: This folder contains additional information linking leases to their associated parcels. 
* `tablMineralLeaseSummaryAll_2020.csv`: This dataset consists of non-spatial information we have for inactive and active lease files, including older inactive leases that we do not have shape files for.  Sent by GLO in April of 2020.
* `odas.csv`: This is a list of leases that are a part of "Orderly Development Agreements" which we manually constructed in the course of reading lease agreements.  These "ODA's" are contracts signed between E&P companies and GLO which require lessees to do additional drilling above and beyond what is required in the original lease contracts, and which pool output across leases within the agreement.  We use "ODA groups" to allocate lease output to leases within an ODA, because lessees often erroneously make payments to just one lease in an ODA, even when the payment represents production from many leases.
### `bids` 
All bids (above the reserve price) for GLO auctions are available [online](http://www.glo.texas.gov/energy-business/oil-gas/mineral-leasing/leasing/index.html) as PDFs. The PDFs were manually entered in and saved as excel sheets. 
### `payments`
Bonus, rental payment and royalty payment information obtained through a public information requests in June of 2017, mid 2019, and mid 2020.
### `prices` 
Historical spot prices for production were downloaded in October, 2019 from the EIA. Oil prices can be found [here](https://www.eia.gov/dnav/pet/hist_xls/RWTCm.xls), and gas prices can be found [here](https://www.eia.gov/dnav/ng/hist_xls/RNGWHHDm.xls)
### `coversheets`
All RAL/GLO contracts are [online](http://www.glo.texas.gov/history/archives/land-grants/index.cfm), and the vast majority of RAL leases should have a review sheet that contains information on the proposed terms of a lease. The terms from the review sheet were manually entered for RAL leases signed between 2005-2016 in July 2017.  This folder includes a set of human-entered and cleaned datasets of these reviews, which we used to verify RAL lease characteristics and collect delay rental payment information
### `Assignments`
Our human-entered and cleaned dataset of the RAL and auctioned lease assignment events
### `Notices`
A set of machine entered (and human-corrected) "bid notice" files, providing information about what leases were available for bid in each auction.  These two files cover just the earliest auctions and the latest auctions.  See `intermediate_data` for more bid notice data.

## shape_files:
* `Shale_Plays and Shale thickness information`: Downloaded from EIA https://www.eia.gov/maps/layer_info-m.php. We selected out the major shale plays, Barnett, Haynesville, Spraberry (Permian), Delaware (Permian), and Eagle Ford.  Shale thickness information ("isopach" data) is only available for the Wolfberry formation (spanning both Spraberry and Delaware portions of the Permian), and parts of the Eagle Ford.
* `Land_Cover`: Downloaded in November 2017 from [here](https://www.mrlc.gov/). The original dataset includes the entire US. The raster file saved in the sub-directory `landcover` was clipped in ArcGIS to just include Texas.
* `texas_grids_bounding_box`: A shapefile containing a bounding box which spans the initial PSF land allocation.
* `txdot-roads_tx`: Road data was downloaded in August 2017 from the Transportation Planning and Programming (TPP) Division of the Texas Department of Transportation (TxDOT), who maintain a spatial dataset of roadway polylines for planning and asset inventory purposes, found here https://tnris.org/data-catalog/entry/txdot-roadways/. This includes data associate with On-System highways, County Roads, Functional Classified City Streets, Toll Roads and Local Streets. 
* `usgs-rivers_tx`: Downloaded in September 2018 from the [U.S. Geological Survey](https://tinyurl.com/yddlyuuv). This National Hydrography Dataset is a comprehensive set of digital spatial data that encodes information about naturally occurring and constructed bodies of water, paths through which water flows, and related entities.
* `us_county`: Census county shapefiles downloaded in January 2017 from the census [website](https://www.census.gov/geo/maps-data/data/cbf/cbf_cousub.html)

## Intermediate Data
This folder contains datasets that involved manual entry *after* analysis to either create or modify. 
* `assignments`: We manually fixed around 200 assignment dates that were inputted incorrectly, especially where assignment dates are earlier than effective dates
* `glo_notices_final.csv`: GLO notices to the public that a parcel will go up for a leasing auction.  These are available from the same source as the bids, and we used a combination of R and manual input to read in the underlying PDFs.  This covers auctions between 2005-2016. 
* `leases`: manual corrections to lease variables

### Private Parcel Shape Files
We purchased Public School Land parcel data from P2E in December 2017 and we use it in all of our parcel-level analyses.  While we cannot publish the raw data, we have included the following code which cleans and analyzes this data: `Data_Cleaning/psf_conversion.R`, `Data_Cleaning/clean_parcels.R`, `Data_Cleaning/clean_parcel_bridge.R`, `Data_Cleaning/clean_lease_spatial_info.R` `Analysis/make_lease_parcels_monthly.R`, `Analysis/final_parcel_outcomes.R`,
`Analysis/parcel_monthplots.R`, and `Analysis/regressions_parcels.R`. 
