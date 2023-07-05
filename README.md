![](/images/ahlsbanner.png)

# A-HLS Epic In Basket Documentation 

## Overview

The Epic In Baskeet Accelerator enables a Salesforce user to compose an In Basket message to a recipient in the Epic EHR. The user is able to select the recipient, draft a message, and associate the message with a patient's encounter before sending.

The Accelerator requires an organization to have the following APIs and HL7v2 Interfaces enabled:
* Practitioner.Search (R4)
* Practitioner.Read (R4)
* Appointment.Search (R4)
* Encounter.Search (R4)
* Incoming External Encounter (ADT01) - can be connected to Salesforce via the MuleSoft Accelerator for Healthcare
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

### **OmniScript (#)**

- Add
- Add
- Add

### **DataRaptor (#)**

- Add
- Add
- Add

### **Custom Components (#)**

- Add
- Add
- Add

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
   * ADT01

Salesforce Pre-Installation Steps:
* Ensure your Salesforce Health Cloud org has OmniStudio installed - either the Vlocity HINS package or Core OmniStudio.
   * To verify installation, please navigate to Setup > Installed Packages > OmniStudio.
* Enable Identity Provider according to these steps: https://help.salesforce.com/s/articleView?id=sf.identity_provider_enable.htm&type=5

#### Install the Accelerator

* Follow the download steps in the "Download Now" flow presented on the HLS Accelerators website for this Accelerator which downloads the following GitHub repository on your machine: 
   * Unzip the resulting .zip file which is downloaded to your machine.
   * Open the “OmniStudio” folder
* Install the DataPack into your org.
   * In Salesforce, click on App Launcher → Search for "OmniStudio DataPacks" and click on it.
   * Click on "Installed" > Import > From File
   * When the window opens, select the .json file identified in the previous step. Click "Open" then click 'Next' 3 times.
   * When prompted, click "Activate Now".

### Post-Install Configuration Steps:

1. **[IF YOUR DATAPACK HAS OMNISCRIPTS - INCLUDE THE FOLLOWING STEPS]** 

2. Click on **App Launcher** → Search for “OmniScripts”

3. 1. Navigate to the recently installed OmniScript in the list view
   2. Click on the dropdown at the right of the OmniScript and select **Activate**.
   3. For more information regarding activating Omniscripts, please see this article: https://help.salesforce.com/s/articleView?id=sf.os_activating_omniscripts.htm&type=5

4. Add the installed OmniScript to the lightning page layout of your choosing. 

5. 1. Refer to this article for more information regarding adding OmniScripts to a Lightning Page: https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_a_lighting_page_20263.htm&type=5
   2. Refer to this article for more information regarding adding OmniScripts to an Experience Cloud Page: https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_an_experience_page_20341.htm&type=5

6. **[IF YOUR DATAPACK HAS FLEXCARDS - INCLUDE THE FOLLOWING STEPS]**

7. Click on **App Launcher** → Search for “FlexCards”

8. 1. Navigate to the recently installed FlexCard in the list view
   2. Open the FlexCard
   3. Click **Activate** and select the appropriate Publish Options
   4. For more information regarding activating FlexCards, please see this article: https://help.salesforce.com/s/articleView?id=sf.os_activateconfigureand_publish_flexcards_24744.htm&type=5

9. Add the installed FlexCard to the lightning page layout of your choosing.

10. 1. Refer to the following articles for more information regarding adding FlexCards to a Lightning or Experience page: 

    2. 1. https://docs.vlocity.com/en/Add-a-FlexCard-to-a-Lightning-Page.html
       2. https://docs.vlocity.com/en/Add-a-FlexCard-to-an-Experience-Page.html

11. Add

12. Add

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
