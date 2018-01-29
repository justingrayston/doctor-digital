# doctor-digital 

## Overview
We have an accelerator called [Apigee HealthAPIx software solution](https://github.com/apigee/flame) which assists healthcare providers in accelerating the development and delivery of digital services based on FHIR (Fast Healthcare Interoperability Resources) APIs.

Apigee HealthAPIx is built on the Apigee Edge API management platform, and features FHIR APIs and a healthcare developer portal to help hospitals meet the demand for data interoperability, deliver patient-centric healthcare, and move faster to the digital world.

http://fhirtest.uhn.ca/ is a publicly accessible FHIR server
We are using the DSTU2 FHIR server for this demo - http://fhirtest.uhn.ca/baseDstu2.
It is accessible using the HAPI client implementation.

We have interfaced some of these APIs(appointment booking) using dialogflow. 

## Components
#### Dialog flow agent - 
These are modules which convert user requests to actionable data
#### Apigee Edge - 
+ apiai-connector - webhook for the dialog flow agent. This is where all the actionable data is used to form an RESTful request for the provider.
+ fhir-api - This is a part of the flame solution which is built as an accelerator. This is the northbound part of the proxy to which the calls are made by the apiai-connector.
+ fhir-connector - This is also part of the flame solution. This is the southbound part of the proxy which connects to the publically accessible FHIR server.
+ auth-b2b - This is also a part of the flame solution. This is a Business2business authorization module for the FHIR APIs exposed as a part of the flame solution.

## Installation Instructions


#### Install edge componenets
##### Pre-requisites
###### Apigee edge account (https://enterprise.apigee.com)
###### node.js 
###### npm

##### Clone the repository
```
git clone https://github.com/rohan-m/doctor-digital.git
```
##### Enter edge directory
```
cd doctor-digital/edge/
```
##### Install gulp 
```
npm install --global gulp-cli
```
If you run into permission issues, try `sudo npm install --global gulp-cli`

##### Pull node modules
```
npm install
```
##### Run the deploy command
```
gulp deploy --target_server_host fhirtest.uhn.ca --target_server_port 80 --target_server_basepath /baseDstu2
```
This will interactively prompt you for following details, and will then create / deploy all relevants bundles and artifacts and will provision the **Doctor Digital Sandbox** on your own Org.

+ Edge Org name
+ Edge Username
+ Edge Password
+ Edge Env for deployment

#### Install dialogflow components
##### Pre-requisites
###### Dialogflow account(https://dialogflow.com)
*Click on `Go to console`
*Sign-in with your google account
##### Create a new agent
##### Import zip
+ Go to settings of the agent
+ Click on `Export and Import` tab
+ Select the `Import from zip`
+ Include the zip provided in this repository
##### Modify fulfilment
+ Click on `Fulfilment`
+ Change the Webhook url to include the Edge organisation, environment and the API key of the api ai connector app
+ You can find the API key in the developer app `apiAiApp` which will be created as a part of installing edge components
##### Add Integrations
+ Click on `Integrations`
+ Click on Google Assistant `Integration Settings`
+ You will find `TEST` and `MANAGE ASSISTANT APP`
+ First click `MANAGE ASSISTANT APP` to configure the assistant
+ [Optional] You will visit Actions on Google information page where you find App Information Edit
+ [Optional] Add Assistant name and some invocation(like `Connect to Digital Health` or `Connect to Doctor Digital`) that you will use to invoke the assistant
+ [Optional] You can also add other info and images
+ [Optional] Save the changes
+ Try `Test Draft` or Back to Dialogflow page click `TEST`
+ You will visit Actions on Google simulator page
+ Please make sure that testing on device is enabled, which is the found at the first icon in the top right corner of the page

## Usage Steps
Try the following on the actions simulator or your Google assistant app on your phone

+ Start with saying `Connect to Digital Health` or `Talk to Digital health` or any other invocations that may be set
+ It will ask for your phone number - 669 5858 is the default number to use, you can add a patient with a new number and a name and you can specify the same(additional details for creating a patient will follow soon, create a git request in the meantime, you can also create request to add additional location/places/hospitals)
+ Once you have provide your phone details there are couple of things you can do Like
  + Get nearby clinic
  + Show my appointments - then delete my appointments 
  + Book a new appointment -works only when you have got the clinic info
  + Get wait time at the clinic -works only when you have got the clinic info
+ The assistant will end the conversation after a successful booking or deletion of an appointment, you may have to reconnect to list the appointment
+ There is a slight delay in listing the appointment as the data in the FHIR server is reflect after a few seconds.

