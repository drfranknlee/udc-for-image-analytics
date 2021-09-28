# Scenario 1: Initialize Catalog with Metadata




The step-by-step instruction and results are captured with screenshots and can be downloaded as [PDF](recording/T101389-s1-v20210921.pdf) or [PPT](recording/T101389-s1-v20210921.pptx)



## 1. Set up Spectrum Discover to Manage Data Sources
Please consult the [Spectrum Discover Documentation](https://www.ibm.com/docs/en/spectrum-discover) on setting up and customizing Spectrum Discover and connecting/scanning a target data source. When all set up, you can manage the access to external data sources (S3, File Systems), initiate and review the scanning of target resources. 

<img src=rm/T101389-s1-sd-data-source-management.png>


## 2. Upload Dataset to Object Storage

In your local or public cloud account, create a cloud object storage service. Start with a new vault called udc-vault and create a folder within called "T101389". All our demo data will be uploaded into this folder. This object storage should be set up as a managed data source by Spectrum Discover. 

<img src=rm/T101389-s1-upload-data.png>


## 3. Set up ContentSearch Policy in Spectrum Discover

In this step, we'll set up a Spectrum Discover ContentSearch policy to extract the image source (eg. raw vs annotated) from the path of the images and polulate the value into metadata tag named "u2-source".


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



Run first command to refresh the token.

   
    gettoken


Run the second command to create the policy using RESTful API service.

    
    curl -k -H "Authorization: Bearer ${TOKEN}" https://localhost/policyengine/v1/policies/T101389_autotag_u2source_pol -X POST -d @./T101389_autotag_u2source.json -H "Content-Type: application/json"
    


This will create a Spectrum Discover AutoTag policy named "T101389_autotag_u2source_pol". The policy will also be executed automatically. 


## 4. Find the dataset and create custom tag

Use the query builder to search for the dataset: 

    filename LIKE ('%T101389%') AND filetype LIKE ('%jpg%') 


Once the dataset is found and displayed, take note that the tag "u2-source" is now populated with values of "annotated" or "raw". The values were extracted by AutoTag policy from the 2nd level of filename or fileppath. 

<img src=rm/T101389-s1-autotagdataset.png>


To preserve this new dataset in the platform, We'll use custom-tagging to populate "udc-dem2" with value "T101389-s1".

An example disply of tagged and searched dataset is shown below: 

<img src=rm/T101389-s1-customtagnewdataset.png>


In the final step, we'll export the new T101389-s1 dataset into a manifest in [CSV file](recording/T101389-s1-manifest-v20210921.csv)

<img src=rm/T101389-s1-exporttomanifest.png>


## 5. View Images Directly from Object Storage

Combine the values of metadata tag "path" (ie bucket/vault) and "name" (filepath to the objects including the directories) for the full path to the images stored in the object storage bucket. The url for the object storage server or host can be located in the Spectrum Discover "Data source management" panel. In our case:

- path: udc-vault
- name: T101389/dataset/annotated/000ec980b5b17156a55093b4bd6004ab.jpg
- cluster: 9.11.221.105

<img src=rm/T101389-s1-find-fullpath-to-object.png>


You can concatenate the cluster, path and name value to the url for the object as: http://9.11.221.105/udc-vault/T101389/dataset/annotated/000ec980b5b17156a55093b4bd6004ab.jpg


You can open and visualize the image direclty from a web browser. 

<img src=rm/T101389-s1-direct-access-to-file.png>

