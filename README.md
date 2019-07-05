# Azure Virtual Assistant Workshop
**Welcome to the one and only ASPIRE FY20 "WHO RUN THE WORLD? BOTS BOTS" - WORKSHOP**
The following guide will help you develop your own virtual assistant bot without the need of any code experience. As a result, your bot will be able to perform the following tasks:
1) Give you answers to the questions stated in the Microsoft Ready FaQ
2) Give you answers to Point of Interest questions
3) Is reachable over Teams and via Voice

From an arechitectural perspective, the following Azure services are being used:
- Bot Framework Virtual Assistant Template thats contains the following Azure services:
  -  Web App
  -  Cosmos Database
  -  Storage account
  -  Azure Cognitive Services:
    - QnA Maker
    - LUIS (Language Understanding Intelligent Service)
    - Azure Search Service
    - Content Moderator
 
```diff
+ Before you begin: 
Please note your LAB CREDENTIALS (username and password). You will need to use it multiple 
times in the upcoming steps. You can use the sticky notes or a notepad for instance.
```

## Overview & reference architecture of the Virtual Assisant
Many customers are looking to deliver conversational assistants tailored to their brand, personalized to their customers, and made available across multiple devices and apps. The virtual assistant solution accelerator (in preview) simplifies the creation of your own assistant, enabling you to get started building in minutes. The scope of Virtual Assistant functionality is broad, typically offering end users a range of capabilities. To increase developer productivity and to enable a vibrant ecosystem of reusable conversational experiences, developers are provided with initial examples of reusable conversational skills. These Skills can be added into a conversational application to light up a specific conversation experience, such as finding a point of interest, interacting with calendar, tasks, email and many other scenarios. Skills are fully customizable and consist of language models for multiple languages, dialogs and code.

![Virtual Assistant Overview](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/virtual-assistant.jpg)

![Virtual Assistant Reference Architecture](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/virtual-assistant-ref-architecture.jpg)

Here you can find more information about the [Virtual Assistant](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-virtual-assistant-introduction?view=azure-bot-service-4.0)


## User Guide  
This guide will help you step by step to perform the tasks that are necessary to create your own Virtual Assistant chatbot. Due to the deployment of a Virtual Assistant or Skill takes around 10 minutes, we will create them both in parallel to save time: a Virtual Assistant and a Point of Interest Skill (remember, a Skill = a Bot, thus you will deploy two bots in total). Later in the lab, we will then join up the Virtual Assistant to your skill (parent and child relationship).

