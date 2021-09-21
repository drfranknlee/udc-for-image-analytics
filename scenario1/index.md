# Scenario 1: Scenario 1: Index Dataset & Prepare Catalog



The step-by-step instruction and results are captured with screenshots and can be downloaded as [PDF](recording/T101389-Scenario1-v20210920.pdf) or [PPT](recording/T101389-Scenario1-v20210920.pptx)



### Step 1: Upload dataset and manifest

In your local or public cloud account, create a cloud object storage service. Start with a new vault called udc-vault and create a folder within called "T101389". All our demo data will be uploaded into this folder. 

Upload the sample data under /dataset/raw into the cloud object storage "/udc-vault/T101389/scenario1/raw/"

Upload the manifest csv file in /manifest into the same location "/udc-vault/T101389/scenario1/"

> Note: You can use your own dataset as long as the path to each file/object in the object storage bucket is updated


### Step 2: Scan the source using Spectrum Discover
Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover) on setting up and customizing Spectrum Discover and connecting/scanning a target data source



### Step 3: Set up Tag-import Policy in Spectrum Discover

SSH log into the Spectrum Discover server. If you cannot gain SSH access to the server, you can still create the policy by using the Spectrum Discover RESTful API after obtaining the token. Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover)


Create the policy json file as following

    {
            "pol_id": "T101389_aircraft_import_secenario1_pol",
            "action_id": "IMPORT_TAGS",
            "action_params": {
                    "agent":"ImportTags",
                    "source_connection":"UDSt-CoS",
                    "tag_file_path":"udc-vault/T101389/scenario1/T101389-aircraft-manifest-scenario1.csv",
                    "tag_file_type":"csv"
            },
            "schedule": "NOW",
            "pol_state": "active",
            "pol_filter": "datasource IN ('udc-vault')"
    }


Run two commands from CLI:

    gettoken

    curl -k -H "Authorization: Bearer ${TOKEN}" https://localhost/policyengine/v1/policies/T101389_aircraft_import_scenario1_pol -X POST -d @./T101389_aircraft_import_scenario1.json -H "Content-Type: application/json"


This will create a Spectrum Discover metadata policy "IMPORT_TAGS" named "T101389_aircraft_import_pol". The policy will also be executed automatically. 


### Step 4: Find the dataset and create custom tag

Use the query builder to search for the dataset: 

    filename LIKE ('%T101389/scenario1%') AND filetype LIKE ('jpg')


Once the dataset is found and displayed, select those that meet the criteria of a new dataset to be custom-tagged. In our case shown in the recording, we select all 11 images for tagging. We'll use tag "udc-dem2" and value "T101389-s1" to create the custom-tag and this way we can easily find this dataset in the future. 

An example disply of tagged and searched dataset is shown below: 

<img src=recording/T101389-Scenario1-result.png>

