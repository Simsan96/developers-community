---
pagename: Getting Started
redirect_from:
  - customer-facing-bots-deploying-bots-to-liveengage.html
  - customer-facing-bots-considerations.html
  - customer-facing-bots-deploying-bots-on-live-chat.html
  - customer-facing-bots-deploying-bots-on-messaging.html
  - customer-facing-bots-getting-started.html
  - customer-facing-bots-limitations.html
  - customer-facing-bots-overview.html
  - customer-facing-bots-prerequisites.html
  - bot-connectors-getting-started.html
sitesection: Documents
categoryname: "Conversational AI"
documentname: Third-Party Bots
permalink: third-party-bots-getting-started.html
indicator:
---

### Introduction

External Bot frameworks and Bot builders can be enabled and managed through LiveEngage just like a normal human agent.

Using Third-Party Bots, you can provision a connector for IBM Watson, Google Dialogflow, Amazon Lex, Microsoft Bot
Framework or leverage LivePerson Functions with the Custom Integrations Option.

{: .important}
If you need to connect a external bot that does not have a pre-built connector, see [this document](third-party-bots-custom-integration.html) for instructions.

Each connector provides the functionality to

- send/receive text messages

- send [structured content](getting-started-with-rich-messaging-introduction.html)

- transfer the conversation to other skills

- change Time To Response for a messaging conversation

- close a conversation

Some connectors may provide more or less functionality depending on the specifics of the product to which it is connecting.

### Bot Lifecycle

The running Bot can have different states based on the health status of the services it utilizes, such as LivePerson APIs, AI vendors, etc.

#### Offline

The Bot is offline and won't accept any conversations.

#### Online

The Bot is online and will accept and process new conversations.

#### Vendor Interruption

The Bot is online and will accept new conversations, but will directly escalate them to the default transfer skill, because the set up AI Vendor is not reachable/working.

#### Service Interruption

The Bot is in the delayed state and will not accept new conversation or process existing conversations. This state is a result of an interruption within Liveperson APIs/servers.

In this state the bot will try to restart automatically once every minute until the interruption is resolved.

### Limitations

#### Supported customer content

Currently the connectors only support text input from the customer. If the customer sends an image or a file to the Bot, the Bot will replace it with a special identifier so the Bot can handle this special use-case with custom code.

The send identifier is **com.liveperson.bot-connectors.consumer.send-file**

#### Support for different messaging channels and the corresponding rich content

The Bot Connector system is designed to support [all relevant rich content](getting-started-with-rich-messaging-rich-messaging-channel-capabilities.html), since it only forwards the received structured content and metadata to LivePerson's messaging and chat service. We have verified and tested the support for **Web Messaging**, **Facebook** and **Apple Business Chat**. All other channels are not verified, but should work if you send the right structured content for the channel. If you experience any issues, please contact LivePerson support or your account team.

#### Creating and starting Bots

Due to limitations within LiveEngage's permission system, it is not possible for an operator with the Agent or the Agent Manager profiles to create new bots or start bots. However, they are still able to stop, edit and delete existing bots.

If you want to enable creating and starting bots for the Agent and Agent Manager profiles, you need to create a new custom Profile, which will derive its base permissions from the Campaign Manager or Admin profiles. Then, make sure to enable the needed permissions for creating and starting bots only while disabling any other permissions. Afterwards, you will need to assign this new Profile to the Agent/Agent Manager who should be able to start/create bots.

<img style="width:600px" src="img/botconnectordashboard/campaign_manager_bot_permissions.png">

Minimal set of permissions for creating and starting bots for Campaign Manager Profile

<img style="width:600px" src="img/botconnectordashboard/administrator_bot_permissions.png">

Minimal set of permissions for creating and starting bots for Administrator Profile

#### Intent and actions evaluation

Please note that your bot setup should always return an intent or an action as a response. If you return an intent without actions or messages, or return no intent at all, the bot will consider this an error and escalate to the default escalation skill.

### Create Bot User in LiveEngage

1. Add a new user in LiveEngage, choose "Bot" for “User type”. If “User type” is not available, contact your LivePerson account manager to enable the feature.

  <img style="width:600px" src="img/dialogflowversion2/image_0.png">

{:start="2"}

