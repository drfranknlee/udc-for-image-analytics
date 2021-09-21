# Scenario 2: Catalog Multi-dimensional Dataset



The step-by-step instruction and results are captured with screenshots and can be downloaded as [PDF](recording/T101389-Scenario2-v20210921.pdf) or [PPT](recording/T101389-Scenario2-v20210921.pptx)



### Step 1: Upload New Dataset Manifest

Upload the new dataset "T101389-s2" manifest from local /dataset directory to "/udc-vault/T101389/dataset/"

> Note: You can use your own dataset as long as the path to each file/object in the object storage bucket is updated


### Step 2: Set up Tag-import Policy in Spectrum Discover

SSH log into the Spectrum Discover server. If you cannot gain SSH access to the server, you can still create the policy by using the Spectrum Discover RESTful API after obtaining the token. Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover)


Create the policy json file as following

        {
                "pol_id": "T101389_ImportTags_pol",
                "action_id": "IMPORT_TAGS",
                "action_params": {
                        "agent":"ImportTags",
                        "source_connection":"UDSt-CoS",
                        "tag_file_path":"udc-vault/T101389/dataset/T101389_s2_manifest.csv",
                        "tag_file_type":"csv"
                },
                "schedule": "NOW",
                "pol_state": "active",
                "pol_filter": "datasource IN ('udc-vault')"
        }


Run two commands from CLI:

        gettoken

        curl -k -H "Authorization: Bearer ${TOKEN}" https://localhost/policyengine/v1/policies/T101389_ImportTags_pol -X POST -d @./T101389_ImportTags_pol.json -H "Content-Type: application/json"


This will create a Spectrum Discover metadata policy "IMPORT_TAGS" named "T101389_aircraft_import_scenario2_pol". The policy will also be executed automatically. 


### Step 3: Enrich the Catalog with Imported Metadata

Use the query builder to search for the dataset: 

    filename LIKE ('%T101389%') AND filetype LIKE ('jpg')


Once the dataset is found and displayed, select those that meet the criteria of a new dataset to be custom-tagged. In our case shown in the recording, we select or find all 22 images for tagging. We'll use tag "udc-dem3" and value "T101389-s2" to create the the new dataset. An example disply of tagged dataset is shown below. Note that we can now show additional matadata such as the dimensions of the bounding box for the aircraft as well as image ID that we assigned to them. 

<img src=recording/T101389-Scenario2-importtagresult.png>


In Spectrum Discover, we can now also pull up a pair of images (raw and annotated) based on their image ID tag "ud-id"

    u2-id LIKE ('%T101389-10005%')


See the resulting display of the pair of data in the catalog, and the images superimposed next to each other with the bounding box for the annotated one. 

<img src=recording/T101389-Scenario2-finding-pair-1.png>

<img src=recording/T101389-Scenario2-finding-pair-2.png>
