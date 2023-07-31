![](/images/ahlsbanner.png)

# A-HLS Epic In Basket Accelerator Documentation 

## Overview

The Epic In Basket Accelerator enables a Salesforce user to compose an In Basket message to a recipient in the Epic EHR. The user is able to select the recipient, draft a message, and send to the selected provider via the customer's Incoming Provider Communication (MDM T02) HL7v2 interface.

The Accelerator requires an organization to have the following APIs and HL7v2 Interfaces enabled:
* Practitioner.Search (R4)
* CareTeam.Search (R4)
* Incoming Provider Communication (MDM T02) - requires custom configuration to connect to Salesforce

The following documentation provides step-by-step configuration instructions to implement the Accelerator in your Salesforce org. 

------

## Business Objective

The objective of the Epic In Basket Accelerator is to enable Salesforce customers to implement a Salesforce-to-Epic provider messaging solution more quickly, easily, and at a lower cost. Previously, healthcare organizations would need to custom build a Salesforce component to support this workflow. Now, with this Accelerator, healthcare organizations have a pre-built, configurable OmniScript that enables a user to send an In Basket message to a selected provider. 

## Business Value and Benefits

* Increase operational efficiencies
* Improve user experience and satisfaction
* Improve patient outcomes
* Lower IT costs

------

## Industry Focus and Workflow

### Primary Industry:

* Provider

### Primary User Persona:

* Call Center Agent
* Care Coordinator
* Patient Intake Coordinator

### User Workflow:

1. Select a recipient - either from the patient's Care Team or by searching for a provider in the Epic system.
2. Compose a message, select the urgency of the message, and attach any relevant files.
3. Select the associated encounter or indicate to create a new encounter upon sending the message.
4. Review and send the message.


------

## Package Includes:

### **OmniScript (1)**

* In Basket Message OmniScript

### **DataRaptor (17)**

* GetPatientEHRId
* GetPersonAccountCareTeam
* DRTransformPractitioner2
* EpicFHIRPatientConditionsXMLtoJSON
* EpicFHIREncountersXMLtoJSON
* DRTransformPractitioner
* EpicFHIRPatientAppointmentsXMLtoJSON
* EpicFHIRPatientDemographicsXMLToJSON
* DRTransformCareTeamJSONtoJSON
* EpicFHIRCarePlanLongXMLtoJSON
* EpicFHIRPatientFYIXMLtoJSON
* EpicFHIRPatientAllergiesXMLtoJSON
* DRTransformCareTeam
* EpicFHIROPCarePlanXMLtoJSON
* EpicFHIRMedicationXMLtoJSON
* EpicFHIRSocialHistoryXMLtoJSON
* DREpicPatient2JSONtoJSON

### **Integration Procedures (3)**

* GetInBasketRecipients
* AuthAndGetEpicData
* EpicFHIRGetData

------

## Configuration Requirements

### Pre-Install Configuration Steps:

EHR Pre-Installation Steps:
* Confirm that your endpoint is configured such that the following APIs are active - for full reference, please refer to the fhir.epic.com API documentation.
   * api/FHIR/R4/Encounter (Search)
   * api/FHIR/R4/Practitioner (Search and Read)
   * api/FHIR/R4/Appointment (Search)
* Confirm that your Epic system has the following HL7v2 Interfaces active:
   * MDM T02
     * Your organization's provider directory in Epic should be up to date with providers' communication preferences. If a provider's communication preference is In Basket, the provider will receive the message in In Basket. 
   * ADT01



Salesforce Pre-Installation Steps:
* Ensure your Salesforce Health Cloud org has OmniStudio installed - either the Vlocity HINS package or Core OmniStudio.
   * To verify installation, please navigate to Setup > Installed Packages > OmniStudio.
* Enable Identity Provider according to these steps: https://help.salesforce.com/s/articleView?id=sf.identity_provider_enable.htm&type=5
* Person Account records in Salesforce need to have patients' EPI IDs stored in them. By default, the Source System ID field is referenced, but the OmniScript can be configured to reference another field that your organization uses to store the patient identfier.
* Care Team members in Salesforce should have up to date Epic IDs in their Contact records. The OmniScript uses the Contact's Federation ID as the default identifier field, but this is configurable in the case that another field is used to store the Care Team member's Epid Identifier. 