2. Add login method as "API key" and generate new API key for the new user

   <img style="width:600px" src="img/dialogflowversion2/image_1.png">

{:start="3"}

3. Make sure the user has chat and/or messaging slot > 0 based on the target channel of the bot.

{:start="4"}

4. Set Max No of Live Chats

   - If Chat in the drop down select - Value > 1.

   - If Messaging Max No of Live Chats -> **No Chats and Max No of Messaging Conversations to Custom Setting and enter a value greater than 0**

{:start="5"}

5. Find api key name in bot user profile

   <img style="width:400px" src="img/dialogflowversion2/image_2.png">

---

### Provision a connector

To access Third-Party Bots, contact your Account Manager to enable the the feature in LiveEngage for your account.

Upon logging in to LiveEngage, you will see the Conversation AI Tab:

<img class="fancyimage" style="width:750px" src="img/botconnectordashboard/conversational_ai_app.png">

Follow the steps below to add a new bot.

1. Click on the "Third-Party Bots" App.

2. Click “+ Add Bot” in the bot table

   <img style="width:800px" src="img/botconnectordashboard/add_new_bot.png">

{:start="3"}

3. Assign agent: Create a new Bot Agent or select an existing one

4. Choose conversation type: Chat or Messaging

Settings for Chat: <br />
<img style="width:900px" src="img/botconnectordashboard/chat_settings.png">

<ul>
  <li>
  Time until warning: Set up the time span after which the consumer will get an inactivity warning.
  </li>
  <li>
  Warning message: The warning message the chat consumer gets if he reaches the threshold.
  </li>
  <li>
  Time until conversation close: Set up the time duration after which the consumer chat conversation will be closed if the customer is inactive
  </li>
  <li>
  Close message: The message which the consumer will receive prior to closing the conversation
  </li>
</ul>

Settings for Messaging: <br />
<img style="width:900px" src="img/botconnectordashboard/messaging_settings.png">

- No special settings are needed for messaging bots

Settings for Engagement Attributes: <br />

Third-Party bots allows the collection of engagement attributes ([documentation](engagement-attributes-types-of-engagement-attributes.html)) if this option is selected. These attributes are collected only at the **_start_** of every conversation. These attributes are then passed along every message as context information. Third-Party bots leverage the LivePerson Visit Information API to collect the engagement attributes, Further information Visit Information API can be found [here](visit-information-api-visit-information.html). Additionally the original Liveperson event from the chat or messaging api is sent along with each request to allow further customization and information.

##### Example lpSdes object for Chat conversations

The authenticated SDEs for Chat conversations have the same format as described [here](chat-agent-api-methods-retrieve-participant-info.html).

