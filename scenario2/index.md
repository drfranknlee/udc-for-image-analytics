# Scenario 1: Scenario 2: Index & Tag Multi-dimensional Dataset



The step-by-step instruction and results are captured with screenshots and can be downloaded as [PDF](recording/T101389-Scenario2-v20210920.pdf) or [PPT](recording/T101389-Scenario2-v20210920.pptx)



### Step 1: Upload dataset and manifest

In your local or public cloud account, create a cloud object storage service. Start with a new vault called udc-vault and create a folder within called "T101389". All our demo data will be uploaded into this folder. 

Upload the sample data under /dataset/ into the cloud object storage "/udc-vault/T101389/scenario2/". This will create two directories: one for "raw" and the other "annotated". 

Upload the manifest csv file in /manifest into the same location "/udc-vault/T101389/scenario2/"

> Note: You can use your own dataset as long as the path to each file/object in the object storage bucket is updated


### Step 2: Scan the source using Spectrum Discover
Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover) on setting up and customizing Spectrum Discover and connecting/scanning a target data source



### Step 3: Set up Tag-import Policy in Spectrum Discover

SSH log into the Spectrum Discover server. If you cannot gain SSH access to the server, you can still create the policy by using the Spectrum Discover RESTful API after obtaining the token. Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover)


Create the policy json file as following

    {
            "pol_id": "T101389_aircraft_import_secenario2_pol",
            "action_id": "IMPORT_TAGS",
            "action_params": {
                    "agent":"ImportTags",
                    "source_connection":"UDSt-CoS",
                    "tag_file_path":"udc-vault/T101389/scenario1/T101389-aircraft-manifest-scenario2.csv",
                    "tag_file_type":"csv"
            },
            "schedule": "NOW",
            "pol_state": "active",
            "pol_filter": "datasource IN ('udc-vault')"
    }


Run two commands from CLI:

    gettoken

    curl -k -H "Authorization: Bearer ${TOKEN}" https://localhost/policyengine/v1/policies/T101389_aircraft_import_scenario2_pol -X POST -d @./T101389_aircraft_import_scenario2.json -H "Content-Type: application/json"


This will create a Spectrum Discover metadata policy "IMPORT_TAGS" named "T101389_aircraft_import_scenario2_pol". The policy will also be executed automatically. 


### Step 4: Enrich the Catalog with Multi-dimensional metadata

Use the query builder to search for the dataset: 

    filename LIKE ('%T101389/scenario2%') AND filetype LIKE ('jpg')


To refine the dataset further to only those that have been annotated: 

    filename LIKE ('%T101389/scenario2%') AND filetype LIKE ('jpg') AND filename LIKE ('%annotated2%')


Once the dataset is found and displayed, select those that meet the criteria of a new dataset to be custom-tagged. In our case shown in the recording, we select or find all 11 images that have been annotated for tagging. We'll use tag "udc-dem2" and value "T101389-s2" to create the custom-tag and this way we can easily find this dataset in the future. 


An example disply of tagged dataset is shown below. Note that we can now show additional matadata such as the dimensions of the bounding box for the aircraft, as well as the image ID that we assigned to the pair (raw and annotated). 

<img src=recording/T101389-Scenario2-result.png>

