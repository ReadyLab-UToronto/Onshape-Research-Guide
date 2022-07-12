# Onshape REST API
This guide provides a brief introduction to the Onshape REST API and its potential applications in research and teaching. For learn about the basics of the Onshape REST API, visit the official [Onshape Developer Documentation](https://onshape-public.github.io) for more information. 

## Table of contents 
- [1. Introduction](#1-introduction)
- [2. Applications](#2-applications)
    - [2.1. Design analysis](#21-design-analysis)
    - [2.2. Engineering analysis](#22-engineering-analysis)
    - [2.3. Computer science research](#23-computer-science-research)
    - [2.4. Teaching with increased interaction](#24-teaching-with-increased-interaction)
- [3. Sample projects](#3-sample-projects)

## 1. Introduction
Onshape's REST API provides efficient access for users to retrieve information from and make modifications to their Onshape documents. More importantly, it enables large-scale research analysis by establishing a computation workflow with the REST API. 

To learn more about getting started with Onshape's REST API, please see the official [Onshape Developer Documentation](https://onshape-public.github.io) for more information. In most cases, using the API keys should be sufficient for most research analysese. More examples and additional guides can be found in the GitHub organizations for [Onshape Public](https://github.com/onshape-public) and [PTC Education](https://github.com/PTC-Education). 

## 2. Applications 
In general, the REST API deals with the **product** of the Onshape documents. In this section, we try to summarize a few categories of research that may be enabled by the REST API. However, this is not an exhausted list, and there is certainly much more potential with the technology. 

### 2.1. Design analysis
There are many aspects of a CAD model that may worth studied. Some examples include: 
- The properties of the parts, such as mass (if a material/density is assigned to the part), volumn, surface area, etc. 
- The use of Part Studio and Assembly features in a workspace and the order of them in the final product. Learn more about the feature list in [Part Studios](https://cad.onshape.com/help/Content/feature_list.htm?tocpath=Part%20Studios%7C_____2) and in [Assembly](https://cad.onshape.com/help/Content/featurelistassembly.htm?tocpath=Assemblies%7C_____3). 
- Details about the sketches used in a Part Studio, including the entities and the constraints. 
- Details about other features used in a document, such as the geometries that a feature references, the values used for the features, etc. 
- The [version and/or microversions](https://cad.onshape.com/help/Content/versionmanager.htm?tocpath=Document%20Management%7CVersioning%20and%20Branching%7C_____0) of a document (a microversion is automatically created by the system for every change made in the model). Most of the analyses mentioned above can also be applied to study past versions. Such that, the design <i>process</i> of how a CAD <i>product</i> evolves can be tracked and studied. 

While all of the information mentioned above can be collected in the Onshape user interface, mannually performing such inspections for a large group of designs will be extremely time consuming, if not impossible. Retrieving model-specific information of Onshape documents can be achieved algorithmically with Onshape's REST API. Most analyses above can be done through the following high-level procedure: 
1. Set up an Onshape client using the generated API keys with the appropriate permissions (only read access is required if no modifications are to be made to the model). 
2. Look for the proper `GET` API endpoint for the information that is being retrieved. 
3. Browse through the returned message for the queried information. 
4. Repeat this process algorithmically for a large-scale study or experiment. 

For some more advanced analysis in Part Studios, you may want to first write one or more [custom feature(s)](https://cad.onshape.com/help/Content/featurestudios.htm) using the Onshape FeatureScript to calculate/collect the data that are of interest. Then, the values returned by the custom feature(s) can be retrieved through a simple `GET` API endpoint. 

### 2.2. Engineering analysis
With a built CAD model in Onshape, there are a variety of engineering analyses that can be performed with the REST API. In general, the main idea with this application is to calculate/retrieve a few values about the model with different [configurations](https://cad.onshape.com/help/Content/configurations.htm) and/or changes made to the model, which can also be performed with one or more `POST` API endpoint(s). Some examples include: 
- The properties (e.g., mass, volumn, area, length, angle) of an assembly, a model, a part, or even just a few features of the model. 
- Kinematic analysis of the motion path of a model, a part, or just a point of a mechanism. This is achieved through tracking the position of the structure as consecutive `POST` API requests are made to modify the model. 
- Animation of the motion of a mechanical part in the form of a GIF file. This is achieved through taking a "screenshot" of the CAD model after every small modification made to the model through consecutive `POST` API requests. 

Again, the aid of one or more custom feature(s) using the Onshape FeatureScript, as described in the previous section, may be beneficial to the calculation and/or data collection of the analysis. In most cases, the general procedure should be the following: 
1. Set up an Onshape client using the generated API keys with the appropriate permissions; you may need write-access enabled if making changes to the model. 
2. Locate the model, part, feature, or any property that you are of interest with an appropriate `GET` API endpoint. Then, try to retrieve the information that you are of interest for testing. 
3. Make changes to the model using one or more `POST` API endpoint or try to retrieve information using the same `GET` endpoint(s) that you used above but with different configurations queried. Test if the changes in values that you are getting with the method tested in the previous step make sense. 
4. With the above tested, you can now repeat this process algorithmically to conduct your analysis. 

### 2.3. Computer science research 
As the REST API enables the modelling and study of a large amount of CAD models in Onshape, there is a great potential for machine learning research that involves big data. Some sample directions include: 
- Study of the geometry (e.g., sketches) and the relationship between parts and features (e.g., mates in assemblies)
- Automatic categorization of shape and structure of CAD models 
- Automatic generation of sketches, parts, and models 

More research publication related to this category can be found in [this page](https://ptc-education.github.io/docs/thought-leadership/publications). 

### 2.4. Teaching with increased interaction
While the ability to visualize abstract math and science concepts is often one of the main struggling points for students. Building and interacting with computer-simulated models in Onshape may provide a great tool for educators to improve students' learning experience. For instance, here are a few potential pathways: 
- While making `POST` requests to an Onshape model, the CAD model is actually being modified in real time. In other words, making consecutive `POST` API requests to a CAD model can essentially simulate the motion of the mechanism/structure while the code runs in the background. 
- Through performing CAD-based virtual experiments, students can interact with, or even design on their own, models that visualize and simulate the abstract concepts. Then, applying the learned concepts to the models may allow the students to gain a better sense of how the concept may affect the physical world. 

In some scenarios, teachers may even benefit from building a simple local web application that can be integrated to the native Onshape user interface through OAuth. Such that, students can open the analysis tool as a side panel or a different tab in their Onshape documents. Visit the official [Onshape Developer Documentation](https://onshape-public.github.io) for more information on OAuth integration. 

## 3. Sample projects 
For some sample projects that utilize the Onshape REST API for research and/or teaching, please visit [this page](/api/sample/README.md) for more information. 