```js
const lpSdes = {
  unauthenticatedSdes: {
    customerInfo: {
      serverTimeStamp: 1571770965040,
      customerInfo: {
        customerStatus: 'cancelled',
        customerType: 'vip',
        balance: -400.99,
        currency: 'USD',
        customerId: '138766AC',
        lastPaymentDate: {
          year: 2014,
          month: 10,
          day: 15
        },
        registrationDate: {
          year: 2013,
          month: 5,
          day: 23
        },
        loginStatus: null,
        companyBranch: null,
        socialId: '11256324780',
        imei: '3543546543545688',
        userName: 'user000',
        companySize: 500,
        accountName: 'bank corp',
        role: 'broker',
        storeZipCode: '20505',
        storeNumber: '123865'
      },
      contexts: {
        page: {
          id: '561554049'
        }
      }
    },
    cartStatus: {
      serverTimeStamp: 1571770965040,
      total: 11.7,
      currency: 'USD',
      contexts: {
        page: {
          id: '561554049'
        }
      },
      numItems: 6,
      products: [
        {
          product: {
            name: 'prod1',
            category: 'category',
            sku: 'sku',
            price: 7.8
          },
          quantity: 1
        }
      ]
    },
    purchase: {
      serverTimeStamp: 1571770965040,
      total: 11.7,
      orderId: 'DRV1534XC',
      currency: 'USD',
      contexts: {
        page: {
          id: '561554049'
        }
      },
      cart: {
        serverTimeStamp: 0,
        total: null,
        currency: null,
        contexts: {},
        numItems: null,
        products: [
          {
            product: {
              name: 'antivirus pro plan',
              category: 'software',
              sku: 'xyz001',
              price: 7.8
            },
            quantity: 3
          }
        ]
      }
    },
    viewedProduct: {
      serverTimeStamp: 1571770965040,
      currency: 'USD',
      contexts: {
        page: {
          id: '561554049'
        }
      },
      products: [
        {
          product: {
            name: 'red high heel shoe',
            category: 'women shoes',
            sku: 'xyz567',
            price: 77.8
          }
        }
      ]
    },
    marketingCampaignInfo: {
      serverTimeStamp: 1571770965040,
      contexts: {
        page: {
          id: '561554049'
        }
      },
      marketingCampaignInfo: {
        originatingChannel: 1,
        affiliate: 'Yahoo',
        campaignId: 'US coupon campaign'
      }
    },
    personalInfo: {
      serverTimeStamp: 1571770965040,
      personalInfo: {
        name: 'John',
        surname: 'Doe',
        customerAge: {
          customerAgeInYears: 34,
          customerYearOfBirth: 1980,
          customerMonthOfBirth: 4,
          customerDateOfBirth: 15
        },
        contacts: [
          {
            email: 'myname@example.com',
            phone: '+1 212-788-8877',
            phoneType: null,
            address: null,
            preferredContactMethod: null
          }
        ],
        gender: 'MALE',
        company: 'company',
        language: 'en-US'
      },
      contexts: {
        page: {
          id: '561554049'
        }
      }
    },
    lead: {
      serverTimeStamp: 1571770965040,
      contexts: {
        page: {
          id: '561554049'
        }
      },
      lead: {
        topic: 'luxury car test drive 2015',
        value: 22.22,
        ticketId: null,
        leadId: 'xyz123',
        currency: 'USD'
      }
    },
    serviceActivity: {
      serverTimeStamp: 1571770965040,
      serviceActivity: {
        topic: 'order checkbook',
        status: 0,
        category: 'finance',
        serviceId: 'service12'
      },
      contexts: {
        page: {
          id: '561554049'
        }
      }
    },
    visitorError: {
      serverTimeStamp: 1571770965040,
      contexts: {
        page: {
          id: '561554049'
        }
      },
      visitorError: {
        contextId: null,
        message: 'Expiration date missing',
        code: 'er100004',
        level: null,
        resolved: null
      }
    }
  },
  authenticatedSdes: {
    authenticatedParticipantInfo: {
      isUserAuthenticated: true,
      participantId: '55fc1779-83b0-4e8b-8eea-503a8eaf8822',
      balance: -400.99,
      registrationDate: {
        year: 2013,
        month: 5,
        day: 23
      },
      lastPaymentDate: {
        year: 2014,
        month: 10,
        day: 15
      },
      customerId: '57ac-072a-5d10-4506-721f-9ebf',
      customerStatus: 'cancelled',
      customerType: 'vip',
      socialId: '11256324780',
      imei: '3543546543545688',
      userName: 'testuser',
      companySize: 500,
      accountName: 'bank corp',
      role: 'broker'
    },
    authenticatedPersonalInfo: {
      name: 'Test',
      surname: 'User',
      contacts: [
        {
          email: 'testuser@liveperson.com',
          phone: '+1-10-344-3765333'
        }
      ]
    }
  }
};

```

##### Example lpSdes object for Messaging conversations

