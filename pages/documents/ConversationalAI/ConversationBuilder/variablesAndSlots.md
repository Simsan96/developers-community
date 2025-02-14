---
pagename: Variables & Slots
redirect_from:
    - conversation-builder-conversation-builder-variables-slots.html
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Conversation Builder
subfoldername: Conversation Builder
permalink: conversation-builder-variables-slots.html
indicator: both
---

In the Interaction Details [Response Match & Actions](conversation-builder-conversation-builder-response-match-actions.html) settings, you can assign various data points to either Slots or Variables. Both are useful in [API Integrations](conversation-builder-integrations-api-integrations.html).

### Displaying data to the user

[See here](conversation-builder-interactions-interaction-basics.html#display-variables-in-interactions) for how to display variables and slots in interactions.

### Variables

Variables are the default way to store and access important data points throughout the flow of a bot. 

#### System variables

There are several system variables that store information that's commonly needed in use cases. You can use these variables in your interactions:

* `{$chatBotId}` - Returns the ID of the bot.
* `{$chatBotUserId}` - Returns the ID of the user (the consumer).
* `{$chatBotUserPlatformId}` - Returns the ID of the bot user agent. This is provided by LiveEngage.
* `{$conversationId}` - Returns the ID of the current conversation. This is provided by LiveEngage.
* `{$firstname}` - Returns the first name of the bot user agent. This is provided by LiveEngage.
* `{$quickReplyPayload}` - Returns the quick reply payload for the current interaction.
* `{$userMessage}` - Returns the current user message.

#### Storing user responses

The most common use case for variables is storing user responses to [questions](conversation-builder-interactions-questions.html).

Frequently you will want to capture what was just said by the user as the value of a variable. You can use `{$userMessage}` to do this, for example:

<img class="fancyimage" width="350" src="img/ConvoBuilder/bestPractices/tips_image_7.png">

You can also use `{$query}` in the same way; it works like `{$userMessage}`.

#### Storing and accessing variables with code

The [Get and Set Bot Variables functions](conversation-builder-scripting-functions-get-set-contextual-data.html#get-and-set-bot-variable) can be used to store and access variables in the [Pre-Process / Post-Process Code](conversation-builder-conversation-builder-interaction-details.html#code) or the [Process User Response](conversation-builder-conversation-builder-interaction-details.html#process-user-response) JavaScript code panels.

### Slots

Slots are a *special type* of variable. Most of the time, you will use [variables](#variables) to take what a user says and hold on to it for later use. Slots are useful for more specialized use cases.

#### Slots & entities

When combined with [entities](conversation-builder-intent-builder-entities.html), slots bring dynamic, fluid behavior to storing user input data.

For example, if you add a question interaction "what type of shoes are you looking for?", the [NLU Assist](conversation-builder-nlu-assist.html#assigning-an-entity-and-slots-to-an-interaction) tool will suggest appropriate entities and slots for that interaction. As long as the user stays within the bounds of entities that you have created, slots will automatically adjust and update based upon user input throughout the conversation.

#### Adding a slot

To configure a slot, simply click the interaction where you'd like to look for entities in the user's input (like a multiple choice question, for example), and go to User Response in the middle of the Interaction Details panel. From there, click the blue **+** icon right to the right of Slot.

In the menu which opens, first assign the slot a name by filling in the "Name" input field. **We recommend using standard naming conventions for slots. The slot name is later used to refer to and access the data which the slot contains**. Then, look for a pre-configured entity (which you should have set up for your domain previously) by typing in first the "@" character and then the name of your desired entity in the "Value" input field to the right of the Name input field.

Lastly, decide how long you'd like the slot's data to be kept for. You can set this by using the "Scope" dropdown menu on the right hand side. The dropdown provides four options:

* Request - the slot's data will only be saved for that particular use of the slot. Only useful if the next question in the tree depends on the slot's data.

* Dialog - the slot's data will only be stored for this specific dialog. Once this dialog ends (either by the user closing the conversation or the automation moving on to a different dialog), the slot's data will be cleared.

* Session - the slot's data will be saved for the entirety of the user's browser session. This is useful when using the data to query APIs and retrieve information which might be useful for multiple dialogs.

* Forever - the slot's data will be saved on our servers forever. It will be accessible via the Conversation Builder for as long as you need it.

`{$botContext.slot.slotName}` is how you can access values in slots and use them in other ways. For example, to have the automation respond with a user's previously stored answer under the assigned entity `animal`, you'd set up a text interaction like so:

"You answered: {$botContext.slot.animal}!"

If your automation asked the user "which animal do you like?" and the user answered "dogs" or something similar, the slot for the entity `animal` would be populated with their answer. The automation would then respond with "You answered: dogs!" populating the code above with the user's reply.

### When to use variables vs slots

Variables are the default storage unit of Conversation Builder, while slots are a special type of variable. The only reason to favor Slots is if you need extra functionality that is linked to entities or if entities will be used in an API Integration catalog search, for example.

When in doubt, it is best to use variables. The [NLU Assist](conversation-builder-nlu-assist.html) tool will help to display when slots are most useful.