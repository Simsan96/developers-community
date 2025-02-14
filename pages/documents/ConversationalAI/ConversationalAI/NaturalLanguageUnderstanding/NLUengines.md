---
pagename: NLU Engines
redirect_from:
    - conversation-builder-intent-builder-nlu-engines.html
    - conversational-ai-platform-natural-language-understanding-nlu-engines.html
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Conversational AI
subfoldername: Natural Language Understanding
permalink: conversational-ai-natural-language-understanding-nlu-engines.html
indicator: both
---

One of the essential tools of Conversational AI is Natural Language Understanding (NLU). This is what allows Intent Builder to analyze consumer input and assign accurate intents.

While LivePerson provides its own propriety NLU out of the box, Intent Builder also allows you to choose your preferred NLU Engine for analyzing text by routing all NLU analysis and training through an API. 

This API layer of abstraction allows you to choose from the following NLU engines:

- LivePerson's native NLU
- Google Dialogflow
- IBM Watson

{: .important}
If you choose LivePerson's native NLU, no changes need to be made. This engine is already configured and set up by default.

### Language support

LivePerson NLU supports intent detection for English and Spanish.

[IBM Watson supports](https://cloud.ibm.com/docs/services/assistant?topic=assistant-language-support#language-support) Arabic, Simplified and Traditional Chinese (China), Czech (Czech), Dutch (Netherlands), English (United States), French (France), German (Germany), Italian (Italy), Japanese (Japan), Korean (Korea), Portuguese (Brazil), and Spanish (Spain).

[Google Dialogflow supports](https://cloud.google.com/dialogflow/docs/reference/language) English (India, United States), French (Canada, France), German (Germany), Hindi (India), Indonesian (Indonesia), Italian (Italy), Japanese (Japan), Korean (Korea), Norwegian (Norway), Polish (Poland), Portuguese (Brazil, Portugal), Russian (Russia), Spanish (Latin America, Spain), Swedish (Sweden), Thai (Thailand), Turkish (Turkey), and Ukranian (Ukraine).

### LivePerson's NLU engine

There are two versions of LivePerson's NLU engine: version 1 (v1) and version 2 (v2).

#### LivePerson NLU v1

This version is a recommender model based on Word Mover's Distance (WMD) algorithms. It's intended to be used if you have fewer than 5 intents and fewer than 20 training phrases per intent. Additionally, performance is enhanced when you have a lot of custom entities.

{: .important}
NLU v1 supports English and Spanish. Also, NLU v1 doesn't require the model to be trained.

#### LivePerson NLU v2

This version is a classifier model based on Convolutional Neural Network (CNN) using Fasttext embeddings. The primary feature of NLU v2 is enabling a separate brand-specific model, built and *trained* for each domain. NLU v2 is a scalable solution that can handle a greater volume of requests, providing faster response times and accuracy.

To perform effectively, NLU v2 expects large sets of data (both intents and training phrases). Additionally, it's preferable to have a maximum of *two* entities per sample sentence, for example, "I want a COLOR SIZE shirt."

When you create a domain with NLU v2 and use it in LiveIntent or in Conversation Builder, the following is recommended:

* At least 5 intents in order to [train](intent-builder-domains.html#train-a-liveperson-nlu-v2-domain)
* At least 20 training phrases per intent

{: .important}
NLU v2 supports only English. Also, NLU v2 requires the model to be [trained](intent-builder-domains.html#train-a-liveperson-nlu-v2-domain).

### Connect a 3rd-party NLU engine

#### 3rd-party NLU limitations

- Length of the 3rd-party NLU domain name should not exceed 64 characters. (Watson limitation)
- Each domain can only support one language, which is specified on the Domain Settings page.
- LivePerson does not support "pulling" into Intent Builder existing models that have been trained in IBM Watson or Google Dialogflow. Only model "push" is supported; this is accomplished by training the model in Intent Builder.

#### Step 1: Enable 3rd-party NLU support

Contact your LivePerson account administrator to enable your account for 3rd-party NLU support.

Once this is done, you can start creating domains with 3rd-party NLU.

#### Step 2: Sign up and get the API keys

Repeat this step twice to create *two* sets of IBM Watson or Google DialogFlow service credentials. When you [train](intent-builder-domains.html#train-a-3rd-party-nlu-domain) the intents in a domain for the first time in Intent Builder, you'll use the first set of credentials. Those credentials will then be active for the first model version that gets created. *Since only one set of credentials can be active at a time*, you'll need to use the second set of credentials the second time you train. And with each subsequent training, you'll need to alternate back and forth between the credentials.

<img class="fancyimage" style="width:600px" src="img/ConvoBuilder/3rdPartyNLU/serviceCreds.png">

##### IBM Watson

1. Register for or log in to an IBM Cloud account.

<img class="fancyimage" style="width:600px" src="img/ConvoBuilder/3rdPartyNLU/watson_login_ibmcloud.png">

2. Create or access a Watson Assistant resource.
<img class="fancyimage" style="width:600px" src="img/ConvoBuilder/3rdPartyNLU/watson_resource_list.png">


3. Generate Service Credentials with the role of Manager and an Auto Generated Service ID.
<img class="fancyimage" style="width:600px" src="img/ConvoBuilder/3rdPartyNLU/watson_create_resource.png">

4. View and copy the newly created credentials.
<img class="fancyimage" style="width:600px" src="img/ConvoBuilder/3rdPartyNLU/watson_api_keys.png">


##### Google Dialogflow

1. Log in to the Dialogflow console.

2. Create a new Dialogflow agent (which will create a new Google project).

3. Create a new Service Account for the newly created Google project with the role of Dialogflow API Admin.

4. Create a JSON-formatted private key for the service account by clicking the Create key.

5. View and copy the created key. This will be used in your Dialogflow config data.

#### Step 3: Add a domain for the 3rd-party NLU provider

In Intent Builder, add a domain *that uses the 3rd-party NLU engine as its NLU provider*. For help with this step, see [here](intent-builder-domains.html#add-a-domain).

You can import the intents and entities, or manually add them later but before proceeding to step 5.

#### Step 4: Create NLU provider credentials

In Intent Builder, in the domain that you created in the previous step, create NLU provider credentials for the 3rd-party NLU engine. For help with this step, see [here](intent-builder-domains.html#create-a-3rd-party-nlu-provider-credential).

This is when you'll copy and paste in the credentials that you downloaded from IBM Watson or Google Dialog flow (step 2 above).

#### Step 5: Train the domain

In Intent Builder, train the domain. For help with this step, see [here](intent-builder-domains.html#train-a-3rd-party-nlu-domain).

Once training is completed, you can use the intent tester to start testing with the model version.
