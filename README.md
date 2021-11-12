# Unified Data Catalog for Image Analytics


Spectrum Discover is an industry-leading unstructured data catalog platform that can be plugged into by various applications and tools for data discovery services. When integrated with Watson Knowledge Catalog from Cloud Pak for Data, we create a even more powerful solution for end-end cataloging of all types of data (structured, semi-structured and un-structured). This is a use case we term <B>Unified Data Catalog (UDC)</B>


This repository includes instruction, dataset, manifest and recording to recreate a demo based on Spectrum Discover to ingest, index, tag and extract insight from a military aircraft dataset. The output is a well-curated and easily-accessible dataset ready for HPC-based preprocessing and deep learning/AI training. 


### About Dataset
The dataset is a collection of 11 military aircraft images from [Kaggle](https://www.kaggle.com/a2015003713/militaryaircraftdetectiondataset/version/29). Here is an example aircraft image from the dataset:

<img src=T101389.dat/T101389.dat.annotated/00b2add164cb42440a52064e390ea3d2.jpg>

    - width: 1280
    - height: 850	
    - type: B1	
    - xmin: 322	
    - ymin: 112	
    - xmax: 893	
    - ymax: 618


### Demo Listings

 - [Scenario 1: Index Dataset & Prepare Catalog](scenario1/index.md)
 - [Scenario 2: Import Metadata to Enrich Catalog](scenario2/index.md)
 - [Scenario 3: Extend Catalog to Data & AI Workbench](scenario3/index.md)
 - [Scenario 4: ContentSearch of Photo Metadata to Enrich Catalog - WIP](scenario4/index.md)
