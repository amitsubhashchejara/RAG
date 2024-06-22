# Retrieval Augmented Generation
This repository implements RAG using various techniques and packages in Python.

## 1. RAG using Unstructured
This file 'simple-rag-from-scratch.ipynb' contains a Python 3 environment that uses the Unstructured client to extract elements from a PDF file. 
The code is designed to handle various dependencies and set up the user secrets and unstructured client.

### Install Dependencies
  * Run the following commands to install the necessary packages:
  * ```
       !pip install -q unstructured
       !pip install -q sentence-transformers
    ```
    
### Import Dependencies
  * Import the required libraries:
  * ```
       import warnings
       warnings.filterwarnings('ignore')
       import os
       import json
       from unstructured_client import UnstructuredClient
       from unstructured_client.models import shared
       from unstructured_client.models.errors import SDKError
       from unstructured.staging.base import dict_to_elements, elements_to_json
       from IPython.display import JSON
    ```

### Setting Up the User Secrets and Unstructured Client
You can get Unstructured api key and url here:
(https://unstructured.io/api-key-hosted)

  * Use the UserSecretsClient to retrieve the API key and server URL:
  * ```
       api_key="your api key"
       url="unstructured_url"
       s = UnstructuredClient(
       api_key_auth=api_key,
       server_url=unstructured_url,
       )
    ```
    
### Getting Elements from a Single PDF
  * Define a function to extract elements from a PDF file:
  * ```
    def get_elements_from_pdf(filename):
     with open(filename, "rb") as f:
         files = shared.Files(
             content=f.read(),
             file_name=filename,
         )
     req = shared.PartitionParameters(
         files=files,
         strategy='hi_res',
         pdf_infer_table_structure=True,
         languages=["eng"],
     )
     try:
         resp = s.general.partition(req)
     except SDKError as e:
         print(e)
     return resp
    ```

### Example Usage
  *   ```
      filename = "file path"
      response = get_elements_from_pdf(filename)
      print(json.dumps(response.elements[:3], indent=2))
      ```
