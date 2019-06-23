# Azure Virtual Assistant Workshop
**{INFO ABOUT WHAT THE LAB WILL CONSIST OF AND WHAT COG SERVICES WILL BE USED}**

## Overview & reference architecture of the Virtual Assisant
Many customers are looking to deliver conversational assistants tailored to their brand, personalized to their customers, and made available across multiple devices and apps. The virtual assistant solution accelerator (in preview) simplifies the creation of your own assistant, enabling you to get started building in minutes. The scope of Virtual Assistant functionality is broad, typically offering end users a range of capabilities. To increase developer productivity and to enable a vibrant ecosystem of reusable conversational experiences, developers are provided with initial examples of reusable conversational skills. These Skills can be added into a conversational application to light up a specific conversation experience, such as finding a point of interest, interacting with calendar, tasks, email and many other scenarios. Skills are fully customizable and consist of language models for multiple languages, dialogs and code.

![Virtual Assistant Overview](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/virtual-assistant.jpg)

![Virtual Assistant Reference Architecture](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/virtual-assistant-ref-architecture.jpg)

More information about the Virtual Assistant:- https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-virtual-assistant-introduction?view=azure-bot-service-4.0.


## User Guide  
This guide will help you step by step to perform the tasks that are necessary to create your own Virtual Assistant chatbot. Due to the deployment of a Virtual Assistant or Skill takes around 10 minutes, we will create them both in parallel to save time: a Virtual Assistant and a Point of Interest Skill (remember, a Skill = a Bot, thus you will deploy two bots in total). Later in the lab, we will then join up the Virtual Assistant to your skill (parent and child relationship).

### 1. Run the deployment script to provision your Virtual Assistant and Point of Interest bot
  1. Navigate to https://marketplace.visualstudio.com/items?itemName=BotBuilder.VirtualAssistantTemplate and download the VS Virtual Assistant Template. Once downloaded, open the file and proceed with the installation guide.

     ![Download the Virtual Assistant Template](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/VA-VS-template.png)
     
  1. Click on the Start menu, search for Visual Studio 2019 and open it. Click on "Create a new project", seach for the Virtual Assistant template 
  
     ![Virtual Assistant Template Search](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-search.png)
     

  1. Under proejct name, input **{YOURMICROSOFTALIAS}**-VirtualAssistant - replace {YOURMICROSOFTALIAS} with your actual Microsoft alias. Tick the checkbox **"Place solution and project in the same directory"**.
       
       ![Virtual Assistant Template Search](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-create.png)
       
       Finally, you should now see all the files and folders required for the Virtual Assistant as shown below:

       ![Virtual Assistant Template Solution](https://raw.githubusercontent.com/samaea/virtual-assistant-workshop/master/images/va-vs-template-solution.png) 
  
  1. Click on the Start menu, search for Git and open it. Afterwards, input the following command

```bash