```js
const lpSdes = {
  unauthenticatedSdes: {
    customerInfo: {
      serverTimeStamp: 1571770965040,
      customerInfo: {
        customerStatus: 'cancelled',
        customerType: 'vip',
        balance: -400.99,
        currency: 'USD',
        customerId: '138766AC',
        lastPaymentDate: {
          year: 2014,
          month: 10,
          day: 15
        },
        registrationDate: {
          year: 2013,
          month: 5,
          day: 23
        },
        loginStatus: null,
        companyBranch: null,
        socialId: '11256324780',
        imei: '3543546543545688',
        userName: 'user000',
        companySize: 500,
        accountName: 'bank corp',
        role: 'broker',
        storeZipCode: '20505',
        storeNumber: '123865'
      },
      contexts: {
        page: {
          id: '561554049'
        }
      }
    },
    cartStatus: {
      serverTimeStamp: 1571770965040,
      total: 11.7,
      currency: 'USD',
      contexts: {
        page: {
          id: '561554049'
        }
      },
      numItems: 6,
      products: [
        {
          product: {
            name: 'prod1',
            category: 'category',
            sku: 'sku',
            price: 7.8
          },
          quantity: 1
        }
      ]
    },
    purchase: {
      serverTimeStamp: 1571770965040,
      total: 11.7,
      orderId: 'DRV1534XC',
      currency: 'USD',
      contexts: {
        page: {
          id: '561554049'
        }
      },
      cart: {
        serverTimeStamp: 0,
        total: null,
        currency: null,
        contexts: {},
        numItems: null,
        products: [
          {
            product: {
              name: 'antivirus pro plan',
              category: 'software',
              sku: 'xyz001',
              price: 7.8
            },
            quantity: 3
          }
        ]
      }
    },
    viewedProduct: {
      serverTimeStamp: 1571770965040,
      currency: 'USD',
      contexts: {
        page: {
          id: '561554049'
        }
      },
      products: [
        {
          product: {
            name: 'red high heel shoe',
            category: 'women shoes',
            sku: 'xyz567',
            price: 77.8
          }
        }
      ]
    },
    marketingCampaignInfo: {
      serverTimeStamp: 1571770965040,
      contexts: {
        page: {
          id: '561554049'
        }
      },
      marketingCampaignInfo: {
        originatingChannel: 1,
        affiliate: 'Yahoo',
        campaignId: 'US coupon campaign'
      }
    },
    personalInfo: {
      serverTimeStamp: 1571770965040,
      personalInfo: {
        name: 'John',
        surname: 'Doe',
        customerAge: {
          customerAgeInYears: 34,
          customerYearOfBirth: 1980,
          customerMonthOfBirth: 4,
          customerDateOfBirth: 15
        },
        contacts: [
          {
            email: 'myname@example.com',
            phone: '+1 212-788-8877',
            phoneType: null,
            address: null,
            preferredContactMethod: null
          }
        ],
        gender: 'MALE',
        company: 'company',
        language: 'en-US'
      },
      contexts: {
        page: {
          id: '561554049'
        }
      }
    },
    lead: {
      serverTimeStamp: 1571770965040,
      contexts: {
        page: {
          id: '561554049'
        }
      },
      lead: {
        topic: 'luxury car test drive 2015',
        value: 22.22,
        ticketId: null,
        leadId: 'xyz123',
        currency: 'USD'
      }
    },
    serviceActivity: {
      serverTimeStamp: 1571770965040,
      serviceActivity: {
        topic: 'order checkbook',
        status: 0,
        category: 'finance',
        serviceId: 'service12'
      },
      contexts: {
        page: {
          id: '561554049'
        }
      }
    },
    visitorError: {
      serverTimeStamp: 1571770965040,
      contexts: {
        page: {
          id: '561554049'
        }
      },
      visitorError: {
        contextId: null,
        message: 'Expiration date missing',
        code: 'er100004',
        level: null,
        resolved: null
      }
    }
  },
  authenticatedSdes: {
    customerInfo: {
      type: 'ctmrinfo',
      acr: 'loa1',
      iss: 'https://customerWebSite.com',
      customerInfo: {
        customerStatus: 'cancelled',
        customerType: 'vip',
        balance: -400.99,
        currency: 'USD',
        customerId: '138766AC',
        lastPaymentDate: {
          year: 2014,
          month: 10,
          day: 15
        },
        registrationDate: {
          year: 2013,
          month: 5,
          day: 23
        },
        loginStatus: null,
        companyBranch: null,
        socialId: '11256324780',
        imei: '3543546543545688',
        userName: 'user000',
        companySize: 500,
        accountName: 'bank corp',
        role: 'broker',
        storeZipCode: '20505',
        storeNumber: '123865'
      },
      auth: {}
    },
    personalInfo: {
      type: 'personal',
      acr: 'loa1',
      iss: 'https://customerWebSite.com',
      personalInfo: {
        name: 'John',
        surname: 'Doe',
        customerAge: {
          customerAgeInYears: 34,
          customerYearOfBirth: 1980,
          customerMonthOfBirth: 4,
          customerDateOfBirth: 15
        },
        contacts: [
          {
            email: 'myname@example.com',
            phone: '+1 212-788-8877',
            phoneType: null,
            address: null,
            preferredContactMethod: null
          }
        ],
        gender: 'MALE',
        company: 'company',
        language: 'en-US'
      },
      auth: {}
    }
  }
};

```

