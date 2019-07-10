# Azure Virtual Assistant Workshop
**Welcome to the one and only ASPIRE FY20 "WHO RUN THE WORLD? BOTS BOTS" - WORKSHOP**

The following guide will help you developing step by step your own Virtual Assistant Chatbot without the need of any code experience. We will use the Virtual Assistant template along with the Point of Interest Skill. Due to the deployment taking around 10 minutes, we will create them both in parallel to save time - remember, a Skill = a Bot, thus you will deploy two bots in total. Later in the lab, we will then join up the Virtual Assistant to your skill (parent and child relationship). As a result, your bot will be able to perform the following tasks:
1) Give you answers to the questions stated in the Microsoft Ready FaQ
2) Give you answers to Point of Interest questions
3) Is reachable over Teams and via Voice

From an architectural perspective, the following Azure services are being used:
- **Bot Framework Virtual Assistant Template thats contains the following Azure services:**
  -  Web App
  -  Cosmos Database
  -  Storage account
  -  **Azure Cognitive Services:**
      - QnA Maker
      - LUIS (Language Understanding Intelligent Service)
      - Azure Search Service
      - Content Moderator
 
```diff
+ Before you begin: 
Please note down your LAB CREDENTIALS (username and password). You will need to use it multiple 
times in the upcoming steps. You can use the sticky notes or a notepad for instance.
```

## Setting the Scene

You will be following this guide and **working on your bot using an Azure Virtual Machine (VM)**. A VM is an emulated computer system created using software. When you start the VM you will see that it looks basically the same as your local desktop. The difference is that it uses physical system resources of the cloud and is isolated to your local computer. With this setting we can provide you with all the tools you need to create the bot without having to download them to your local computer. 

Please go to **URL** and **sign in with your Lab User credentials**, unique to you. Then **start the VM** as you can see in the screenshot below

   ![lab user VM](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/VM.png)

## Overview & reference architecture of the Virtual Assisant
Many customers are looking to deliver conversational assistants tailored to their brand, personalized to their end customers, and made available across multiple devices and apps. The Virtual Assistant template is a solution accelerator that simplifies the creation of your own assistant, enabling you to get started developing in minutes. To increase developer productivity and to enable the reuse of conversational experiences, developers are provided with initial examples of conversational skills. These Skills can be added into a conversational application (like a chatbot) to enhance a specific conversation experience, such as finding a point of interest, interacting with calendar, tasks, email and many other scenarios. Skills are fully customizable and consist of language models for multiple languages, dialogs and code.

In this tutorial we will make use of the Point of Interest Skill.

![Virtual Assistant Overview](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/virtual-assistant.jpg)

![Virtual Assistant Reference Architecture](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/virtual-assistant-ref-architecture.jpg)

Here you can find more information about the [Virtual Assistant](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-virtual-assistant-introduction?view=azure-bot-service-4.0)


