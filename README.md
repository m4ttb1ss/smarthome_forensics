# Smarthome forensics app

## Purpose
This was developed while a master's thesis in the Master's program "Digital Forensics" at the university of applied sciences Albstadt-Sigmaringen by Matthias Bissinger.
It provides a Datamodel named "Smarthome" which contains fields that are commonly available in various log and activity files of smarthome components. 

## Author
Matthias Bissinger

## Prerequisites
It is helpful to have the "Lookup table editor" app installed to edit the investigation_ids lookup table what needs to be done to add necessary forensic investigation information. If the app is not present, the csv file needs to be edited directly on disk.

## Datamodel compliance
The datamodel constraints are the indexes in macro "smarthome_indexes" and tag=smarthome. The only required field is the field "message" which needs to be present or extracted or calculated from the log source being ingested. 