##### Example lpEvent object of Messaging API

```js
  const lpEvent = {
    sequence: 0,
    originatorClientProperties: {
      type: '.ClientProperties',
      appId: 'webAsync',
      ipAddress: '192.168.226.81',
      deviceFamily: 'DESKTOP',
      os: 'OSX',
      osVersion: '10.14.6',
      integration: 'WEB_SDK',
      integrationVersion: '3.0.29',
      browser: 'CHROME',
      browserVersion: '72.0.3626.121',
      features: [
        'PHOTO_SHARING',
        'CO_BROWSE',
        'QUICK_REPLIES',
        'AUTO_MESSAGES',
        'MULTI_DIALOG',
        'FILE_SHARING',
        'RICH_CONTENT'
      ]
    },
    originatorId:
      '32f23bf295b1420b3a2f3f2f96c54bb2d8455699e5b6edc8add6c27b7b0b50fb',
    originatorMetadata: {
      id: '32f23bf295b1420b3a2f3f2f96c54bb2d8455699e5b6edc8add6c27b7b0b50fb',
      role: 'CONSUMER',
      clientProperties: {
        type: '.ClientProperties',
        appId: 'webAsync',
        ipAddress: '192.168.226.81',
        deviceFamily: 'DESKTOP',
        os: 'OSX',
        osVersion: '10.14.6',
        integration: 'WEB_SDK',
        integrationVersion: '3.0.29',
        browser: 'CHROME',
        browserVersion: '72.0.3626.121',
        features: [
          'PHOTO_SHARING',
          'CO_BROWSE',
          'QUICK_REPLIES',
          'AUTO_MESSAGES',
          'MULTI_DIALOG',
          'FILE_SHARING',
          'RICH_CONTENT'
        ]
      }
    },
    serverTimestamp: 1571816583661,
    event: {
      type: 'ContentEvent',
      message: 'hi',
      contentType: 'text/plain'
    },
    dialogId: '93d1b226-da30-48ca-a04b-dbe071ebd23c',
    __isMe: false,
    conversationContext: {
      skillId: '3417641010',
      campaignId: 3417641610,
      engagementId: 3417642010,
      type: 'MESSAGING',
      visitor: {
        sharkVisitorId: 'Y4NWRmZTYwZmQ1MjM3YTA2',
        sharkSessionId: 'FstLzrE6SvC5NrseUHi48g',
        ipAddress: '192.168.226.81',
        browser: 'CHROME',
        os: 'OSX',
        osVersion: '10.14.6',
        integration: 'WEB_SDK',
        integrationVersion: '3.0.29',
        browserVersion: '72.0.3626.121',
        language: 'en-US',
        features: [
          'PHOTO_SHARING',
          'CO_BROWSE',
          'QUICK_REPLIES',
          'AUTO_MESSAGES',
          'MULTI_DIALOG',
          'FILE_SHARING',
          'RICH_CONTENT'
        ],
        deviceFamily: 'DESKTOP'
      }
    },

  };
```

##### Example lpEvent object of the Chat API

