# Scenario 1: Index Dataset & Enrich Catalog with Metadata Import


The step-by-step instruction and results are captured with screenshots and can be downloaded as [PDF](T101389-Scenario1-v20210920.pdf) or [PPT](T101389-Scenario1-v20210920.pptx)



### Step 1: Upload dataset and manifest

Upload the data in /dataset into a cloud object storage bucket (eg. "udc-vault") under the folder "T101389"

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