### 1. Run the deployment script to provision your Virtual Assistant and Point of Interest bot
  1. Navigate to https://marketplace.visualstudio.com/items?itemName=BotBuilder.VirtualAssistantTemplate and download the VS Virtual Assistant Template to your desktop (save it). Once downloaded, go to the solution and **double-click on the file and proceed with the installation guide.**

     ![Download the Virtual Assistant Template](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/VA-VS-template.png)
     
  1. Click on the **Start menu, search for Visual Studio 2019** and open it. Click on **"Create a new project"**, in the search field, type **Virtual Assistant Template.** 
  
     ![Virtual Assistant Template Search](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-search.png)
     

  1. Under project name, input your unice Lab User name **{LABUSERNAME}**-VA - --> replace {LABUSERNAME} with your actual Lab User alias. **Leave "Location" and "Solution Name"** as is. Tick the checkbox **"Place solution and project in the same directory"**. Click **"Create"**.
       
       ![Virtual Assistant Template Search](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-create.png)
       
      
      
      **You should now see all the files and folders required for the Virtual Assistant as shown below:**

       ![Virtual Assistant Template Solution](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-solution.png) 
       
  1. Before we can make use of and the Bot deployment script, we will need to know LUIS' authoring key in order to integrate it into the  Virtual Assistant script. This way, it will automatically create the required LUIS Apps in Azure. We need to note this key down for the later steps. Open Microsoft Edge and **navigate to https://www.luis.ai**. **If you are stuck on the "Loading" page** for more than 10 seconds, **refresh the page by clicking on F5 or the refresh button in the web browser**
  
      ```diff
      + Why do we do this?      
      The Virtual Assistant Template helps developers to make use of pre-written code samples to include 
      specific functionalities that are common for chatbot scenarios. On the motto "Why reinventing the wheel?". 
      The commands you will type in the next steps will run the script that automatically deploys different 
      Azure services for specific funtionalities. In order that these various funtionalities can relate to 
      each other and are well connected, we need to use Visual Studio, go into the code and insert information.      
      ```

  
  1. **Sign in in with your lab credentials** and follow the setup wizard. You will be promted to acceppt permissions and be directed to the home screen of LUIS. See the screenshots for your reference.
  
       ![LUIS Login Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis_0_login.png)
       
       ![LUIS Login Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis-aad-permissions.png)
       
       ![LUIS Setup Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis-setup-0.png)
        
       ![LUIS Setup Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis-setup-1.png)
  
  1. Click on top right corner, **click on your username's initials** and **click on Settings**. Finally, **note down the authoring key** to your sticky notes/notepad.
  
       ![LUIS Settings](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis_1_settings.png)
       
       ![LUIS Settings](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/luis_2_settings.png)
  
  
   1. We will now use PowerShell 6 to log into Azure and create the Virtual Assistant resource. 
   
      On your computer, **click on the Start menu (Windows Sign)**, search for **PowerShell 6 and open it**. Before we can deploy the bot into Azure, we need to first login to Azure. **Run the command below** and login. This will **open a new window**. Once the authentication is complete, **switch back** to the PowerShell window to continue with further commands.
   
       ```powershell
       az login
       ```   
  
  1. **Copy the following command and paste it** into sticky notes or a notepad. Replace **{LABUSERNAME} with your Credentials** and make sure to delete the bracktets, too. **Copy the new command from your notes and paste it into the PowerShell 6 window**. This will trigger the deployment of the Virtual Assistant solution into Azure.  

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
      In the backend, the Azure resource "LUIS App", as part of the Virtual Assistant, will now be 
      deployed with the information you just inserted. LUIS is our Languange Understanding Intelligence 
      Service that helps recognize the user's intent, 
      no matter how the sentences are phrased.
      ```

       ![Virtual Assistant - Execute PowerShell script to deploy the bot](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-ps-deploy.png)
       
  1. We are now going to clone the botframework solutions repository that contains of the ready-made Point of Interest skill. The Point of Interest (POI) Skill adds the funtionality to the bot to find points of interest and directions, powered by Azure Maps and FourSquare. Click on the Start menu, search for **Git** and open it.
  
  1. Input the following commands:
  
       ```bash
       cd source/repos
       git clone https://github.com/samaea/botframework-solutions
       ```
       
  1. **Please wait** until Git completes cloning the repository otherwise you may face unexpected errors. 
       
        - Note: The official Microsoft repository for the Virtual Assistant/Skills template is https://github.com/microsoft/botframework-solutions, but I have copied my own version to ensure updates do not affect this lab.
  
      ![Virtual Assistant - Git console](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/skill-git.png)
 
  1. Open a **new** PowerShell 6 window (right-click on the PowerShell icon in the tab bar to open a new window) to deploy the Point of Interest Skill in parallel by **right clicking on the PowerShell icon in the taskbar** and clicking on **PowerShell 64 (x64)**.
  
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
  
  
  1. **Please wait** until the both PowerShell deployment windows is finished (i.e the Virtual Agent and the Point of Interest deployment) before proceeding. Otherwise you may face unexpected errors. You will know it is finished when you see an identical output from both deployments:
  
        ![Virtual Assistant - PowerShell deployment done](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/powershell-deploydone.png)
  
  1. The template automates the deployment of the POI skill, however we need to insert the Azure Maps API key manually:

       **Navigate to www.portal.azure.com** and log in with your Lab credentials. On the left, click on **Resource Groups** and find your **{LABUSERNAME}-Pointofinterest Resource Group**. If you click on it, you will find the Azure services that were deployed with the previous steps we performed. **Search for the Azure service "Azure Maps Account"**. Click on it and find in the left pane **Authentication**. Open and copy the **Primary Key** to your sticky notes/notepad. 
   
        ![Azure Maps Account](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/AzureMapsAccount.jpg)
   
       **Search for the Azure service "Azure Maps Account"**. Click on it and find in the left pane **Authentication**. Open and copy the **Primary Key** to your sticky notes/notepad. 
 

        ![Azure Maps Key](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/AzureMapsKey.jpg)

  1. Now open a new **Visual Studio 2019 window** by **right clicking on the Visual Studio icon in the taskbar** and clicking on **Visual Studio 2019**. A new window should open.
  
        ![Visual Studio new project](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/vs-new-window.png)
   
   1. Click on **open a new project**.
   
        ![Visual Studio new project](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/VSnewproject.png)   
   
  1. Navigate trough the following path (in the screenshot highlighted in yellow and red) to find the Visual Studio solution **Skills.sln**
   
        ![Skill.sln project](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/skillssln.png)
   
  1. On the right tab, scroll down until you find the **PointOfInterestSkill** tab. Open **appsettings.json and insert your Azure Maps Key in the code to your left under "azureMapsKey" with the "".**
   
        ![Update Azure Maps Key](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/appsettingsPOI.png)
   
  1. Now we need to update the changes. In the same window, again on your right, **right click on the PointOfInterestSkill tab and click "Publish".**
   
        ![Point of Interest publish](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/POIpublish.png) 
  
  1. A wizard should open up. **Click on "Select Existing"** and then **Publish**.
  
        ![Point of Interest publish](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/vs-publish.png)
        
  1. Expand the menu by **clicking on the "ReadyUser{LABUSERNAME}-PointofInterestSkill" folder** and **selecting the "ReadyUser{LABUSERNAME}-PointofInterestSkill"** app service. Finally, **click on Publish**.
  
        ![Point of Interest publish](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/vs-publish-1.png)
        
        ![Point of Interest publish](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/vs-publish-3.png)        
  
      ```diff
      + Why do we do this?
      You successfully updated the integration of the Azure Maps Key into the Point of Interest Skill 
      for your Bot to be able to use Bing Maps to search for locations.
       ```
       
      
  1.  Up to now, we created two different bots: The Virtual Assistant and the Point of Interest Skill Bot. Since the Virtual Assistant should be able to use the POI skill, we will now link them: Navigate to the **Virtual Assistant Directory and then run the botskills command (below) to join the Virtual Assistant bot with the Point of Interest Skill/Bot:**
  
      Copy the command to your sticky notes/notepad and **replace {LABUSERNAME}** with your Lab credentials. Copy again and insert the command into the **PowerShell 6 window** (use one that is already open or open a new window). Run the command by hitting "Enter".
  
       ```powershell
       cd C:\Users\labuser\source\repos\{LABUSERNAME}-VA\{LABUSERNAME}_VA\
       botskills connect --botName {LABUSERNAME}-PointofInterestSkill --remoteManifest "http://{LABUSERNAME}-PointofInterestSkill.azurewebsites.net/api/skill/manifest" --luisFolder "C:/Users/labuser/source/repos/botframework-solutions/skills/src/csharp/pointofinterestskill/pointofinterestskill/Deployment/Resources/LU/en/" --cs
       ```
       
      ![Botskills Connect](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/botskills-connect.png)
     
  1. The above command made some changes to the code. To reflect these changes for the Virtual Assistant in the cloud, we need to push these updates back to Azure. Similarly what we did with the PointOfInterest Skill solution, we will do the same for the Virtual Assistant solution by using Visual Studio. **Switch** to the Visual Studio window that has your **ReadyUser{LABUSERNAME}-VA** solution opened and follow the **Publish** process (i.e identical to what you did in steps 15-17, but **this time with the Virtual Assistant solution** instead and **selecting ReadyUser{LABUSERNAME}-VA** instead of **ReadyUser{LABUSERNAME}-PointofInterestSkill**.
  
       ![Virtual Assistant Template Solution](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-solution.png) 

       ![Virtual Assistant Template Solution](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/vs-publish-4.png) 

### 2. Azure QnA Maker to update the bot with our Microsoft Ready FaQs 
 
The template created an [**Azure service called QnA-Maker**](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/overview/overview) that helps to easily insert "Frequently Asked Questions" websites or files for the bot to make use of and be able to give appropriate answers to questions that are stated in these documents. The FaQ files are populated into the QnA-Maker so-called knowledge base. We will now update the QnA Maker with our Microsoft Ready FaQs in a word document.
 
  1.  Open an In-Private browsing in Microsoft Edge. Download the **Microsoft Ready FaQ** document by navigating to: https://aka.ms/msreadyfaq and input your Microsoft credentials when requested. Click on **Save**.
  
         ![In-Private Edge session](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/e936af27-1cc7-41f3-ab12-14cf3705f140-en.png)

  1.  Navigate in a web browser to **www.qnamaker.ai**, click on **My Knowledge bases** and **sign in with your lab credentials**. You will find 2 knowledgebases: "faq" and "chitchat" already created by the template. 

         ![QnA Maker knowledge bases](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/QnaMakerhome.png)



  1.  **Open the faq and navigate to "settings" on the top right pane**. 

         ![QnA Maker settings](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/qnasettings.png)



  1.  Now **add the just saved "MicrosoftReady-FaQ" word file via the " + Add file" - Button**. You should find the file under the **Downloads** folder. Click **Save and train** on the top right.

         ![QnA Maker MS Ready FaQ update](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/qnasavenandtrain.png)


  1.  If you click on the top pane on **Edit** and collapse the **Source: custom editorial**, you will find the newly added Source **MicrosoftReadyFaQ** with the questions and answers listed on the right. 


      ```diff
      + Good to know
    
      Under each stated question is the option " + Add alternative phrasing". QnA-Maker is not fully intelligent
      by itself and out of the box. As soon as a user types in a question, QnA-Maker searches through its 
      knowledge base and based on a probability value it will give back the answer with the highest score. 
      However, user phrase their questions not always 1:1 as stated in the knowledge base. If relevant
      keywords are included, chances are high the right anwers will be responded. To add more intelligence 
      and understand the user's real intent behind a question, LUIS is a good service to work with. 
      ```

  1.  You can now test your FaQ by clicking on **Test** in the top right pane. Play a bit with rephrasing the questions as seen in the screenshot example

         ![QnA Maker Test](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/qnatest.png)


  1.  In order to make these changes to the knowledge base effective, we need to publish it as shown in the screenshot, **click on the top pane on publish and then on the publish-button**:

         ![FaQ Publish](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/faqpublish.png)

  1.  You will receive a success message:

         ![FaQ Publish Success](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/faqpublishsuccess.jpg)

  1. As a final step, we need to update your Virtual Assistant's Dispatch to recognise the new QnAMaker FAQ changes. **In PowerShell**, **navigate to the root VA directory (if you are not there already)** and run the botskills refresh command:
  
       ```powershell
       cd C:\Users\labuser\source\repos\{LABUSERNAME}-VA\{LABUSERNAME}_VA
       botskills refresh --cs --verbose
       ```
       
        ![BotSkills refresh](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/botskills-refresh.png)

PLACEHOLDER FOR CHITCHAT ADJUSTMENTS - NEED TO CHECK WHETHER TIME IS SUFFICIENT

### 3. Enablement of Speech 
The Virtual Assistant template creates and deploys an Assistant with all speech enablement steps provided out of the box. However, we will need to enable the Direct Line Speech channel to provide coordinated access to speech-to-text and text-to-speech functionalities from Azure Speech Services. These allow bots to bring full voice in, voice out conversational experiences.  We will also need to deploy Azure Speech Services into our Azure resource group, enable the streaming endpoint on the Bot Service resource and enable WebSockets on our App Service Web App as these are prerequisites of this channel.

  1.  In the Azure Portal, **navigate to your {LABUSERNAME}-VA resource group**, **click on the {LABUSERNAME}-VA Web App Bot Resource**.
  
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-va-resources.png)
         
   1.   **Click on Channels** and **select the DirectLine Speech channel**.
   
         ![Azure Portal - Enablement of the Direct Line Speech Channel](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-directlinespeech.png)
         
   1.  **Click on Save**.
   
         ![Azure Portal - Enablement of the Direct Line Speech Channel - Save](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-directlinespeech-savepng.png)
         
   1. **Note down your Direct Line Speech key as you will need this later on**. You can view this by **clicking on "View"** next to the secret key. Once you have taken note of the key, **click on "Cancel"**.
          
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-directlinespeech-key.png)
         

      ```diff
      + Good to know
      
      You have now successfully enabled the Direct Line Speech Channel with only a few clicks. This is
      one of the benefits of the Microsoft Bot Framewok: Developers can easily integrate several functionalities
      with a few clicks instead of time-consuming coding. [Here](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0) you can find all the available communication 
      channels that the bot can be extended to.
      ```

   1.  We will now enable the Bot Framework Protocol Streaming Extensions support for optimal, low-latency speech interaction. **Click on Settings** on the left and **tick the checkbox "Enable Streaming Endpoint"**.
   
          ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-streaming-endpoint.png)
  
  
  
   1. **Navigate back to the resource group** by either clicking on the **{LABUSERNAME}-VA link at the top** or by **clicking on the resource groups icon** located to the left of the screen.
   
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-navigate-to-the-resource-group.png)
 
 
 
   1. **Click on the {LABUSERNAME}-VA App Service resource** 
   
         ![Azure Portal - Resources in the VA RG](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-va-resources-app-service.png)
  
  
  
   1. **Click on Configuration** and then on **General Settings**.
   
         ![Azure Portal - App Service Configuration Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-appservice-config.png)
         
   
   
   1. **Tick the "On" checkbox next to WebSockets**. Afterwards, **click on Save**.
   
      ![Azure Portal - App Service Configuration Page](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-appservice-websockets.png)


      ```diff
      + Good to know
      
      You have now enabled all the extensions and set the necessary settings in the bot resourcec to activate
      speech functionalities.
  
   1. Now we need to deploy Azure Speech Services within our resource group. This is the Azure resource that will finally give the bot speech functionality. To read more about the Speech Service, find the documentation [here](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/overview)
   