```js
  const lpEvent = {
    '@id': '3',
    '@type': 'line',
    time: '2019-10-23T03:46:00.062-04:00',
    textType: 'plain',
    text: 'You are now chatting with dialogflow_v2_chat.',
    by: 'info',
    source: 'system',
    systemMessageId: '3',
    subType: 'REGULAR',
    sequenceId: '2',
    state: 'chatting',
    chatSessionKey:
      'H4127499448709883677-2ea2cecce74a45218dc3e911d25361f2K8404910',
    skillName: 'dialogflow_v2_chat',
    skillId: '3417640810',
    agentName: 'dialogflow_v2_chat',
    agentId: '3417640910',
    startTime: '2019-10-23T03:45:58.344-04:00',
    duration: '0',
    lastUpdate: '2019-10-23T03:45:59.978-04:00',
    chatTimeout: '40',
    visitorId: '2611108340074',
    agentTyping: 'not-typing',
    visitorTyping: 'not-typing',
    visitorName: 'visitor',
    rtSessionId: '4294983598',
    sharkVisitorId: 'Y4NWRmZTYwZmQ1MjM3YTA2',
    sharkSessionId: 'FstLzrE6SvC5NrseUHi48g',
    sharkContextId: '4',
    engagementId: '3417641910',
    campaignId: '3417641610',
    language: 'en-US',
    participantId: 'a2fe6e1e-ab38-47a6-b8b3-f38d573d3603',
    link: [
      {
        '@href':
          'https://XXXXX/api/account/le61911979/agentSession/593497449/chat/H4127499448709883677-2ea2cecce74a45218dc3e911d25361f2K8404910/info',
        '@rel': 'self'
      },
      {
        '@href':
          'https://XXXXX/api/account/le61911979/agentSession/593497449/chat/H4127499448709883677-2ea2cecce74a45218dc3e911d25361f2K8404910/info/visitorName',
        '@rel': 'visitor-name'
      },
      {
        '@href':
          'https://XXXXX/api/account/le61911979/agentSession/593497449/chat/H4127499448709883677-2ea2cecce74a45218dc3e911d25361f2K8404910/info/visitorTyping',
        '@rel': 'visitor-typing'
      },
      {
        '@href':
          'https://XXXXX/api/account/le61911979/agentSession/593497449/chat/H4127499448709883677-2ea2cecce74a45218dc3e911d25361f2K8404910/info/agentTyping',
        '@rel': 'agent-typing'
      },
      {
        '@href':
          'https://XXXXX/api/account/le61911979/agentSession/593497449/participantInfo/a2fe6e1e-ab38-47a6-b8b3-f38d573d3603',
        '@rel': 'participant-info'
      },
      {
        '@href':
          'https://XXXXX/api/account/le61911979/agentSession/593497449/participantExtendedInfo/a2fe6e1e-ab38-47a6-b8b3-f38d573d3603',
        '@rel': 'participant-extended-info'
      }
    ],
    isWelcomeEvent: true,
    conversationContext: {
      skillId: '3417640810',
      campaignId: '3417641610',
      engagementId: '3417641910',
      type: 'CHAT',
      visitor: {
        sharkVisitorId: 'Y4NWRmZTYwZmQ1MjM3YTA2',
        sharkSessionId: 'FstLzrE6SvC5NrseUHi48g',
        ipAddress: null,
        browser: null,
        os: null,
        osVersion: null,
        integration: null,
        integrationVersion: null,
        browserVersion: null,
        language: 'en-US',
        features: null,
        deviceFamily: null
      }
    }
  };
```

{:start="5"}

5. Setup Escalation: Skill to transfer to in the event of an error during connection to the AI service

  <img style="width:900px" src="img/botconnectordashboard/error_handling.png">

<ul>
  <li>
    Transfer message to Customer: Default escalation message to the consumer in case the bot encounters an error
  </li>
  <li>
    Transfer message to Agent: Message to the Agent from the escalating bot which will be provided together with the conversation when it is transferred
  </li>
  <li>
    Transfer failure message: Message to the customer in case the escalation to the default escalation skill did not work.
  </li>
  <li>
    Transfer to skill: Default escalation skill the bot should escalate to in case of any error.
  </li>
    <li>
    Enable error-hook: Liveperson Function which gets triggered when a bot is having an issue 
  </li>

</ul>

**Note**: if no other skills are configured, it might be that the bot will escalate the conversation to itself. However in this case only new messages will be processed.

6. Connect to A.I.: Choose an AI engine from a list. Add the configuration of AI. See [Next Steps](#next-steps).

### Next Steps

Move on to the product guides to learn how to connect and configure your specific bot framework/builder.

- [Watson Assistant](bot-connectors-ibm-watson-assistant.html)

- [Dialogflow V1 (No longer supported)](bot-connectors-google-dialogflow.html)

- [Dialogflow V2](bot-connectors-google-dialogflow-version-2.html)

- [Amazon Lex](bot-connectors-amazon-lex.html)

- [Microsoft Bot Framework](bot-connectors-microsoft-bot-framework.html)

- [Custom Third Party Bots](bot-connectors-custom-third-party-bots.html)