### 1. Run the deployment script to provision your Virtual Assistant and Point of Interest bot
  1. Navigate to https://marketplace.visualstudio.com/items?itemName=BotBuilder.VirtualAssistantTemplate and download the VS Virtual Assistant Template to your desktop (save it). Once downloaded, **double-click on the file and proceed with the installation guide.**

     ![Download the Virtual Assistant Template](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/VA-VS-template.png)
     
  1. Click on the Start menu, search for Visual Studio 2019 and open it. Click on "Create a new project", in the search field, type Virtual Assistant Template. 
  
     ![Virtual Assistant Template Search](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-search.png)
     

  1. Under project name, input **{LABUSERNAME}**-VA - replace {LABUSERNAME} with your actual Lab User alias. Leave "Location" and "Solution Name" as is. Tick the checkbox **"Place solution and project in the same directory"**. Click **"Create"**.
       
       ![Virtual Assistant Template Search](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-create.png)
       
       You should now see all the files and folders required for the Virtual Assistant as shown below:

       ![Virtual Assistant Template Solution](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-solution.png) 
       
  1. Before we can make use of and the Bot deployment script, we will need to know LUIS' authoring key in order to integrate it into the  script to automatically provision the required LUIS Apps. We need to note this key down for the later steps. Open Microsoft Edge and **navigate to https://www.luis.ai**.
  
  ```diff
+ Why do we do this?
The Virtual Assistant Template helps developers to make use of pre-written code samples to include specific 
functionalities that are common for chatbot scenarios. On the motto "Why reinventing the wheel?". The commands you will type in
in the next steps will run the script that automatically deploys different Azure services for specific funtionalities.
In order that these various funtionalities can relate to each other and are well connected, we need to use Visual Studio, go into the code and insert information.
```

  
  1. **Sign in in with your lab credentials** and follow the setup wizard.
  
       ![LUIS Login Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis_0_login.png)
       
       ![LUIS Login Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis-aad-permissions.png)
       
       ![LUIS Setup Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis-setup-0.png)
        
       ![LUIS Setup Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis-setup-1.png)
  
  1. Click on top right corner, **click on your username's initials** and **click on Settings**. Finally, **take note of the authoring key**.
  
       ![LUIS Settings](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis_1_settings.png)
       
       ![LUIS Settings](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis_2_settings.png)
  
   1. Click on the Start menu, search for PowerShell 6 and open it. Before we can deploy the bot into Azure, we need to first login. Run the command below and login.
   
       ```powershell
       az login
       ```   
  
  1. Copy the following command and paste it into sticky notes or a notepad. Replace **{LABUSERNAME} with your Credentials** and make sure to delete the bracktets, too. Copy the new command from your notes and input it to PowerShell 6. This will trigger the deployment of the Virtual Assistant solution into Azure.  

       ```powershell
       cd C:\Users\labuser\source\repos\{LABUSERNAME}-VA\{LABUSERNAME}_VA
       .\Deployment\Scripts\deploy.ps1 -verbose
       ```
       
  1. Once you executed the above command, you will be prompted to fill out with some information within the PowerShell window:
  
       - Bot Name (used as default name for resource group and deployed resources): **{LABUSERNAME}**-VA
       - Azure resource group region: **westus**
       - Password for MSA app registration (must be at least 16 characters long, contain at least 1 special character, and contain at least 1 numeric character): **h67afqgapd@1jhas**
       - LUIS Authoring Region (westus, westeurope, or australiaeast): **westus**
       - LUIS Authoring Key (found at https://luis.ai/user/settings): **{YOURLUISAUTHORINGKEY}**      
       
```diff
+ Why do we do this?
In the backend, the Azure resource "LUIS App" will now be deployed with the information you just inserted.
LUIS is our Languange Understanding Intelligence Service that helps recognize the user's intent, 
no matter how the sentences are phrased.
```

       ![Virtual Assistant - Execute PowerShell script to deploy the bot](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-ps-deploy.png)
       
  1. Click on the Start menu, search for **Git** and open it.
  
  1. Input the following commands:
  
       ```bash
       cd source/repos
       git clone https://github.com/samaea/botframework-solutions
       ```
  
      ![Virtual Assistant - Git console](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/skill-git.png)
 
  1. Open a new PowerShell 6 window to deploy the second bot (Point of Interest Skill) in parallel by **right clicking on the PowerShell icon in the taskbar** and clicking on **PowerShell 64 (x64)**.
  
        ![Virtual Assistant - PowerShell deploy POI Skill](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/PowerShell-openanotherwindow.png)
   
  1. **Input the following command to navigate to the Point of Interest Skill** we just downloaded via Git and run the bot deployment script:
  
       ```powershell
       cd C:\Users\labuser\source\repos\botframework-solutions\skills\src\csharp\pointofinterestskill\pointofinterestskill
       .\Deployment\Scripts\deploy.ps1 -verbose
       ```
       
  1.  Once you executed the above command, you will be prompted to fill out with some information within the window:
  
       - Bot Name (used as default name for resource group and deployed resources): **{LABUSERNAME}**-PointofInterestSkill
       - Azure resource group region: **westus**
       - Password for MSA app registration (must be at least 16 characters long, contain at least 1 special character, and contain at least 1 numeric character): **h67afqgapd@1jhas**
       - LUIS Authoring Region (westus, westeurope, or australiaeast): **westus**
       - LUIS Authoring Key (found at https://luis.ai/user/settings): **{YOURLUISAUTHORINGKEY}**
       
        ![Virtual Assistant - PowerShell deploy VA](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/skill-ps-deploy.png)
       
   1. The Point of Interest (POI) Skill adds the funtionality to the bot to find points of interest and directions, powered by Azure Maps and FourSquare. 
The template automates the deployment of the POI skill, however we need to insert the Azure Maps API key manually:

**Navigate to www.portal.azure.com** and log in with your Lab credentials. On the left, click on **Resource Groups** and find your **{LABUSERNAME}-Pointofinterest Resource Group**. If you click on it, you will find the Azure services that were deployed with the previous steps we performed. **Search for the Azure service with the typ "Azure Maps Account"**. Click on it and find in the left pane **Authentication**. Open and copy the **Primary Key** to your sticky notes/notepad. 
   
   [SCREENSHOTPLACEHOLDER] 
   
   
   7. Now open a new **Visual Studio 2019 window** and click on **open a new project** 
   
   SCREENSHOT
   
   8. Navigate trough the following path (in the screenshot highlighted in yellow and red) to find the Visual Studio solution **Skills.sln**
   
   SCREENSHOT
   
   9. On the right tab, scroll down until you find the **PointOfInterestSkill** tab. Open **appsettings.json and insert your Azure Maps Key in the code to your left under "azureMapsKey" with the "".**
   
   SCREENSHOT
   
   10. Now we need to update the changes. In the same window, again on your right, **right click on the PointOfInterestSkill tab and click "Publish".**
   
   SCREENSHOT  
  
```diff
+ Why do we do this?
You successfully updated the integration of the Azure Maps Key into the Point of Interest Skill 
for your Bot to be able to use Bing Maps to search for locations.
   ```
 
  ```diff
  -    {{USER NEEDS TO CONFIGURE POINTOFINTEREST BOT WITH THE AZURE MAPS API KEY. SO USER WILL NEED TO VISIT THE AZURE PORTAL, OPEN THE POINTOFINTEREST RESOURCE GROUP, FIND THE MAPS COG SERVICE RESOURCE, COPY THE KEY UNDER AUTHENTICATION. FINALLY, USER NEEDS TO OPEN TO C:\Users\labuser\source\repos\botframework-solutions\skills\src\csharp\pointofinterestskill\pointofinterestskill\AppSettings.json and paste the key in there. Afterwards, user will need to open the Skills.sln solution at C:\Users\labuser\source\repos\botframework-solutions\skills\src\csharp\, right click on "Point of Interest" project in Visual Studio, click on Publish and then selected the Point of Interest Web app and click publish. Screenshot needed}}
   ```
       
      
  1.  Up to now, we created two different bots: The Virtual Assistant and the Point of Interest Skill Bot. Since the Virtual Assistant should be able to use the POI skill, we will now link them: Navigate to the **Virtual Assistant Directory and then run the botskills command to join the Virtual Assistant bot with the Point of Interest Skill/Bot:**
  
       1. Copy the command to your sticky notes/notepad and replace **{LABUSERNAME}** with your Lab credentials. Copy again and insert the command into the **PowerShell 6 window** (use one that is already open or open a new window).
  
       ```powershell
       cd C:\Users\labuser\source\repos\{LABUSERNAME}-VA\{LABUSERNAME}_VA\
       botskills connect --botName {LABUSERNAME}-PointofInterestSkill --remoteManifest "http://{LABUSERNAME}-PointofInterestSkill.azurewebsites.net/api/skill/manifest" --luisFolder "C:/Users/labuser/source/repos/botframework-solutions/skills/src/csharp/pointofinterestskill/pointofinterestskill/Deployment/Resources/LU/en/" --cs
       ```
       
      ![Botskills Connect](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/botskills-connect.png)
     
     
The template created an [**Azure service called QnA-Maker**](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/overview/overview) that helps to easily insert "Frequently Asked Questions" websites or files for the bot to make use of and be able to give appropriate answers to questions that are stated in these documents. The FaQ files are populated into the QnA-Maker so-called knowledge base. 

You will now update the knowledge base with our Microsoft Ready FaQs stated in a Word document: 
 
1. On the GitHub guide you are currently working with, scroll up until you find the file **Microsoft Ready FaQ**. Save this file to your desktop. 

Screenshot

2. Navigate in a web browser to **www.qnamaker.ai and sign in with your lab credentials**. You will find 2 knowledgebases: "faq" and "chitchat" already created by the template. **Open the faq and navigate to "settings" on the top right pane**. 

Screenshot

Now **add the just saved "MicrosoftReady-FaQ" word file via the " + Add file" - Button**. Click **Saven and train** on the top right.

Screenshot

You can now test the bot within the QnA-Maker Website Dashboard by **clicking on the top right "Test"**, a window will open where you can phrase your question. Be aware that the question you type in has to be equal or very similar (with the same keywords) to the questions that are stated in the FaQ file you just added. The QnA-Maker is not as "intelligent" as you might think to be able to give the answers no matter how you phrase the question. 

## Enablement of Speech 
The Virtual Assistant template creates and deploys an Assistant with all speech enablement steps provided out of the box. However, we will need to enable the Direct Line Speech channel which procides coordinated access to speech-to-text and text-to-speech from Azure Speech Services that allow bots to become fully voice in, voice out conversational experiences.  We will also need to deploy Azure Speech Services into our resource group, enable the Streaming endpoint on the Bot Service resource and enable WebSockets on our App Service Web App as these are prerequisites of this channel.

  1.  In the Azure Portal, **navigate to the {LABUSERNAME}-VA resource group**, **click on the {LABUSERNAME}-VA** bot resource.
  
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-va-resources.png)
         
   1.   **Click on Channels** and **select the DirectLine Speech channel**.
   
         ![Azure Portal - Enablement of the Direct Line Speech Channel](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-directlinespeech.png)
         
   1.  **Click on Save**.
   
         ![Azure Portal - Enablement of the Direct Line Speech Channel - Save](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-directlinespeech-savepng.png)
         
   1. **Take note of your Direct Line Speech key as you will need this later on**. You can view this by **clicking on "View"** next to the secret key. Once you have taken note of the key, **click on "Cancel"**.
          
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-directlinespeech-key.png)
         
   1.  **Click on Settings** and **tick the checkbox "Enable Streaming Endpoint".
   
          ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-streaming-endpoint.png)
          
   1. **Navigate back to the resource group** by either clicking on the {LABUSERNAME}-VA link at the top or by clicking on the resource groups icon located to the left of the screen.
   
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-navigate-to-the-resource-group.png)
         
   1. **Click on the {LABUSERNAME}-VA App Service resource** (i.e the Web App)
   
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-va-resources-app-service.png)
         
   1. **Click on Configuration** and then on **General Settings**.
   
         ![Azure Portal - App Service Configuration Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-appservice-config.png)
         
   1. **Tick the "On" checkbox next to WebSockets**. Afterwards, **click on Save**.
   
         ![Azure Portal - App Service Configuration Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-appservice-websockets.png)
         
   1. Now we will deploy Azure Speech Services within our resource group. **Click on the "+" symbol** located at the top left of the screen and then **search for "Speech"**.
         
         ![Azure Portal - Create Resource and search for Speech](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-create-resource.png)
         
   1. **Click on "Create"**.
   
         ![Azure Portal - Create Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-create-resource-speech.png)
         
   1. **Fill in the following information** and then **click on "Save"**:
   
       - Name: **SpeechServices**
       - Subscription: Leave as default.
       - Location : **westus2** 
         - **NOTE**: It is important you select **westus2** as this is currently the only supported region that supports DirectLine Speech.
       - Pricing: **S0**
       - Resource Group: **{LABUSERNAME}**-VA
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-create-resource-speech-2.png)
         
   1. **Navigate back to the {{LABUSERNAME}-VA resource group**
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-resourcegroups.png)
         
   1.  **Click on the SpeechServices cognitive service resource**.
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-speechservices.png)
      
   1.  **Click on Keys** and take note of one of the keys. You will need this later on along with the Direct Line Speech key.
         
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-speechservices-key.png)
   
  Well done - your bot should now be capable of speaking over voice. This could be performed over various different means such as a mobile/desktop app or website for example. For this tutorial, we will be using a desktop application called DL Speech Client to allow us to communicate with your bot over speech.
  
  
   1.  **From outside of the Lab's virtual machine**, using a web browser, **Download** the DL Speech Client by using this link:- https://aka.ms/dlspeechclient. You will need to login using your Microsoft credentials if asked. Afterwards, **click on the "Download" button** located at the top left of the screen.
  
   1. Upon successful download of the file, **go to File Explorer** and **click on the "Downloads" folder**. **Right click on the DLSpeechClient.zip file** and **from the dropdown menu, select Extract All**.
  
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/fileexplorer-extractall.png)
         
   1. **Click on Extract** and **ensure** "Show extracted files when complete" is selected.
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/fileexplorer-extract-2.png)
         
   1. Another window will open with the extracted files upon completion. **Click on the DLSpeechClient.application** file and if prompted, **click** on Install.
   
   1. **Click on the settings** gear icon and **Fill in the following information** and then **click on "Ok"**:
   
       - Subscription key: **use the key obtained from step 15**
       - Subscription key region: **westus2**
       - User from Id: **Input your name** 
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/dlspeechclient-settings.png)
   
   1. For the bot secret, **input the key obtained from step 4** and then **click** on Reconnect.
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/dlspeechclient-settings-2.png)
   
   1. Now you can talk to the bot by **clicking** on the microphone symbol.
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/dlspeechclient-settings-3.png)
   
   
   
