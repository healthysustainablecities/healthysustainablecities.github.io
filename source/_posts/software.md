---
title: Global Healthy & Sustinable City Indicators software
date: 2023-02-01
author: GHSCI
---

The [Global Healthy and Sustainable City Indicators (global-indicators) software](https://github.com/global-healthy-liveable-cities/global-indicators) can be used to analyse and report on health related spatial indicators of urban design and transport features for diverse regions of interest using open and/or custom data. Examples of indicators include the percentage of population with access to some feature of interest like a regularly serviced public transport stop within 500 metres, or a composite indicator of neighbourhood walkability.   It does this using spatial and network analyses for sample points generated along a derived pedestrian network.  Results are aggregated to a grid with resolution corresponding to the input population data used, as well as overall summaries for the city or region of interest.  It outputs data in geopackage and csv formats, and can be configured to produce city reports, maps and plots for publication in multiple languages.

# Usage in brief

Analysis for a particular city proceeds in three steps:

1. **Configuration**
2. **Region analysis**
3. **Generate resources**

As a result of running the process, a geopackage of spatial features for a specified and configured urban region is generated, including indicators for point locations, a small area grid (eg 100m), and overall city estimates.  In addition CSV files containing indicators for small area grid cells and the overall city are also generated.  Optionally, PDF 'scorecard' reports summarising policy and spatial indicator results may be generated for dissemination.

A fully configured example city has been provided along with data to provide an example of how the process works. 

## Software installation

1. [The latest version of the software can be downloaded here](https://github.com/global-healthy-liveable-cities/global-indicators/archive/refs/heads/main.zip).  This zip file can be extracted as a project folder on your computer or cloud drive in a desired location. 
2. Install and run [Docker Desktop](https://www.docker.com/) according to the guidelines for your operating system of choice.
3. The global-indicators software is run from a command line interface, for example [Command Prompt](https://www.howtogeek.com/235101/10-ways-to-open-the-command-prompt-in-windows-10/) or [Terminal](https://aka.ms/terminal) on Windows, [Terminal](https://support.apple.com/en-au/guide/terminal/apd5265185d-f365-44cb-8b09-71a064a42125/mac) on MacOS, or [Bash](https://www.gnu.org/software/bash/) on Linux: 

## Configuration, Analysis and Reporting

- navigate to the folder where you extracted your software
- enter `.\global-indicators` if using Windows, or `bash ./global-indicators.sh` if using Linux or MacOS.

Depending on the computer you’re using, this should result in a screen like this:

![image](images/0_run_ghscic.jpg)
*Figure 1. Once you have Docker Desktop installed, run the software in a command line interface.*

If you’ve got this far successfully, it means that Docker has retrieved the required computational environment and dependencies for running our software and launched it in a ‘container’.  This is a bit like an isolated virtual Linux computer, which helps our software run regardless of each user’s different computer context (operating system, software etc).  It also launches a PostGIS spatial database container in the background too, which helps with the processing and data management in the background.

If you look at the contents of this folder, either by typing ‘dir’ or in a file explorer application, you’ll see there are three scripts (files ending in .py) corresponding to those mentioned in the Usage in Brief section above:

![image](images/folder_process.jpg)
*Figure 2. The process folder that contains scripts guiding the three key steps of configuration, analysis, and reporting.*

The first step is run by entering `python 1_create_project_configuration_files.py`.  This will initialise a set of configuration files in the configuration folder, and will advise you to edit these as required.  For now, we can leave them as they are and proceed with the example city, [Las Palmas de Gran Canaria](https://en.wikipedia.org/wiki/Las_Palmas).

The second step is to analyse a region, drawing on the data that you have retrieved and stored in the data subfolders and configured in the region and datasets configuration files.  For our example city, this done by entering, `python 2_analyse_region.py example_ES_Las_Palmas_2023`.  The last bit (`example_ES_Las_Palmas_2023`) is just a code name we have used to refer to Las Palmas, defined in our region configuration file.  We use a code name instead of a city’s actual name because it can avoid ambiguity.   For example, you may want to analyse Las Palmas in both 2023 as well as 2024; or you may be interested in Valencia in Venezuela  (e.g. VE_Valencia_2023) as well as Valencia in Spain (ES_Valencia_2023).  

To see the list of codenames for configured cities, run the script without providing a codename and it will let you know this, as well as a reminder for how to run the process overall.

![image](images/2a_list_codenames.jpg)
*Figure 3. The codenames for configured cities can be viewed by running `python 2_analyse_region.py`.*

Or, run it with a codename for a fully configured city (e.g. `example_ES_Las_Palmas_2023`, Las Palmas, for which data has been provided) and it will proceed to analyse the city:

![image](images/2b_analyse_regions.jpg)
*Figure 4. Running neighbourhood analysis for Las Palmas by entering `python 2_analyse_region example_ES_Las_Palmas_2023.py`.*

Finally, you can generate a report by entering `python 3_generate_resources.py example_ES_Las_Palmas_2023` (or the codename for another city, once you source its data, configure it and analyse it):

![image](images/3_generate_resources.jpg)
*Figure 5. The output of successful report generation for Las Palmas, which also provides the path to find the output PDF file(s).*

Once you’ve successfully run the process, output files will be located for the analysed city or region in an output folder (e.g. `process\data\_study_region_outputs\example_ES_Las_Palmas_2023`, located within your project directory):

![image](images/folder_process_data_output.jpg)
*Figure 6. The list of output files and folders resulting from running the full 3-step process; these are explained later in this guide.*

If anything goes wrong (e.g. you tried to run any of the other cities, for which data haven’t been provided), the process will stop and indicate some kind of error.  A log file will be generated in the study region folder that can assist with debugging, or otherwise verifying the process has been successful and let you see some of the details of how things are processed (we’ll go into details later).  In the above screenshot this is called `__Las Palmas de Gran Canaria__example_ES_Las_Palmas_2023_processing_log.txt`.

As a record for how you ran your analysis, the file `_parameters.yml` contains a record of your project, region and data configurations for this region.  Spatial features used in analysis as well as grid and overall city summaries are saved within a geopackage file, and CSV files of final summary results are provided too.  The PDF reports are located within the folder `_web reports`, while the maps and images generated for use in that report are located in the `figures` folder.

![image](images/folder_process_data_output_figures.jpg)
*Figure 7. Generated plot and map figures from the analysis of Las Palmas, with annotations in English and Spanish.*

![image](images/folder_process_data_output_reports.jpg)
*Figure 8. PDF reports in English and Spanish for Las Palmas.*

![image](images/pdf_report.jpg)
*Figure 9. Example page from the Spanish PDF policy and spatial indicators report for Las Palmas (policy results are for illustration purposes only, not real!).*

**Note that the example data used for the policy review is fictitious; we have not reviewed the policy of Las Palmas.  It has just been provided for demonstration purposes for the concept.  The policy review instrument will be updated later in 2023, with subsequent software updates to accommodate this.**

# 1. Configuration

Before commencing analysis, your study regions will need to be configured.

Study regions are configured using .yml files located within the `configuration/regions` sub-folder. An example configuration for Las Palmas de Gran Canaria (for which data supporting analysis is included) has been provided in the file `process/configuration/regions/example_ES_Las_Palmas_2023.yml`, and further additional example regions have also been provided.  The name of the file, for example `example_ES_Las_Palmas_2023`, acts a codename for the city when used in processing and avoids issues with ambiguity when analysing multiple cities across different regions and time points: 'example' clarifies that this is an example, 'ES' clarifies that this is a Spanish city, 'Las_Palmas' is a common short way of writing the city's name, and the analysis uses data chosen to target 2023.  Using a naming convention like this is recommended when creating new region configuration files (e.g. ES_Barcelona_2023.yml, or AU_Melbourne_2023.yml).

The study region configuration files have a file extension .yml (YAML files), which means they are text files that can be edited in a text editor to define region specific details, including which datasets used - eg cities from a particular region could share common excerpts of population and OpenStreetMap, potentially).

Additional configuration may be required to define datasets referenced for regions, configuration of language for reporting using the following files:

`datasets.yml`: Configuration of datasets, which may be referenced by study regions of interest

`\_report\_configuration.xlsx`: (required to generate reports) Use to configure generation of images and reports generated for processed cities; in particular, the translation of prose and executive summaries for different languages.  This can be edited in a spreadsheet program like Microsoft Excel.

`config.yml`: (optional) Configuration of overall project, including your time zone for logging start and end times of analyses

Optional configuration of other parameters is also possible. 

To view the above directions, and more information on generating the required configuration files (further detailed below), run the first script by entering:

```
python 1_create_project_configuration_files.py
```

To initialise a new study region configuration file, you can run this script again providing your choice of codename, for example:

```
python 1_create_project_configuration_files.py example_ES_Las_Palmas_2023
```

or, for the Australian city of Melbourne to be analysed with data targetting 2023,

```
python 1_create_project_configuration_files.py AU_Melbourne_2023
```

Running this script in this way will generate the configuration files used when running analyses that can be modified using a text editor as required.


![image](images/1_create_project_configuration_files.jpg)
*Figure 10. Output resulting from creating the configuration files; if you were to run this a second time after successfully running it, it would recognise and report that the files already exist, and otherwise remind you about the purpose of the respective configuration files.*

As per the above screenshot, the configuration files listed in the table below will be copied to the `process/configuration` folder. These may be be edited in a text editor or in a spreadsheet editor such as Excel for the CSV file) to add and customise analysis for new regions.  You will need to edit the regions/_codename_.yml file if configuring a new city.   You will also likely need to modify the datasets.yml file, unless the data that is already described there suits your purpose, and you source it if not supplied.  Dataset entries describe where these files are to be stored (`data_dir`), as well as where they are sourced from. Only a small example set of datasets have been provided (relating to the city of Las Palmas), so you will need to retrieve data for other cities and configure this as per the example entries.

| Configuration file   | File description                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **regions/_codename_.yml**      | Used to define and specify details for study regions to be analysed.  Records in datasets.yml for population, OpenStreetMap,  and urban region data are cross-referenced as these will often be shared across multiple cities.   City-specific sets of GTFS transit feed data are also defined directly under regions configured in this file. ** Generate this file by running the configuration script followed by a codename of your choice, then open and edit the resulting text file writing over the existing entries to guide you.**   |
| datasets.yml         | Used to define key datasets and their associated metadata                                                                                                                                                                                                                                                                                                                                                                                                            |
| config.yml           | This file contains the description and definitions of the general project parameters (e.g. your local time zone, for accurate localised recording of start and end times of processing in log files).                                                                                                                                                                                                                                                                |
| indicators.yml       | Some aspects of indicators calculated can optionally be modified here; currently this is set up for the core indicators of interest for the  [1000 City Challenge](https://www.healthysustainablecities.org/1000cities).                                                                                                                                                                                                                                             |
| osm_destinations.csv | Used to classify destinations to be retrieved from OpenStreetMap and support calculation of indicators defined in indicators.yml (e.g. percentage of population with access to a particular kind of amenity)                                                                                                                                                                                                                                                         |
| osm_open_space.yml   | Definitions for identifying areas of open space using OpenStreetMap                                                                                                                                                                                                                                                                                                                                                                                                  |
| policies.yml         | A list of policies for reporting.  This is a proof of concept for now, modelled of the previous 25-city study.  An update is anticipated mid-2023, following release of new methodology to support the 1000 Cities Challenge.                                                                                                                                                                                                                                        |

Optionally, projects can be configured to:

- analyse [GTFS feed data](https://database.mobilitydata.org/) for evaluating accessibility to regularly serviced public transport
- use custom sets of OpenStreetMap tags for identifying destinations (see [OpenStreetMap TagInfo](https://taginfo.openstreetmap.org/) and region-specific tagging guidelines to inform relevant synonyms for points of interest)
- use custom destination data (a path to CSV with coordinates for points of interest for different destination categories can be configured in `process/configuration/regions/_codename_.yml`)

## Regions
New regions can be added or their analysis modified by adding to and editing the regions/_codename_.yml file, in conjunction with the datasets.yml file.  

<details>
  <summary>
    Click to view description of region configuration parameters
  </summary>

```
name: Full study region name, e.g. Las Palmas de Gran Canaria
year: Target year for analysis, e.g. 2023
country: Fully country name, e.g. España
country_code: Two character country code (ISO3166 Alpha-2 code), e.g. ES
continent: Continent name, e.g. Europe
crs:
  name: name of the coordinate reference system (CRS), e.g. REGCAN95 / LAEA Europe
  standard: acronym of the standard catalogue defining this CRS, eg. EPSG
  srid: spatial reference identifier (SRID) integer that identifies this CRS according to the specified standard, e.g. 5635 (see https://spatialreference.org/, or search for what is commonly used in your city or country; e.g. a national CRS like those listed at https://en.wikipedia.org/wiki/List_of_national_coordinate_reference_systems )
  utm: UTM grid if EPSG code is not known (used to manually derive EPSG code= 326** is for Northern Hemisphere, 327** is for Southern Hemisphere), e.g. 28N (see https://mangomap.com/robertyoung/maps/69585/what-utm-zone-am-i-in- )
study_region_boundary:
  data: Region boundary data (path relative to project directory, or GHS:variable='value'), e.g. "region_boundaries/Example/Las Palmas de Gran Canaria - Centro Nacional de Información Geográfica - WGS84 - EPSG4326.geojson"
  source: The name of the provider of this data, e.g. Centro Nacional de Información Geográfica
  publication_date: Publication date for study region area data source, or date of currency, e.g. 2019-02-01
  url: URL for the source dataset, or its provider, e.g. https://datos.gob.es/en/catalogo/e00125901-spaignllm
  licence: Licence for the data, e.g. CC-BY-4.0
  licence_url: URL for the data licence, e.g. https://www.ign.es/resources/licencia/Condiciones_licenciaUso_IGN.pdf
  ghsl_urban_intersection: Whether the provided study region boundary will be further restricted to an urban area defined by its intersection with a linked urban region dataset (see urban_region), e.g. true
population: the population record in datasets.yml to be used for this region, e.g. example_las_palmas_pop_2022
OpenStreetMap: the OpenStreetMap record defined in datasets.yml to be used for this region, e.g. las_palmas_20230221
network:
  osmnx_retain_all: If false, only retain main connected network when retrieving OSM roads, e.g. false
  buffered_region: Recommended to set to 'true'.  If false, use the study region boundary without buffering for network extraction (this may appropriate for true islands, but could be problematic for anywhere else where the network and associated amenities may be accessible beyond the edge of the study region boundary).
  polygon_iteration: Iterate over and combine polygons (this may be appropriate for a series of islands), e.g. false
  connection_threshold: Minimum distance to retain (a useful parameter for islands, like Hong Kong, but for most purposes you can leave this blank)
  intersection_tolerance: Tolerance in metres for cleaning intersections.  This is an important methodological choice, and the chosen parameter should be robust to a variety of network topologies in the city being studied.  See https://github.com/gboeing/osmnx-examples/blob/main/notebooks/04-simplify-graph-consolidate-nodes.ipynb. For example: 12
urban_region: the urban region dataset defined in datasets.yml to be optionally used for this region (such as data from the Global Human Settlements Layer Urban Centres Database, GHSL-UCDB), e.g. GHS-Example-Las-Palmas
urban_query: GHS or other linkage of covariate data (GHS:variable='value', or path:variable='value' for a dataset with equivalently named fields defined in project parameters for air_pollution_covariates), e.g. GHS:UC_NM_MN=='Las Palmas de Gran Canaria' and CTR_MN_NM=='Spain'
country_gdp:
  classification: Country GDP classification, e.g. lower-middle
  reference: Citation for the GDP classification, e.g. The World Bank. 2020. World Bank country and lending groups. https://datahelpdesk.worldbank.org/knowledgebase/articles/906519-world-bank-country-and-lending-groups
covariate_data: additional covariates to be linked (currently this is designed to direct the process to retrieve additional city statistics from the GHSL UCDB entry for the city, where available), e.g. urban_query
custom_destinations: Details of custom destinations to use (e.g. as done for Maiduguri, Nigeria), in addition to those from OSM (optional, as required; else, leave blank) file name (located in study region folder), category plain name field, category full name field, Y coordinate, X coordinate, EPSG number, attribution
gtfs_feeds: Details for city specific parent folder (within GTFS data dir defined in config.yml) and feed-specific unzipped sub-folders containing GTFS feed data.  For each feed, the data provider, year, start and end dates to use, and mapping of modes are specified (only required if feed does not conform to the GTFS specification https://developers.google.com/transit/gtfs/reference). For cities with no GTFS feeds identified, this may be left blank.  Otherwise, copy the example in the example_ES_Las_Palmas_2023.yml file or other example files where multiple GTFS feeds for different modes have been defined to get started and customise this for your city.
policy_review: Optional path to results of policy indicator review for inclusion in generated reports
notes: Notes for this region
```
</details>


<details>
  <summary>
    Click to view example completion of a record in this file for Las Palmas de Gran Canaria, Spain, using the codename `example_ES_Las_Palmas_2023`.
  </summary>

```
name: Las Palmas de Gran Canaria
year: 2023
country: Spain
country_code: ES
continent: Europe
crs:
  name: REGCAN95 / LAEA Europe
  standard: EPSG
  srid: 5635
  utm: 28N
study_region_boundary:
  data: "region_boundaries/Example/Las Palmas de Gran Canaria - Centro Nacional de Información Geográfica - WGS84 - EPSG4326.geojson"
  source: Centro Nacional de Información Geográfica
  publication_date: 2019-02-01
  url: https://datos.gob.es/en/catalogo/e00125901-spaignllm
  licence: CC-BY-4.0
  licence_url: https://www.ign.es/resources/licencia/Condiciones_licenciaUso_IGN.pdf
  ghsl_urban_intersection: false
  citation: "Instituto Geográfico Nacional (2019). Base de datos de divisiones administrativas de España. https://datos.gob.es/en/catalogo/e00125901-spaignllm.
  notes: manually extracted municipal boundary for Las Palmas de Gran Canaria in WGS84 from the downloaded zip file 'lineas_limite.zip' using QGIS to a geojson file for demonstration purposes."
population: example_las_palmas_pop_2022
OpenStreetMap: las_palmas_20230221
network:
  osmnx_retain_all: false
  buffered_region: true
  polygon_iteration: false
  connection_threshold:
  intersection_tolerance: 12
urban_region: GHS-Example-Las-Palmas
urban_query: GHS:UC_NM_MN=='Las Palmas de Gran Canaria' and CTR_MN_NM=='Spain'
country_gdp:
  classification: High-income
  reference: The World Bank. 2020. World Bank country and lending groups. https://datahelpdesk.worldbank.org/knowledgebase/articles/906519-world-bank-country-and-lending-groups
covariate_data: urban_query
custom_destinations:
  file:
  dest_name:
  dest_name_full:
  lat:
  lon:
  epsg:
  attribution:
gtfs_feeds:
  folder: Example
  gtfs_es_las_palmas_de_gran_canaria_guaguas_20230222:
    gtfs_provider: Guaguas
    gtfs_year: 2023
    gtfs_url: http://www.guaguas.com/transit/google_transit.zip
    start_date_mmdd: 20230405
    end_date_mmdd: 20230605
    modes:
policy_review: process/data/policy_review/_policy_review_template_v0_TO-BE-UPDATED.xlsx
notes:
```
</details>

## Datasets

Below is an example of the example datasets for analysing Las Palmas,  included when you download the global-indicators software.  For the core data sets (OpenStreetMap, population, and urban region) we describe where the data is stored (`data_dir`), as well as details on where it was sourced from and how it should be cited when reported on findings.  There are also additional details, such as the spatial coordinate reference system, that allow the data to be correctly read in and analysed in conjunction with other datasets.  There is also a default schema for reading in GTFS (transit feed) data, that can be optionally over-ridden as required for specific regions in regions/_codename_.yml (you may mot need to edit this, if your GTFS data conform to the schema at https://developers.google.com/transit/gtfs).

<details>
  <summary>
    Click to view dataset.yml configuration details below:
  </summary>
  
```
OpenStreetMap:
    las_palmas_20230221:
        data_dir: OpenStreetMap/Example/example_ES_Las_Palmas_2023_osm_20230221.pbf
        # data_dir: OpenStreetMap/las_palmas-latest_2023-02-21.osm.pbf
        source: OpenStreetMap.fr
        publication_date: 20230221   
        url: https://download.openstreetmap.fr/extracts/africa/spain/canarias/las_palmas-latest.osm.pbf
        note: This is configured with a derived excerpt from the larger OpenStreetMap dataset for Las Canarias based on the 1600m buffered municipal boundary of Las Palmas de Gran Canaria to reduce file size for demonstration purposes.
population:
    example_las_palmas_pop_2022:
        pop_min_threshold: 5
        # urban sample points intersecting grid cells with estimated population less than this will be excluded from analysis
        alias: ghs_pop_2020_2022a_example_las_palmas
        name: "Global Human Settlements population data: 2020, Mollweide (EU JRC, 2022)"
        data_name: GHS-POP
        data_dir: population_grids/Example/GHS_POP_E2020_GLOBE_R2022A_54009_100_V1_0_R6_C17
        data_type: raster:Int64
        crs_name: Mollweide
        crs_standard: ESRI
        crs_srid: 54009
        source_url: https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/GHSL/GHS_POP_GLOBE_R2022A/GHS_POP_E2020_GLOBE_R2022A_54009_100/V1-0/tiles/GHS_POP_E2020_GLOBE_R2022A_54009_100_V1_0_R6_C17.zip
        date_acquired: 20230222
        year_published: 2022
        year_target: 2020
        provider: EU JRC
        licence: CC BY 4.0
        licence_url: https://creativecommons.org/licenses/by/4.0/deed.ast
        resolution: 100m
        raster_band: 1
        raster_nodata: -200
        citation: "Schiavina, Marcello; Freire, Sergio; MacManus, Kytt (2022): GHS-POP R2022A - GHS population grid multitemporal (1975-2030). European Commission, Joint Research Centre (JRC) [Dataset] doi: 10.2905/D6D86A90-4351-4508-99C1-CB074B022C4A"

urban_region:
    GHS-Example-Las-Palmas:
        name: "Global Human Settlements urban centres: 2015 (EU JRC, 2019; Las Palmas de Gran Canaria only)"
        data_dir: "urban_regions/Example/Las Palmas de Gran Canaria - GHS_STAT_UCDB2015MT_GLOBE_R2019A_V1_2.gpkg"
        data_type: vector
        epsg_name: WGS84
        epsg: 4326
        citation: "Florczyk, A. et al. (2019): GHS Urban Centre Database 2015, multitemporal and multidimensional attributes, R2019A. European Commission, Joint Research Centre (JRC). https://data.jrc.ec.europa.eu/dataset/53473144-b88c-44bc-b4a3-4583ed1f547e"
        covariates:
            E_EC2E_T15:
                Units: tonnes per annum
                Unit description: tonnes per annum
                Description: Total emission of CO 2 from the transport sector, using non-short-cycle-organic fuels in 2015
            E_EC2O_T15:
                Units: tonnes per annum
                Unit description: tonnes per annum
                Description: Total emission of CO 2 from the energy sector, using short-cycle-organic fuels in 2015
            E_EPM2_T15:
                Units: tonnes per annum
                Unit description: tonnes per annum
                Description: Total emission of PM 2.5 from the transport sector in 2015
            E_CPM2_T14:
                Units: µg per cubic metre
                Unit description: micrograms per cubic meter
                Description: Total concertation of PM 2.5 for reference epoch 2014
gtfs:
    data_dir: transit_feeds
    headway: pt_stops_headway
    default_modes:
        Tram       : {'route_types': [ 0],'agency_id': }
        Metro      : {'route_types': [ 1],'agency_id': }
        Rail       : {'route_types': [ 2],'agency_id': }
        Bus        : {'route_types': [ 3],'agency_id': }
        Ferry      : {'route_types': [ 4],'agency_id': }
        Cable tram : {'route_types': [ 5],'agency_id': }
        Aerial lift: {'route_types': [ 6],'agency_id': }
        Funicular  : {'route_types': [ 7],'agency_id': }
        Trolleybus : {'route_types': [11],'agency_id': }
        Monorail   : {'route_types': [12],'agency_id': }
        # as per https://developers.google.com/transit/gtfs/reference
    analysis_period: ['07:00:00', '19:00:00']
    headway_intervals: [20,30]
```
</details>

When configuring new study region(s), some input data will be required.  This is to be stored in sub-folders within the `process/data` folder. Examples of required data include a population grid, an excerpt from OpenStreetMap, and a study region boundary (either an administrative boundary, or an urban region from the Global Human Settlements Urban Centres Database).  Other data are optional: GTFS transit feed data, or a policy review analysis.  The kinds of data that can be configured for usage are summarised in the below table, and we have provided examples for each of these for Las Palmas (noting that the example policy review data is fictional; we have not conducted a policy review for Las Palmas yet!).

|Usage note    | Data sub-folder     | Purpose        |
|--------------|--------------------|----------------|
|Required      | OpenStreetMap      | Representing features of interest |
|Required      | population_grids   | Representing population distribution |
|Conditional   | region_boundaries  | Identifying study region |
|Conditional   | urban_regions      | Identifying urban and/or study region |
|Optional      | policy_review      | Summarising results of a policy review analysis |
|Optional      | transit_feeds      | Collections of GTFS feeds to represent public transport service frequency |
|Optional      | other_custom_data  | Other custom data, such as points of interest |

### Data storage
Specific paths to data are configurable in the configuration files.  However, in general, it is recommended that input data be stored within subfolders of the data folder as follows:

- data
    - OpenStreetMap
        - [_a .pbf or .osm excerpt of OpenStreetMap; should be dated with a suffix to indicate publication date e.g. "las_palmas-latest.osm_20230210.pbf" or "oceania_yyyymmdd.pbf" where yyyy is the year, mm is the 2-digit numerical month, and dd is the 2-digit numerical day date_].  
    - population_grids
        -  [_a sub-folder with a descriptive name for a subset of tiles from [GHSL](https://ghsl.jrc.ec.europa.eu/download.php?ds=pop) or a national grid, e.g. "GHS_STAT_UCDB2015MT_GLOBE_R2019A" or "GHS_POP_P2030_GLOBE_R2022A_54009_100_V1_0"_] 
            - [_one or more .tif files and associated metadata, e.g. "GHS_POP_P2030_GLOBE_R2022A_54009_100_V1_0_R14_C12.tif"_] *you can download specific tile(s) for your area of interest by clicking on the GHS population grid map*
    - region_boundaries
        - [_optional subfolder for grouping boundary files, e.g. "Belgium"]
            - [_a specific file with the region boundary, e.g. "adminvector_3812.gpkg" or "ghent_3812.geojson"_] 
        - [_a specific file with the region boundary (ie. optionally not in a sub-folder)]
    - urban_regions [_optionally, a study region can be defined by or restricted to a defined urban region from the [Global Human Settlements Urban Centres Database](https://ghsl.jrc.ec.europa.eu/download.php?ds=ucdb)_]
        - GHS_STAT_UCDB2015MT_GLOBE_R2019A_V1_2.gpkg
    - transit_feeds
        - [_some region specific GTFS parent folder, e.g. "Belgium"_]
            - [_one or more sub-folders containing unzipped files from GTFS feeds, e.g. de_lijn-gtfs_20230117, where the suffix indicates date of publication (feed metadata is also recorded in region config file)_]
    - policy_review (_in development_)
        - An Excel workbook grading policies corresponding to the policies configured in policies.yml

### OpenStreetMap data

OpenStreetMap data are used to represent features of interest within regions of interest.

This can be retrieved in .pbf (recommended; smaller file size) or .osm format from sites including:

- https://download.geofabrik.de/
- http://download.openstreetmap.fr/extracts/
- https://planet.openstreetmap.org/pbf/  (whole of planet data archives; very large file sizes, but can be useful for retrieving historical data)

It is recommended that a suffix to indicate publication date is added to the name of downloaded files as a record of the time point at which the excerpt is considered representative of the region of interest e.g. "las_palmas-latest.osm_20230210.pbf" or "oceania_yyyymmdd.pbf" where yyyy is the year, mm is the 2-digit numerical month, and dd is the 2-digit numerical day date_.

The main considerations are that the excerpt is as small as possible while ensuring that it has complete coverage of the region(s) of interest (and about 1600 metres additional beyond the boundary, or as otherwise configured).  Using a smaller rather than a larger file will speed up processing.

For example, for a project considering multiple cities in Spain, the researchers could download an excerpt for the country of [Spain](https://download.geofabrik.de/europe/spain.html) or for specific sub-regions like [Catalunya](https://download.geofabrik.de/europe/spain/cataluna.html) as required to ensure that the region of interest is encompassed within the extract.  Using the example of Spain, which also contains regions outside Europe, if sourcing from Geofabrik as the example links above, it is worth noting that the excerpts are grouped by continent so some care should be taken.  For example, [Las Palmas de Gran Canaria](https://download.geofabrik.de/africa/canary-islands.html) are found under Canary Islands in Africa on Geofabrik, or a smaller PBF specifically for [Las Palmas](https://download.openstreetmap.fr/extracts/africa/spain/canarias/) can be retrieved from download.openstreetmap.fr under 'africa/spain/canarias'.  Our software provides example data for Las Palmas, and to reduce file size we prepared a clipped portion of the data around this city using only the minimum area required..

*Metadata for the stored OpenStreetMap data should be recorded in `datasets.yml`.*

### Population grid data

Population grid data are used to represent the population distribution within regions of interest.

We recommend usage and configuration of the [R2022a](https://ghsl.jrc.ec.europa.eu/download.php?ds=pop) or more recent population data from the Global Human Settlements Layer project, with a time point of 2020 (or as otherwise appropriate for your project needs, and bearing in mind the limitations of the GHSL population model) and using Mollweide (equal areas, 100m) projection.  *note: take care when retrieving the GHSL population data to select the correct year! The default at the time of writing is a population projection for 2030, however currently the most relevant dataset are the 2020 estimates; make sure to select 2020*

A whole-of-planet population dataset for 2020 can be downloaded from the following link:
https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/GHSL/GHS_POP_GLOBE_R2022A/GHS_POP_E2020_GLOBE_R2022A_54009_100/V1-0/GHS_POP_E2020_GLOBE_R2022A_54009_100_V1_0.zip

This file is approximately 5 GB in size, so if you are only analysing one or a few cities, it may be worth identifying the specific tiles  that relate to these using the interactive map downloader provided by GHSL and storing them as suggested above.

The report describing this data is located [here](https://ghsl.jrc.ec.europa.eu/documents/GHSL_Data_Package_2022.pdf?t=1655995832).

The citation for the data is:
Schiavina M., Freire S., MacManus K. (2022):
GHS-POP R2022A - GHS population grid multitemporal (1975-2030).European Commission, Joint Research Centre (JRC). https://doi.org/10.2905/D6D86A90-4351-4508-99C1-CB074B022C4A

Optionally, official population data can be used if this is judged to be more accurate or meaningful for your city's analysis and interpretation of results by your intended audience.  For example, to use the official 1km Australian population grid for 2021 (instead of the 100m grid estimates from the GHSL-POP r2022a dataset for 2020 or 2025), this could be specified as per the following code block:

```
population:
    australia_population_2021:
        pop_min_threshold: 5
        # urban sample points intersecting grid cells with estimated population less than this will be excluded from analysis
        alias: APG21e_1_0_0
        name: "Australian Population Grid 2021 (ABS, 2023)"
        data_name: APG21e_1_0_0
        data_dir: population_grids/Australian_Population_Grid_2021_in_TIFF_format
        data_type: raster:Int64
        crs_name: GDA94 / Australian Albers
        crs_standard: EPSG
        crs_srid: 3577
        source_url: https://www.abs.gov.au/statistics/people/population/regional-population/2021/GEOTIFF.zip
        date_acquired: 20230302
        year_published: 2022
        year_target: 2021
        provider: EU JRC
        licence: CC BY 4.0
        licence_url: https://www.abs.gov.au/website-privacy-copyright-and-disclaimer#copyright-and-creative-commons
        resolution: 1km
        raster_band: 1
        raster_nodata: 
        citation: "Australian Bureau of Statistics (2022), Australian Population Grid 2021. https://www.abs.gov.au/statistics/people/population/regional-population/2021"
```

Choice of population data grid is an important methodological choice for analysts as it determines the resolution at which results are output (for example a choice between 100m estimated population, or 1000m official population statistic).  As such, it is good to know that this is an option available to ensure your indicator results serve their intended purpose and audience.

*Metadata for the stored population data should be recorded in `datasets.yml`.*

### Region boundary data

Region boundary data are used to identify the study region(s) of interest.  These may be retrievable from a government open data portal, or the UN OCHA [Humanitarian Data Exchange](https://data.humdata.org/).  *Metadata for the stored boundary data for specific cities should be recorded in `regions/_codename_.yml`.*

### Urban region data

Urban region data are used to optionally identify and restrict analysis to the urban portion of a study region, and optionally can also be used to identify an urban region of interest (e.g. an urban agglomeration surrounding a major city).  Additionally, city-level urban covariates contained within the database may be linked with the final estimates (e.g. air pollution estimates).

This is done using the Global Human Settlements Urban Centres Database, for example [GHS_STAT_UCDB2015MT_GLOBE_R2019A_V1_2.gpkg](https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/GHSL//GHS_STAT_UCDB2015MT_GLOBE_R2019A/V1-2/GHS_STAT_UCDB2015MT_GLOBE_R2019A_V1_2.zip) (or a more recent release, if available), which may be retrieved from https://ghsl.jrc.ec.europa.eu/download.php?ds=ucdb.

The dataset linked above represents urban centres in 2015, and may be cited as:
Florczyk A., Corbane C,. Schiavina M., Pesaresi M., Maffenini L., Melchiorri, M., Politis P., Sabo F., Freire S., Ehrlich D., Kemper T., Tommasi P., Airaghi D., Zanchetta L. (2019)
GHS Urban Centre Database 2015, multitemporal and multidimensional attributes, R2019A. European Commission, Joint Research Centre (JRC)PID: https://data.jrc.ec.europa.eu/dataset/53473144-b88c-44bc-b4a3-4583ed1f547e

*Metadata for the stored urban region data should be recorded in `datasets.yml`.*

### Policy review data

The global-indicators software has been designed to report on both policy and spatial indicator results.  The integration of policy results is in development, and pending an updated policy review survey instrument.  The beta implementation has been carried out using the policy review analysis used in the 25-city study published in 2022, and draft templates are available on request.  It is anticipated that the updated policy review instrument will be incorporated for reporting purposes later in 2023.  In the meantime, an example set of policy data from the 25 city study has been provided to provide an indication of how this can be used to generate a city report containing policy checklists.

*Policy review data files are aligned with regions in `regions/_codename_.yml`.*

### GTFS transit feed data

Collections of GTFS feeds are used to represent public transport service frequency.  More than one transit operator may operate in the region of interest, so it is important to aim to have full coverage of both the region and the operators, without overlap of services, to represent the service frequency of public transport stops in a city.

Transit feed data may be available from Government open data portals, or otherwise from aggregator sites such as https://transitfeeds.com/ and https://www.transit.land/.

We recommend that feeds be stored in a region specific GTFS parent folder, e.g. "Spain - Valencia", that contains one or more sub-folders containing unzipped files from GTFS feeds, e.g. gtfs_spain_valencia_EMT_20190403 and gtfs_spain_valencia_MetroValencia_20190403, where the suffix indicates date of publication.

*Metadata for the stored GTFS transit feed data should be recorded for specific cities in `regions/_codename_.yml`.*

### Data output folder

The results of analyses will be output to study region specific sub-folders within the `data/_study_region_outputs` directory

The sub-folders have a naming schema reflecting the convention:

[city code name]_[2-letter country code]_[yyyy]

For example, ghent_be_2020 or valencia_v2_es_2020.

Following completion of successful processing of a study region, the following provides an indication of the contents of this folder:

|Item                                                            | Type | Description |
|----------------------------------------------------------------|------|-------------|
|_web reports                                                    | Folder| Contains generated policy and spatial indicator reports, optionally in multiple languages|
|figures                                                         | Folder| Contains generated maps and figures|
|\_\_*region name*\_\_*codename*_processing_log.txt | text file| A text file that is progressively appended to with the screen outputs for analyses that are not otherwise displayed.  This contains a record of processing, and is useful when debugging if something has gone awry with a particular configuration or supplied data|
|_parameters.yml | text file| A text file containing records of the most recent configuration analysed (on detection of a new version, older versions will be retained with a suffix indicating the date at which it was current)|
|*codename*_es_yyyy_1600m_buffer.gpkg | geopackage| A geopackage containing derived study region features of interest used in analyses, and including grid and overall summary results for this region |
|*codename*_es_yyyy_1600m_pedestrian_osm_yyyymmdd.graphml | graphml| A graphml representation of the derived routable pedestrian network for this buffered study region |
|*codename*_es_yyyy_1600m_pedestrian_osm_yyyymmdd_proj.graphml | graphml| A graphml representation of the derived routable pedestrian network for this buffered study region, projected with units in metres |
|*codename*_es_yyyy_city_yyyy-mm-dd.csv | CSV file| Overall summary results of indicator analysis for this region |
|*codename*_es_yyyy_grid_100mm_yyyy-mm-dd.csv | CSV file| Grid summary results of indicator analysis for this region |
|*codename*_osm_yyyymmdd.pbf | PBF file| An excerpt from OpenStreetMap for this buffered study region as configured |
|poly_li_*codename*_yyyy.poly | text file| A polygon boundary file; this is generated for the buffered urban region of interest as per configuration in regions/_codename_.yml, and is used to excerpt a portion of OpenStreetMap for this region from the configured input data|
|*population alias*_*codename*_*epsg code*.tif | raster image| A population raster for this buffered study region, excerpted from the input data, in the projects coordinate reference system |
|*population alias*_*codename*_*crs*.tif| raster image| A population raster for this buffered study region, excerpted from the input data, in the coordinate reference system of the input population data (e.g. Mollweide, in the case of the recommended GHS-POP data) |

Optionally, projects can be configured to:

- analyse [GTFS feed data](https://database.mobilitydata.org/) for evaluating accessibility to regularly serviced public transport
- use custom sets of OpenStreetMap tags for identifying destinations (see [OpenStreetMap TagInfo](https://taginfo.openstreetmap.org/) and region-specific tagging guidelines to inform relevant synonyms for points of interest)
- use custom destination data (a path to CSV with coordinates for points of interest for different destination categories can be configured in `process/configuration/regions/_codename_.yml`)
  
# 2. Analyse neighbourhoods
To analyse a configured region, enter

```python 2_analyse_region.py [*configured city codename*]```

This creates a database for the city and processes the resources required for analyses, according to the way they have been configured.

To view the code names for configured cities, you can run the script without a city name.  This displays the list of names for currently configured cities, each of which can be entered as arguments when running this script.

Local neighbourhood analysis for sample points is then performed across a city, creating urban indicators as defined in `indicators.yml`.  The following neighbourhood analyses are conducted:

- Access to amenities (food market, convenience store, open space typologies, public transport typologies)
- Daily Living score for access to amenities
- Walkability index
   - within city
        - A walkability index is calculated as a sum of standard scores (z-scores) for population density, street connectivity and daily living score for access to amenities
   - between cities
        - In addition to the within-city walkability index, a reference walkability index is also calculated. 

Finally, spatial urban indicator summaries are aggregated for a small area grid (corresponding to the resolution of the input population grid; e.g. 100 metres using the recommended GHSL population grid data) and overall city, exported as CSV (without geometry) and as layers to the geopackage file in the `data/study_region/[study region name]` folder.


# 3. Reporting

To generate maps, figures and reports for the results, run

```python 3_generate_resources.py [CITY CODE NAME]```

This script is used to generate reports, optionally in multiple languages, for processed cities.  It integrates the functionality previously located in the repository https://github.com/global-healthy-liveable-cities/global_scorecards, which was used to generate [city reports](https://doi.org/10.25439/rmt.c.6012649) for our 25 city study across 16 languages.  These can be configured using the configuration file _report_configuration.xlsx in conjunction with the regions, indicators and policies configuration files.

At the time of writing (February 2023) the included report template is the same as that used in the 25 city study, outputting a PDF report of policy and spatial indicator results, designed for web distribution.  In addition a folder of maps and figures are also produced, optionally in multiple languages, pending configuration.  Further templates are planned to support high quality print output, and optionally select policy and/or spatial indicators for reporting.

In addition it is planned that validation reporting with descriptive and analytical summaries will be incorporated.Currently, validation is to be carried out by users independent of the process.  


