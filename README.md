# Smarthome forensics app

## Purpose
This app was developed as part of a thesis in the Master's program "Digital Forensics" at Albstadt-Sigmaringen University by Matthias Bissinger.
It provides a Datamodel named "Smarthome" which contains fields that are commonly available in various log and activity files of smarthome components. 

## Author
Matthias Bissinger

## Prerequisites
It is helpful to have the "Lookup table editor" addon installed to edit the investigation_ids lookup table what needs to be done to add necessary forensic investigation metadata. If the app is not present, the csv file needs to be edited directly on disk.

## Datamodel compliance
The datamodel constraints are the indexes in macro "smarthome_indexes" and tag=smarthome. The only required field is the field "message" which needs to be present, extracted or calculated from the log source being ingested. All additional fields are optional, but recommended to map them to the datamodel fields if possible.

## Default indexes
The app creates a 5GB sized index named "smarthome" which is by default added to the "smarthome_indexes" macro. 
Additionally it creates a 5GB index named "investigation" which is for future purposes. In case you don't want to have that created, remove it from *default/indexes.conf* as it's not required in the current version.

## Lookups

### Investigation IDs
The lookup table "investigation_ids.csv" with the according lookup definition "investigation_ids" contains all sources that were ingested into the "smarthome_indexes".

Open the csv file and enter your specific metadata which are investigation case ID, artifact ID and file hash. These data then will be added to the apprpiate dashboards.

### App dependencies
The lookupup table "app_dependencies.csv" with the according lookup definition contains the required and recommended apps for the best experience of the app.

## Usage

The dashboard "Investigation" shows all data which are mapped to the *Smarthome*-Datamodel. Additionally, it shows the metadata added in the investigation_ids lookup. 
The dashboard "Search history contains all search queries which were generated due to the usage of the "Investigation" dashboard. 

Under "Indexed data sources" all files which were indexed are listed with there appropriate metadata once added in the investigation_ids lookup table. 

## Permissions and roles
The app ships with a predefined role "investigator". This role inherits from "user" and is extended by having access to the *_audit* index. The latter one is necessary to make usage of the Search history dashboard. 
Add any new user to the "investigator" role, except the user should be an admin.

## Data integrity
The predefined index "smarthome" has the *enableDataIntegrityControl* flag set. With that enabled, you can control and verify the integrity of data anytime (see [Data integrity control](https://docs.splunk.com/Documentation/Splunk/8.2.2/Security/Dataintegritycontrol#Check_your_hashes_to_validate_your_data)).

## Add data

### Add data for existing sourcetypes

1. Go to *Settings -> Add data*
2. Upload your file
3. Choose the correct sourcetype
4. Set the hostname according to the name of the data source you gathered the log from
5. Choose index "smarthome"
6. Search for your data and make sure it is tagged with "smarthome" (what it should be due the existing sourcetype configuration)

Currently the following sourcetypes are available:
- ets5:bus:monitor
- homematic:ccu2:systemprotokoll
- openhab:events
- openhab:log
- smarthomeng:log
- amazon:alexa:deviceStateHistory

### Add data for non-existing sourcetypes

1. Go to *Settings -> Add data*
2. Upload your file
3. In the "Choose sourcetype" windows click save as and name the new sourcetype
4. Set the hostname according to the name of the data source you gathered the log from
5. Choose index "smarthome"
6. Click on "Extract data" and start extracting the data from the raw log
7. Add field aliases for existing fields to map the fields to the Smarthome datamodel
8. Add your sourcetype to the list in eventtype "smarthome"

After adding the sourcetype to the eventtype it will be tagged automatically and the data will be available in the Investigation dashboard. CAUTION: The field *message* needs to present, either by a field alias or a calculated field.


