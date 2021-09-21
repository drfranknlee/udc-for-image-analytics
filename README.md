# Demo -- Unified Data Catalog for Military Aircraft


Spectrum Discover is an industry-leading unstructured data catalog platform that can be plugged into by various applications and tools for data discovery services. 


This repository includes instruction, dataset, manifest and recording to recreate a demo based on Spectrum Discover to ingest, index, tag and extract insight from a military aircraft dataset. The output is a well-curated and easily-accessible dataset ready for HPC-based preprocessing and deep learning/AI training. 


The instruction below uses the dataset in "set-T101389". The raw data is a collection of 11 military aircraft images from https://www.kaggle.com/a2015003713/militaryaircraftdetectiondataset/version/29


The step-by-step instruction and results are captured with screenshots and can be downloaded as [PDF](set-T101389/recording/T101389-Scenario1-v20210920.pdf) or [PPT](set-T101389/recording/T101389-Scenario1-v20210920.pptx)



### Step 1: Upload dataset and manifest

Upload the data in /dataset into a cloud object storage bucket (eg. "udc-vault") under the folder "T101389"

Here is an example image from the dataset:

<img src=set-T101389/dataset/00b2add164cb42440a52064e390ea3d2.jpg>


Upload the manifest in /manifest into the same bucket and folder

> Note: You can use your own dataset as long as the path to each file/object in the object storage bucket is updated


### Step 2: Scan the source using Spectrum Discover


### Step 3: Set up Tag-import Policy in Spectrum Discover

SSH log into the Spectrum Discover server. If you cannot gain SSH access to the server, you can still create the policy by using the Spectrum Discover RESTful API after obtaining the token. Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover)


Create the policy json file as following

    {
            "pol_id": "T101389_aircraft_import_pol",
            "action_id": "IMPORT_TAGS",
            "action_params": {
                    "agent":"ImportTags",
                    "source_connection":"UDSt-CoS",
                    "tag_file_path":"udc-vault/T101389/T101389-aircraft-manifest.csv",
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





