# Scenario 1: Initialize Catalog with Metadata




The step-by-step instruction and results are captured with screenshots and can be downloaded as [PDF](recording/T101389-Scenario1-v20210921.pdf) or [PPT](recording/T101389-Scenario1-v20210921.pptx)



## 1: Upload dataset and manifest

In your local or public cloud account, create a cloud object storage service. Start with a new vault called udc-vault and create a folder within called "T101389". All our demo data will be uploaded into this folder. 

Upload the sample data and manifest files into the cloud object storage "/udc-vault/T101389/dataset/" so they will appear as "/raw", "/annotated" and "...manifest.csv"

> Note: You can use your own dataset as long as the path to each file/object in the object storage bucket is updated


## 2: Connect Spectrum Discover to Data Source
Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover) on setting up and customizing Spectrum Discover and connecting/scanning a target data source



## 3: Set up ContentSearch Policy in Spectrum Discover

SSH log into the Spectrum Discover server. If you cannot gain SSH access to the server, you can still create the policy by using the Spectrum Discover RESTful API after obtaining the token. Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover)


Create the policy json file as following

        {
        "pol_id": "T101389_autotag_u2source_pol",
        "action_id": "AUTOTAG",
        "action_params": {
            "rule": {
            "name":"setFromExistingField",
            "action": "regex",
            "existingField": "filename",
            "regexPattern": "[^/]+",
            "matchNo": 3,
            "newField": "u2-source"
            }
        },
        "schedule": "NOW",
        "pol_filter": "filename LIKE ('%T101389%') AND filetype LIKE ('%jpg%')",
        "pol_state": "active",
        "explicit": "true",
        "collections": []
        }



Run two commands from CLI:

    gettoken

    curl -k -H "Authorization: Bearer ${TOKEN}" https://localhost/policyengine/v1/policies/T101389_autotag_u2source_pol -X POST -d @./T101389_autotag_u2source.json -H "Content-Type: application/json"


This will create a Spectrum Discover AutoTag policy named "T101389_autotag_u2source_pol". The policy will also be executed automatically. 


## 4: Find the dataset and create custom tag

Use the query builder to search for the dataset: 

    filename LIKE ('%T101389%') AND filetype LIKE ('%jpg%') 


Once the dataset is found and displayed, take note that the tag "u2-source" is now populated with values of "annotated" or "raw". The values were extracted by AutoTag policy from the 2nd level of filename or fileppath. 

<img src=recording/T101389-Scenario1-autotagdataset.png>


To preserve this new dataset in the platform, We'll use custom-tagging to populate "udc-dem2" with value "T101389-s1".

An example disply of tagged and searched dataset is shown below: 

<img src=recording/T101389-Scenario1-customtagnewdataset.png>


In the final step, we'll export the new T101389-s1 dataset into a manifest in [CSV file](recording/T101389-s1-manifest-v20210921.csv)

<img src=recording/T101389-Scenario1-exporttomanifest.png>
