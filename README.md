# Azure Virtual Assistant Workshop
**Welcome to the one and only ASPIRE FY20 "WHO RUN THE WORLD? BOTS BOTS" - WORKSHOP**
The following guide will help you develop your own virtual assistant bot without the need of any code experience. As a result, your bot will be able to perform the following tasks:
1) 
2)
3)

From an arechitectural perspective, the following Azure services are being used:
- Bot Framework Virtual Assistant Template
- Azure Cognitive Services:
  - QnA Maker
  - LUIS (Language Understanding Intelligent Service)
 
```diff
Before you begin: Please note your LAB CREDENTIALS (username and password). 
You will need to use it multiple times in the upcoming steps.
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
Why do we do this?
The Virtual Assistant Template helps developers to make use of pre-written code samples to include specific 
functionalities that are common for chatbot scenarios. On the motto "Why reinventing the wheel?". 
In order that these several funtionalities can relate to each other and are well connected, we need to create Azure resources and insert information.
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
  
  1. Input the following command to navigate to your Virtual Assistant's folder and deploy the solution into Azure. Replace **{LABUSERNAME}** with your actual Lab User alias. Make sure to delete the brackets so that you only have your lab username stated. 

       ```powershell
       cd C:\Users\labuser\source\repos\{LABUSERNAME}-VA\{LABUSERNAME}_VA
       .\Deployment\Scripts\deploy.ps1 -verbose
       ```
       
  1. Once you executed the above command, you will be prompted to fill out with some information within the powershell window:
  
       - Bot Name (used as default name for resource group and deployed resources): **{LABUSERNAME}**-VA
       - Azure resource group region: **westus**
       - Password for MSA app registration (must be at least 16 characters long, contain at least 1 special character, and contain at least 1 numeric character): **h67afqgapd@1jhas**
       - LUIS Authoring Region (westus, westeurope, or australiaeast): **westus**
       - LUIS Authoring Key (found at https://luis.ai/user/settings): **{YOURLUISAUTHORINGKEY}**      
       
```diff
Why do we do this?
In the backend, the Azure resource "LUIS App" will now be deployed with the information you just inserted.
```

       ![Virtual Assistant - Execute PowerShell script to deploy the bot](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-ps-deploy.png)
       
  1. Click on the Start menu, search for **Git** and open it.
  
  1. Input the following commands:
  
       ```bash
       cd source/repos
       git clone https://github.com/microsoft/botframework-solutions
       ```
  
      ![Virtual Assistant - Git console](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/skill-git.png)
 
  1. Open another PowerShell window to deploy the second bot in parallel by **right clicking on the PowerShell icon in the taskbar** and clicking on **PowerShell 64 (x64)**.
  
        ![Virtual Assistant - PowerShell deploy POI Skill](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/PowerShell-openanotherwindow.png)
   
  1. Input the following command to navigate to the Point of Interest Skill we just downloaded via Git and run the bot deployment script:
  
       ```powershell
       cd C:\Users\labuser\source\repos\botframework-solutions\skills\src\csharp\pointofinterestskill\pointofinterestskill
       .\Deployment\Scripts\deploy.ps1 -verbose
       ```
       
  1.  Once you executed the above command, you will be prompted to fill out with some information:
  
       - Bot Name (used as default name for resource group and deployed resources): **{LABUSERNAME}**-PointofInterestSkill
       - Azure resource group region: **westus**
       - Password for MSA app registration (must be at least 16 characters long, contain at least 1 special character, and contain at least 1 numeric character): **h67afqgapd@1jhas**
       - LUIS Authoring Region (westus, westeurope, or australiaeast): **westus**
       - LUIS Authoring Key (found at https://luis.ai/user/settings): **{YOURLUISAUTHORINGKEY}**
       
   1. {{USER NEEDS TO CONFIGURE POINTOFINTEREST BOT WITH THE AZURE MAPS API KEY. SO USER WILL NEED TO VISIT THE AZURE PORTAL, OPEN THE POINTOFINTEREST RESOURCE GROUP, FIND THE MAPS COG SERVICE RESOURCE, COPY THE KEY UNDER AUTHENTICATION. FINALLY, USER NEEDS TO OPEN TO C:\Users\labuser\source\repos\botframework-solutions\skills\src\csharp\pointofinterestskill\pointofinterestskill\AppSettings.json and paste the key in there. Afterwards, user will need to open the Skills.sln solution at C:\Users\labuser\source\repos\botframework-solutions\skills\src\csharp\, right click on "Point of Interest" project in Visual Studio, click on Publish and then selected the Point of Interest Web app and click publish. Screenshot needed}}
       
      ![Virtual Assistant - PowerShell deploy VA](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/skill-ps-deploy.png)
      
  1.  Upon successful completion of the Skill deployment, we will now link our Virtual Assistant bot with the ready-made Point of Interest skill. Navigate to the Virtual Assistant Directory and then run the botskills command to join the Virtual Assistant bot with the Point of Interest Skill/Bot:
       1. Replace **{LABUSERNAME}** with your actual Lab alias.
  
       ```powershell
       cd C:\Users\labuser\source\repos\{LABUSERNAME}-VA\{LABUSERNAME}_VA\
       botskills connect --botName {LABUSERNAME}-PointofInterestSkill --remoteManifest "http://{LABUSERNAME}-PointofInterestSkill.azurewebsites.net/api/skill/manifest" --luisFolder "C:/Users/labuser/source/repos/botframework-solutions/skills/src/csharp/pointofinterestskill/pointofinterestskill/Deployment/Resources/LU/en/" --cs
       ```
       
      ![Botskills Connect](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/botskills-connect.png)
        
