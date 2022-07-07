# Onshape REST API
This guide provides a brief introduction on setting up and using the Onshape REST API in Python.  Specifically, resources detailed in this guide are written in Jupyter notebooks through Google Colab. It is recommended to use the same programming environment for practice and learning. For other programming languages, please see [this GitHub repository](https://github.com/onshape-public/onshape-clients).

## Table of contents 
 * [1. General resources](#1-general-resources)
 * [2. Generating your Onshape API keys](#2-generating-your-onshape-api-keys)
 * [3. Getting started with the Onshape REST API](#3-getting-started-with-the-onshape-rest-api)
   * [3.1. Understanding the URL](#31-understanding-the-url)
   * [3.2. REST API](#32-rest-api)
   * [3.3. Initiating an Onshape API call](#32-rest-api)
     * [3.3.1. Configurations](#331-configurations)
     * [3.3.2. Making POST API calls](#332-making-post-api-calls)
* [4. Other methods](#4-other-methods)
    * [4.1. Using the `onshape_client` library](#41-using-the-onshape_client-library)
    * [4.2. Using Jupyter notebook snippets](#42-using-jupyter-notebook-snippets)
* [5. Sample projects](#5-sample-projects)

## 1. General resources 
Before getting started with this guide, you should have a working background knowledge on the Python programming language and preferrably some experience with Jupyter notebooks in Google Colab. 

Below are some useful resources and links that will be referred to in this guide, please feel free to save these resources for future referrence: 
- All Onshape REST API endpoints are documented on [Glassworks](https://cad.onshape.com/glassworks/explorer/#/). 
- The source code of the `onshape_client` library can be found in [this GitHub repository](https://github.com/onshape-public/onshape-clients) and the code for all API calls can be found in [this directory](https://github.com/onshape-public/onshape-clients/tree/master/python/onshape_client/oas/api). 
- The [PTC-API-Playground](https://github.com/PTC-Education/PTC-API-Playground) GitHub repository provides various sample projects through making Onshape API calls. 
- A libary of ready-to-use Python snippets of Onshape API calls can be found [here](https://github.com/PTC-Education/PTC-API-Playground/blob/main/Onshape_API_Snippets.ipynb), which can be easily imported to your own Jupyter notebook on Google Colab. A quick guide to import can be found in the `README.md` file of its home repository. 
- A few more introduction videos on Digital Twins with the Onshape REST API can be found [here](https://ptc-education.github.io/docs/solutions/onshapedx). 

## 2. Generating your Onshape API keys
To gain access to your Onshape document through making API calls, we need to first generate and obtain a set of API keys for your account: 

1. Go to https://dev-portal.onshape.com and log in with your Onshape account. 
2. Under "API keys" of the left panel, click "Create new API key" in the top right corner of the page. 
3. Choose the company (if any) and permissions that you would like this key to have access to (recommend at least permissions to read and write, then be careful with other permissions before you gain sufficient experience). 
4. Once the API key is created, a pop-up box should show an access key and a secret key. 
5. You are free to record and save your API keys in any ways. However, we recommend opening up a text file locally in your computer and saving these two keys in the following format, replacing the `{...}` with your actual keys. Then, save the file as a `.py` file.  

        access = '{access_key}'
        secret = '{secret_key}'

**CAUTION:** 
- You should NEVER share your API keys with other people, nor upload this publicly through the Internet. Having access to your API keys is equivalent to gaining access to your Onshape account through password login with the permissions you specified in step 3 above. 
- You should also periodically delete your API keys and create a new set of keys on the [developer portal](https://dev-portal.onshape.com), so that the leaked keys can be deactivated even you accidentally uploaded your keys to any public platform. 

## 3. Getting started with the Onshape REST API 
Note that this section only describes the most basic structure of a typical Onshape API call, more advanced applications can be found in the [Onshape API Snippets](https://github.com/PTC-Education/PTC-API-Playground/blob/main/Onshape_API_Snippets.ipynb) and other sample projects in the [PTC-API-Playground](https://github.com/PTC-Education/PTC-API-Playground). 

### 3.1. Understanding the URL 
For every Onshape document, its URL can be broken down in a few structured segments.For example, here is a URL of an Onshape document: https://cad.onshape.com/documents/263517311c2ad139d4eb57ca/w/b45057ae06777e0c28bca6c5/e/d316bcbc694c9dbb6555f340
- `https://cad.onshape.com/` is called the "base URL". Default base URL will be `cad.onshape.com` for standard Onshape accounts. For enterprise accounts, it will be a different URL for every different enterprise (e.g., `ptc.onshape.com`). 
- `documents/` or `d/` provides the unique ID of the "document" that is loaded in the browser, or `DID`. A document contains all the content related to the design project, including Part Studios, Assemblies, Drawings, etc. 
- `w/` provides the unique ID of the "workspace" that is currently being worked on, or `WID`. By default, you always start with the "Main" workspace when you first create an Onshape document. Meanwhile, additional workspace can be created in the document through branching. However, this part of the URL can also be replaced with: 
  - `v/` followed by a `VID`: a "version" or "release" of the Onshape document that is created at a specific point of time of the design process. 
  - `m/` followed by an `MID`: a "micro-version" of the document at a specific point of time of the design process. Every change made to an Onshape document automatically generates a micro-version for the document. 
  - Note that versions are specifically created and defined by the users, but micro-versions are automatically logged by the system. 
- `e/` provides the unique ID of the "element" that is currently opened in the workspace, or `EID`. An element is essentially a tab in an Onshape document, which can be a Part Studio, an Assembly, a Drawing, etc.

There is also an efficient way of separating the URL into its components: 

    from onshape_client.onshape_url import OnshapeElement 

    url = 'https://cad.onshape.com/documents/263517311c2ad139d4eb57ca/w/b45057ae06777e0c28bca6c5/e/d316bcbc694c9dbb6555f340'
    element = OnshapeElement(url) 
    
    base = element.base_url
    
    # Assume we would like to replace "did", "wid", and "eid" of the "fixed_url" with IDs from the main "url" above 
    fixed_url = '/api/partstudios/d/did/w/wid/e/eid/massproperties'
    fixed_url = fixed_url.replace('did', element.did)
    fixed_url = fixed_url.replace('wid', element.wvmid)
    fixed_url = fixed_url.replace('eid', element.eid)

**Note:** you may need to install the `onshape_client` library through the following command line in your terminal if you have never done so before: 

    $ pip install onshape-client 

### 3.2. REST API 
On [Glassworks Explorer](https://cad.onshape.com/glassworks/explorer/#/), every Onshape API endpoint is labelled with its respective API call type. For every REST API call in Onshape, it should fall under one of the three types: 
- `GET`: retrieve information from the server (e.g., retrieve the parameters of a specific feature in the Onshape document). 
- `POST`: update the server with new information (e.g., change the  parameter values of a specific feature in the Onshape document). 
- `DELETE`: delete information from the server (e.g., delete a specific part from a Part Studio in the Onshape document). 

**Note:** you may not be able to use any API call labelled with `DELETE` if you did not allow deleting permission in step 3 of [section 2](#2-generating-your-onshape-api-keys) above. 

### 3.3. Initiating an Onshape API call
Before making any API call to Onshape, you need to first set up and configure an "Onshape client" with your API keys. In other words, you need to log in to your Onshape account before you can make changes to your documents, but through an API approach. In general, the set-up process is in the following format: 

    from onshape_client.client import Client 
    client = Client(configuration={"base_url": base, 
                                   "access_key": access, 
                                   "secret_key": secret}) 

Alternatively, Section 00.1 of the [Onshape API Snippets](https://github.com/PTC-Education/PTC-API-Playground/blob/main/Onshape_API_Snippets.ipynb) already provides two methods of configuring your API client: manually typing in your API keys, or uploading the `.py` file that you may have created and saved in Step 5 of [section 2](#2-generating-your-onshape-api-keys) above. Both of these methods provide efficient and safe configuration approach; your API keys will be deleted from the script right after you successfully configure your account. 

After you successfully configure your API keys, you can start making REST API calls with the permissions you provided as you created the keys. A typical Onshape API call has the following format: 

    response = client.api_client.request(method, url, query_params, headers, body)

- `method` is one of the three API call types listed in [section 3.2](#32-rest-api) above. This should be entered as a string with all letters capitalized (e.g., `"GET"`). The API call type for every endpoint is also labelled on [Glassworks](https://cad.onshape.com/glassworks/explorer/#/).
- `url` is the URL address to this API call, found as the title for each REST API call on [Glassworks](https://cad.onshape.com/glassworks/explorer/#/). **Note** that this may NOT be the same URL that you use to access the Onshape document from the browser. For example, to retrieve a list of all the part features in a Part Studio: `url='{base_url}/api/partstudios/d/{did}/{wvm}/{wvmid}/e/{eid}/features'`. Notice the added `api/partstudios/` at the beginning and `/features` at the end of the URL. 
- `query_params` is a dictionary of parameters that you would like your CAD model to possess. For every endpoint on [Glassworks](https://cad.onshape.com/glassworks/explorer/#/), the "parameters" labelled with `(query)` along with their data type are what can be optionally included in the `query_params` dictionary. Common components of `query_params` include: 
  - The "configuration" of the Onshape document, see further details in [section 3.3.1](#331-configurations). 
  - The `partId` of the part that you would like to look specifically into in a Part Studio. You can get all the parts in a Part Studio with this [Glassworks endpoint](https://cad.onshape.com/glassworks/explorer/#/Part/getPartsWMV), or snippet 02.1 in the [Onshape API Snippets](https://github.com/PTC-Education/PTC-API-Playground/blob/main/Onshape_API_Snippets.ipynb). 
  - Note that the variety of `query_params` are not limited to the two presented here. More advanced applications can be found in the [PTC-API-Playground](https://github.com/PTC-Education/PTC-API-Playground). 
- `headers` is a dictionary that generally consists of only two components: `"Accept"` and `"Content_Type"`. On [Glassworks](https://cad.onshape.com/glassworks/explorer/#/), the `"Accept` header can be found in "Media type" under "Responses" of the API call that you are making. For most cases, we would like `"Content_Type"` to be `"application/json`, so that we can deal with API response data in JSON format. As an example, `headers = {'Accept': 'application/vnd.onshape.v2+json; charset=UTF-8;qs=0.2', 'Content-Type': 'application/json'}`. 
    - Note that if you are making multiple related API calls (e.g., the workflow of making a `POST` call as shown in [section 3.3.2](#332-making-post-api-calls)), make sure the `"Accept"` components of all `headers` across API calls have the same version. Otherwise, the server cannot deal with data of different format in different versions. 
- `body` is a dictionary of additional request payload that the API call carries with. It is generally only used when making a `POST` call, and it should be written in the same JSON format, the format that is defined for the `GET` call; see further details in [section 3.3.2](#332-making-post-calls). 

Putting everything together, here is an example of a complete API call to get the mass properties of a part from the Part Studio: 

    url = 'https://cad.onshape.com/documents/263517311c2ad139d4eb57ca/w/b45057ae06777e0c28bca6c5/e/d316bcbc694c9dbb6555f340'
    
    element = OnshapeElement(url) 
    base = element.base_url
    fixed_url = '/api/partstudios/d/did/w/wid/e/eid/massproperties'
    fixed_url = fixed_url.replace('did', element.did)
    fixed_url = fixed_url.replace('wid', element.wvmid)
    fixed_url = fixed_url.replace('eid', element.eid)
    
    method = 'GET' 
    params = {'partID': 'JZH', 
              'configuration': 'configVariable%3D0.01%2Bmeter'}
    headers = {'Accept': 'application/vnd.onshape.v2+json;charset=UTF-8;qs=0.2',
               'Content-Type': 'application/json'}
    payload = {}
    
    response = client.api_client.request(method, url=base + fixed_url, query_params=params, headers=headers, body=payload)

With the procedure detailed above, the `response` from such API calls will be stored in JSON format, and a good way of making the output data more readable and accessbile will be the following: 

    import json 
    parsed = json.loads(response.data) 
    print(json.dumps(parsed, indent=4, sort_keys=True)) 

Then, `parsed` can be accessed in the same way as a Python dictionary, and the `print` statement will print out `parsed` in a more readable structure with indentation. 

#### 3.3.1. Configurations
When modelling in an Onshape document, building ["configurations"](https://cad.onshape.com/help/Content/configurations.htm) of the design provides an efficient method to change the design to different states, both within the Onshape user interface and through making REST API calls. With configurations created for an Onshape document, one can easily change the model in Onshape through multiple API calls with different `configuration` input for `query_params` in the request. In general, there are two types of configuration that one can create in Onshape: 
1. Configuration input: various user-defined states of the model that the model can be varied to, where each state may change multiple parameters and/or dimensions of the design. 
2. Configuration variable: similar to an ordinary [variable](https://cad.onshape.com/help/Content/variable.htm) in Onshape, but with more efficient access and control through API calls. 

When accessing the Onshape document with certain configurations through the URL, simply adds `?configuration={config}` to the end of the URL of the document. To build `{config}` with the configurations of the document, the following rules applied: 
- `%3D` after a configuration input/variable means `=`. 
- `%2B` after the numerical value of a configuration variable means the unit of the variable to be followed. 
- `%3B` is used as `and` when more than one configuration input and/or variable are used.

For example, if I have an Onshape document with configurations `size` and `base_dimension`, where: 
- `size` is a configuration input with states: `"Default"`, `"Large"`, and `"Small"`; 
- `base_dimension` is a configuration variable that defines the base length of the model.  

Then, a possible configuration of this model can be specified through an API call in the following format: 

    # Setting "size" to be "Large" and the "base_dimension" to be 0.5 meter 
    config = "size%3DLarge%3Bbase_dimension%3D0.5%2Bmeter"
    params["configuration"] = config

**Note:** however, if you do not specify the `"configuration"` in your `query_params` for the API calls, Onshape will always use the default value of all the configurations, despite any changes in the actual Onshape document. Also, changes to the configuration through API calls are not saved in your actual Onshape document. Hence, you should plan the usage of your CAD model beforehand when deciding whether you should build the model with "configurations" or "variables" (which is essentially a part "feature"). 

#### 3.3.2. Making `POST` API calls 
When making a `POST` API call to update information of the Onshape model, most procedures and API call components are similar to a `GET` call. The major difference comes from specifying the payload `body` of the request. 

As an example, let's take a look at snippet 01.4 in the [Onshape API Snippets](https://github.com/PTC-Education/PTC-API-Playground/blob/main/Onshape_API_Snippets.ipynb). For this example, we would like to update the geometry of a part feature in the Part Studio. In general, the overall pathway of achieving this will be: 
1. Get the information about this feature through [this endpoint](https://cad.onshape.com/glassworks/explorer/#/PartStudio/getPartStudioFeatures), which is a `GET` API call. Making this call can follow the procedure described above in [section 3.3](#33-initiating-an-onshape-api-call).  
2. Explore the returned JSON `response` from the `GET` request and locate the specific feature that you would like to modify. Update the information of that feature in-place in the request `response` in its original JSON format. 
3. Send the updated information back to your Onshape document and update the feature through [this endpoint](https://cad.onshape.com/glassworks/explorer/#/PartStudio/updatePartStudioFeature), which is a `POST` API call. 

Note that when you open a `POST` endpoint on [Glassworks](https://cad.onshape.com/glassworks/explorer/#/), there should be a section named "Request body" under the "Parameters" that are required for the call. You should define the `body` for your API call request to include the required information as specified. 

For example, the example above tries to make changes to a feature in the document, and the [endpoint](https://cad.onshape.com/glassworks/explorer/#/PartStudio/updatePartStudioFeature) of the `POST` call in step 3 requires the information of the `"feature"` that is to be changed. Hence, the required information for that specific feature is what you will need to locate, retrieve, and modify in step 2. 

## 4. Other methods
Instead of building API calls from scratch, there are two other methods that may provide more efficient access to API calls. **Note:** these are methods are still under testing, some API calls may not function properly. Hence, it is still important to learn how to build an API call from scratch with the steps outlined above. 

### 4.1. Using the `onshape_client` library 
Installing and importing the `onshape_client` library also allow you to make these API calls directly. However, the documentation that automatically shows up when filling the arguments of these calls is not yet fully comprehensive, which may require further referrence to [Glassworks](https://cad.onshape.com/glassworks/explorer). The source code for this library can be found in [this GitHub repository](https://github.com/onshape-public/onshape-clients), and the code for all API calls can be found in [this directory](https://github.com/onshape-public/onshape-clients/tree/master/python/onshape_client/oas/api). 

With a `client` set up as shown in [section 3.3](#33-initiating-an-onshape-api-call), here is an example of getting the mass properties of a part studio in Onshape through making API calls (corresponds to [this Glassworks endpoint](https://cad.onshape.com/glassworks/explorer/#/PartStudio/getPartStudioMassProperties)): 

    response = client.part_studios_api.get_part_studio_mass_properties(did="263517311c2ad139d4eb57ca", 
                                                                       wvm='w', 
                                                                       wvmid='b45057ae06777e0c28bca6c5', 
                                                                       eid='d316bcbc694c9dbb6555f340')
    print(response)

Note a few things from this example: 
- After you type out `client.`, you should be able to see a list of different APIs, which corresponds to the API categories on [Glassworks](https://cad.onshape.com/glassworks/explorer). 
- After specifying the API category (e.g., `client.part_studios_api.` in this case), you should be able to see a list of different API calls, which are all the API calls that can be made through the `onshape_client` library. Note that the function names may be slightly different from the titles shown on Glassworks.
- The arguments of the function should align with the field names on [Glassworks](https://cad.onshape.com/glassworks/explorer). In the example above, the arguments included are all the required parameters for the call, and you can definitely also enter other optional parameters as well. 
- The returned output of the function (i.e., `response` in this case) is already well formatted; a simple `print` statement is capable of printing it in a human-readable format. 
- If you run into any errors when executing the API call, you may want to check out the source code of the function [here](https://github.com/onshape-public/onshape-clients/tree/master/python/onshape_client/oas/api). 

### 4.2. Using Jupyter notebook snippets
All API calls have been automatically generated with the method stored in [this repository](https://github.com/PTC-Education/Python-OpenAPI). The API calls are structured in the form of code snippets in Jupyter notebook, which provides greater flexibility with documentation. However, the generator may not be routinely updated with the most current version of the API specification. Also, it should be noted that this repository is only tested with some of the more popular API endpoints, and it may run into errors for edge cases. 

## 5. Sample projects 
For some sample projects that utilize the Onshape REST API for research and/or analysis, please visit [this page](https://github.com/PTC-Education/Onshape-Research-Guide/tree/main/api/sample) for more information. 