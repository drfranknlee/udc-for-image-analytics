# Demo -- Unified Data Catalog for Military Aircraft


This repository includes instruction and manifest to ingest and index a military aircraft dataset into Spectrum Discover. Three additional tasks can be performed to custom-tag, extract-tag, import-tag to enrich the data catalog. 


The instruction below uses the dataset in "T101389". 



### Step 1: Upload dataset and manifest

Upload the data in /dataset into a cloud object storage bucket (eg. "udc-vault") under the folder "T101389"

Upload the manifest in /manifest into the same bucket and folder

> You can use your own dataset as long as the path to each file/object in the object storage bucket is updated


### Step 2: Scan the source using Spectrum Discover


### Step 3: Set up Tag-import Policy in Spectrum Discover

Log into the Spectrum Discover server

Create the policy json file as following

    {
            "pol_id": "T101389_aircraft_import_pol",
            "action_id": "IMPORT_TAGS",
            "action_params": {
                    "agent":"ImportTags",
                    "source_connection":"UDSt-CoS",
                    "tag_file_path":"udc-vault/T101389-aircraft-manifest.csv",
                    "tag_file_type":"csv"
            },
            "schedule": "NOW",
            "pol_state": "active",
            "pol_filter": "datasource IN ('udc-vault')"
    }


Run two commands from CLI:

    gettoken

    curl -k -H "Authorization: Bearer ${TOKEN}" https://localhost/policyengine/v1/policies/T101389_aircraft_import_pol -X POST -d @./T101389_aircraft_import.json -H "Content-Type: application/json"


This will create a Spectrum Discover metadata policy "IMPORT_TAGS" named "T101389_aircraft_import_pol". The policy will also be executed automatically. 










Source of data: https://www.kaggle.com/a2015003713/militaryaircraftdetectiondataset/version/29