**Click on the "+" symbol** located at the top left of the screen and then **search for "Speech"**.
         
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
 
 
 
   1. **Navigate back to the {{LABUSERNAME}-VA resource group** where all the Azure resources we deployed in the steps earlier are listed
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-resourcegroups.png)
         
 
 
 1.  **Click on the "SpeechServices" Cognitive Service resource**.
   
        ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-speechservices.png)
      


1.  **Click on Keys** and note down KEY 1 on your sticky notes/notepad. You will need this later on along with the Direct Line Speech key you noted down earlier.
         
       ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/portal-speechservices-key.png)
   
      ```diff
      + Good to know     
     Well done - your bot is now be capable of speaking over voice. This could be performed over 
     various different means, such as a mobile/desktop app or website for example. For this tutorial, 
     we will be using a desktop application called "DL Speech Client" to allow us to communicate with 
     your bot over speech.
       ```
  
   1.  **Leave the Virtual Machine by minimizing the window and go back to your desktop**. Open a new web browser window, **Download** the DL Speech Client by using this link:- https://aka.ms/dlspeechclient. You will need to login using your Microsoft credentials if asked.
   
       - **NOTE: This application is for internal Microsoft use only and must not be distributed externally.**
  
   1. Upon successful download of the file, **open the File Explorer** and **click on the "Downloads" folder** to find the just downloaded zip file. **Right click on the DLSpeechClient.zip file** and **from the dropdown menu, select Extract All**.
  
  
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/fileexplorer-extractall.png)
         

1. **Click on Extract** and **ensure "Show extracted files when complete" is selected.**
   
      ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/fileexplorer-extract-2.png)
         
 
 1. Another window will open with the extracted files upon completion. **Click on the DLSpeechClient.application** file and if prompted, **click on Install.**


   1. **Click on the settings** gear icon and **Fill in the following information** and then **click on "Ok"**:
   
       - Subscription key: **use the key obtained from step 15** (Speech Service KEY 1)
       - Subscription key region: **westus2**
       - User from Id: **Input your name** 
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/dlspeechclient-settings.png)
   
   
   1. For the bot secret, **input the key obtained from step 4** (Direct Line Speech Service key) and then **click on Reconnect.**
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/dlspeechclient-settings-2.png)
   
   
   1. Now you can talk to the bot by **clicking on the microphone symbol.**
   
         ![Azure Portal - Deploy Speech Services](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/dlspeechclient-settings-3.png)
   
   
   