#### Install the Accelerator

* Follow the download steps in the "Download Now" flow presented on the HLS Accelerators website for this Accelerator which installs an unmanaged package with the In Basket Custom Object into your org, and downloads the following GitHub repository on your machine: 
   * Unzip the resulting .zip file which is downloaded to your machine.
   * Open the “OmniStudio” folder
* Install the DataPack into your org.
   * In Salesforce, click on App Launcher → Search for "OmniStudio DataPacks" and click on it.
   * Click on "Installed" > Import > From File
   * When the window opens, select the .json file identified in the previous step. Click "Open" then click 'Next' 3 times.
   * When prompted, click "Activate Now".

### 3. Integration Configuration

There are 3 different ways that organizations may configure the integration for the accelerator. Click on the method below that supports your organization's integration architecture:

* [MuleSoft Direct](#mulesoft-direct)
* [MuleSoft Anypoint Platform](#mulesoft-anypoint-platform)
* [Epic Direct Connection](#Epic-Direct-Connection)

## MuleSoft Direct

Required SKUs:

-   Health Cloud
-   Anypoint Starter or Base

### 1. Complete MuleSoft Direct Configuration:

-   Complete the Setup steps for the  [**Integrations Setup for Industry Integration Solutions**](https://help.salesforce.com/s/articleView?language=en_US&id=sf.industry_integration_solutions.htm&type=5)
    -   In the  **Generic FHIR Client Setup**, enter the following information for the Epic EHR credentials
        -   **Authentication Protocol**: JSON Web Token
        -   **Base URL**: your Epic FHIR server’s base URL
        -   **Token URL**: your Epic FHIR server’s authentication URL
        -   **Client ID**: 43b0500b-ea80-41d4-be83-21230c837c15
        -   **Private Key**  - import the Private Key which is included in the Accelerator download .zip folder
        -   **Non-PRD Test Information**:
            -   **Endpoint**:  [https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token](https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token)
            -   **Client ID**: fe0e3375-fffa-469b-8338-d5fe08f7d3b1
            -   Refer to the  **Epic FHIR App**: Salesforce Health Cloud - Clinical Summary

<h3>2. Configure the Accelerator for MuleSoft Direct</h3>

Update the **EpicFHIRGetData** Integration Procedure to use the MuleSoft Direct connection instead of Epic FHIR APIs directly:

* App Launcher > Integration Procedures > EpicFHIRGetData
	* Create a **New Version** of the Integration Procedures
	* For each of the **HTTP Action** elements, make the following changes:
		* In the **Path** field, remove the “/FHIR/R4” portion of the path such that the Path = /api/AllergyIntolerance (for example)
		* Replace **Epic_Auth_JWT** with the name of the Named Credential resulting from the MuleSoft Direct setup above (e.g. **Health_generic_system_app**)
	* **Activate** your new Integration Procedure version

## MuleSoft Anypoint Platform

<h3>1. Configure MuleSoft</h3>

* Follow the [**Epic Administration System API Setup Guide**](https://anypoint.mulesoft.com/exchange/org.mule.examples/hc-accelerator-epic-us-core-administration-sys-api/minor/1.0/pages/edh-nhj/Setup%20Guide/)
* Create a new **Remote Site** (Setup > Remote Site Settings > New Remote Site) with the URL of the newly-created **MuleSoft app**.
* Create a **Named Credential** for your **MuleSoft app**:
	* Refer to **steps 5 - 8** in this article for the detailed setup: [https://sfdc247.com/2021/11/step-by-step-guide-on-integrating-mulesoft-and-salesforce.html](https://sfdc247.com/2021/11/step-by-step-guide-on-integrating-mulesoft-and-salesforce.html)
	* You do **NOT** need to complete the **External Services** configuration in the article above

<h3>2. Configure the Accelerator for MuleSoft</h3>

* App Launcher > Integration Procedures > EpicFHIRGetData
	* Create a **New Version** of the Integration Procedure
	* For the **HTTP Action** element, make the following changes:
		* In the **Path** field, set it to the URL of the newly-created **MuleSoft app** with the trailing API call - /api/AllergyIntolerance (for example)
		* In the Named Credential field, Replace **Epic_Auth_JWT** with the newly created **Named Credential** for your **MuleSoft app**


## Epic Direct Connection


Required SKUs:
* Health Cloud

<h3>1. EHR Pre-Configuration Steps:</h3>

* Ensure your EHR system's network is configured to accept traffic from your Health Cloud org.
* Install the Epic on FHIR App called “**Salesforce Health Cloud - Clinical Summary**” into your Epic organization. 
	* **Client ID:** 43b0500b-ea80-41d4-be83-21230c837c15

<h3>2. Salesforce Pre-Installation Steps:</h3>

<h4>Create a new Custom Metadata Type </h4>

* Setup > **Custom Metadata Types**
	* New Metadata Type - **must be named** as follows:  **ClientCredentialsJWT**
        * Add the following **Custom fields**. All fields should be **“Text”** fields with a length of **255** characters.
            * aud
            * callback uri
            * cert
            * iss
            * jti
            * sub
![](/images/fcimage2.png)
<h4>Install custom Apex Class</h4>

* Open the **“salesforce-sfdx”** folder in the .zip file that you downloaded for the Accelerator. 
* Use IDX or sfdx to install the files under the “salesforce-sfdx” folder. 
	* Click here to access the [**IDX workbench**](https://workbench.developerforce.com/login.php)
	* For more information regarding IDX, please review this [**Trailhead**](https://trailhead.salesforce.com/content/learn/modules/omnistudio-developer-tools)
* Alternatively, you may create a new Apex Class manually by performing the following:
	* Setup > Apex Classes > New
	* Copy the entire code of the Apex Class in the salesforce-sfdx folder and paste in the new Apex Class editor and Save.
* Import the keystore **FHIRDEMOKEYSTORE.jks** from .zip file. 
	* Setup > **Certificates and Key Management** > Import from **Keystore**
	* **Password**: salesforce1


<h4>Create a new Authentication Provider</h4>

* Setup > Auth Providers
    * Create a New Authentication Provider
        * Provider Type: **ClientCredentialJWT**
        * Name: Epic_JWT_Auth
        * iss: 43b0500b-ea80-41d4-be83-21230c837c15
        * sub: 43b0500b-ea80-41d4-be83-21230c837c15
        * aud: set this to the API endpoint for authentication - either the MuleSoft API or Epic FHIR API - e.g., https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token
        * jti: salesforce
        * cert: fhirdemo_cert
        * callback uri: https://YOURDOMAIN/services/authcallback/Epic_JWT_Auth
        * Execute Registration As: your system administrator User
![](/images/fcimage3.png)
<h4>Configure Remote Site Settings</h4>

* Setup > Remote Site Settings > New
    * Give the Remote Site a name and paste the domain of the API endpoint into the URL field.
    * Click Save.

<h4>Create a new Named Credential</h4>

* Setup > **Named Credential** > **New Legacy**
	* **Name**: **Must be** the following: **Epic Auth JWT**
	* **URL**: the URL of the endpoint you are going to connect to. For example, https://fhir.epic.com/interconnect-fhir-oauth/ 
	* **Identity Type**: Named Principal
	* **Authentication Protocol**: OAuth 2.0
	* **Authentication Provider**: the name of your Authentication Provider above
	* Check the **“Run Authentication Flow on Save”** box

![](/images/fcimage4.png)

![](/images/psimage5.png)

## Configure Accelerator
After completing the respective integration steps, complete the following:

#### 1. Configure FlexCards
* Open each FlexCard in the Accelerator and ensure it is Activated. If it is not, do the following:
	* Deactivate the FlexCard
	* Activate the FlexCard
	* For Publish Options, select “Record Page” and click Save.

#### 2. Configure Integration Procedure


#### 3. Update Timeline


#### 4. Update Person Account Lightning Page

------

## Assumptions

1. A customer has licenses for Health Cloud, and the HINS Managed Package with OmniStudio. These solutions have all been installed and are functional.
2. A customer is assuming Salesforce Lightning Experience — not Classic.
3. Data Model elements that are part of the HINS (Vlocity) Managed package and Health Cloud are all available.
4. The Accelerator uses the Lightning Design System standards and look. Customers may want to apply their own branding which can be achieved.

------

## Revision History

- **Revision Short Description (Month Day, Year)**

- - Add
  - Add



- 
