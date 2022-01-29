# Chatbot Architecture

These days Chatbots are on the rise and the specific reason behind it is that industries are trying to reduce the redundant tasks. We all don’t want to revisit the same task/process by going through the same set of procedure again and again. Hence, the next level of interfacing is introduced, called conversational bots.

The chatbot is not a new concept, the first chatbot ever was developed in the 1960’s, but nowadays we have chatbots over multiple platforms over the internet. These chatbots are configured to achieve some goal or to have a healthy conversation for entertainment. Thanks to IOT devices, we now have these chatbots working independently on devices in restaurants, banks, shopping centers etc. All this just to reduce the redundant and monotonous tasks like taking orders for restaurants or booking a flight or executing a particular job. On the other hand, these chatbots have proven to have increased the user engagement of the website, because it is more interactive to talk to a chatbot rather than clicking on buttons.

When you wish to develop a chatbot, based the usability and context of business operations the architecture involved in building a chatbot changes dramatically. We cannot write a string matching and conditional operations for every kind of chatbot. Hence, Chatbot architecture is the heart of chatbot development.

Based on its use case there are two major types of chatbots:

1. Chatbot for Entertainment
2. Chatbot for Business

In this article, I will brief you about different popular architectures involved in chatbot development. Based on the use case, you can adopt to any of the technique you like.

## **What to expect from final Chatbot Product?** <a href="#cf11" id="cf11"></a>

Chatbot responses to user messages should be appropriate enough to continue the conversation. It is sometime not necessary for chatbot to understand and remember all the details of the sentence; based on requirements we need to alter different parts / elements and use the other; but the basic communication flow remains the same. However, choosing the correct architecture depends on the type of domain of the chatbot.

Sometimes in entertainment chatbots, the architecture varies, where you either use the pattern matcher for user input or you save the conversation history to get the insight and decide the type of response. The target in entertainment bots is to increase the average time spent by user conversing with bot. If the time is less, which implies that the conversation is shorter; hence chatbot is boring.

Chatbot for business are often the most popular and serves transactional requests, i.e. they are made for a specific purpose or to achieve a particular goal. It should be the utmost priory of the chatbot developer to have the goal fixed and defined beforehand. By defined I mean, one should have the number of requirement fixed before actually developing a chatbot. For example, travel chatbot is providing an information about flights, hotels, and tours and helps to find the best package according to user’s criteria and can do bookings accordingly. Also, Google Assistant readily provides information requested by the user and Uber bot takes a ride request.

Conversation for business chatbots are typically short, less than 15mins. Each conversation has a specific goal and the quality of chatbot can be accessed by how many users get to the goal like following:

1. Has the user found the information he/she was looking for?
2. Has the user successfully booked a flight and a hotel?
3. Has the user bought products which help to solve the problem at hand?

Perhaps some of the chatbots doesn’t fit into this classification, but it should be good enough to work for the majority of bots which are developed for now. Hence for better understanding of the models I will explain the simplest first and then increase the complexity. Remember, there is no right approach for a chatbot architecture, you can use any of the approach which favors your use case. Also, it is not wise to use a complex approach for a simpler use case. As you won’t use a snipper to kill an ant, it will just make your life difficult.

PS: No offence to Ant Lovers. :p![](https://miro.medium.com/max/60/1\*CHge3eDLX9zYIDnOZhpWBA.jpeg?q=20)![](https://miro.medium.com/max/800/1\*CHge3eDLX9zYIDnOZhpWBA.jpeg)[https://tinyurl.com/tkr5l7y](https://tinyurl.com/tkr5l7y)

## **Intent — Context** <a href="#d42f" id="d42f"></a>

While developing a chatbot, we first design the capabilities or request the chatbot can handle. As I have mentioned earlier, always design the capability classes first, these classes are nothing but the intents to which the chatbot can answer. In other words intent is the class of operations or requests which can be handled by the chatbot to give response. An intent is usually created by defining a class of request and putting in the sentences associated with it.

For example if we are creating a chatbot that have a capability to set an alarm. Then an intent class is defined as ALARM\_SET and user can express this request in hundreds of ways like “Set an alarm for 10AM” or “Wake me up at 10 in the morning” or “Remind me when its 10AM” etc. Hence whenever a user ask a query falling under this category then the intent is ALARM\_SET and chatbot generate response accordingly.

Similarly, Context is the real world entity around which conversation is happening. In other words, the intent request needs an entity to process and generate response. Like in the above example “10AM” is the context on which we have to set the alarm. Now imagine we are creating a chatbot for knowledge transfer on the behalf of an Insurance Company. Consider one of the intent class is ABOUT, so whenever a user can ask “Tell me about _retirement plan_” or “What is a _retirement plan_” etc., so here “retirement plan” is context. Hence in this way most of the chatbots are designed. Remember these terminology by heart.

Now I will discuss about different architectures involved in chatbot development.

## **Pattern-based Approach** <a href="#bf48" id="bf48"></a>

![](https://miro.medium.com/max/60/1\*aP3Fx0tx2Y-VCbOOKyR8Zg.jpeg?q=20)![](https://miro.medium.com/max/365/1\*aP3Fx0tx2Y-VCbOOKyR8Zg.jpeg)[https://tinyurl.com/sssatyz](https://tinyurl.com/sssatyz)

This is the simplest way to create a chatbot and is also known as rule-based chatbot. You can create both entertainment and business chatbot with this approach. The technique involved in it is simple, you just need to specify the possible input question that the user will ask and its associated response. Refer to the diagram below:

> **\<context>**
>
> **\<pattern>What is your NAME\</pattern>**
>
> **\<template>My name is $(‘QUERY\_RESULT’)\</template>**
>
> **\</context>**

When a chatbot receives the message, it goes through all the user defined patterns until finds the pattern which matches user messages. If match is found, the chatbot uses the correct response template to generate the response. In other words, developer is defining a set of rules or pattern as conditions to give response to user.

Heuristics for selecting the right response can be engineered in many different ways, sometimes a general if-else conditions is implied whereas sometimes a machine learning algorithm is trained on these input pattern. This approach is very popular for entertainment bots. AIML is the widely used language for writing pattern and responses.

[**ChatScript** ](https://github.com/ChatScript/ChatScript)is the famous open source library used to implement the rule based language. It so popular and widely used because it has some additional natural language processing pipeline which includes part of speech tagging, synonyms finder etc., which gives additional accuracy to matching the patterns with user input. Although, it does not use any machine learning algorithms or call any 3rd party API’s unless you program it to do so.

## **Intent Classification Architecture** <a href="#9207" id="9207"></a>

The challenge with the pattern-based or rule based approach is that, the patterns should be coded manually, and it is not an easy task. Imagine, if we try to increase the capability of the chatbot, then we need to hardcode every condition the chatbot can answer. This is extremely difficult to maintain and can cause a lot of overlapping confusion between the patterns. This can possibly reduce the accuracy of the chatbot. Also as mentioned earlier single question can be asked in multiple ways. Therefore, it is not easy for a human to define and find pattern by natural language understanding, whereas computers can do this easily.

In other words, for narrow domains a pattern matching architecture would be the ideal choice. However, for chatbots that deal with multiple domains or multiple services, broader domains, sophisticated, state-of-the-art neural network architectures, such as Long Short-Term Memory (LSTMs) and reinforcement learning agents are the best options.

Machine learning can be applied on intent classification algorithm to classify and find patterns in the natural language, thanks to word embedding. You just need to provide training set of a few hundreds or thousands of examples, and it will pick up patterns in data and classify the intent accurately and in fairly less amount of time.

Such machine learning algorithm can be built using any popular machine learning library like Sci-kit learn, Tensorflow or PyTorch. Another option is to use one of cloud API: wit.ai, api.ai, Microsoft LUIS.![](https://miro.medium.com/max/60/1\*UIZJ2vJq7kX214iU11-Ezw.jpeg?q=20)![](https://miro.medium.com/max/662/1\*UIZJ2vJq7kX214iU11-Ezw.jpeg)[https://tinyurl.com/yx894jqb](https://tinyurl.com/yx894jqb)

## **Response Generation** <a href="#e128" id="e128"></a>

Pattern matching, intent classification and context extraction helps to understand what user message means. Whenever the chatbot gets the intent and the context of message, it shall generate a response. But now the question arises how? You can approach it differently based on the type of chatbot you are building.

Simplest way is to response a static message by placing some of the values variable, based on the entity and processing that you undergo. This processing can be a Query to a database or calling some 3rd party API. ChatScript provide a very handy solution to these kind of problems. For Example:

> **Q:** What is the age of Obama?
>
> **A:** Age of $(‘Entity’) is $(‘Query result’)

Apart from this, different kind of chatbots offer different processing and response mechanism. For example, a medical chatbot will have a predefined set of Symptoms Model for different diseases and for generating the response it will apply statistical modelling and probabilistic approach to generate similarity with symptoms model to predict the disease based on the user input to chatbot questions and gives response accordingly.

A question answering chatbot will dig into the knowledge graph or a database to query the request and generate the best answer score to give the correct response. On the other hand, a weather based chatbot will call a 3rd party API’s to get the right data and place it into fixed messages to give the response.

This concept of generating the responses based on user message is also called Response modelling. There are two major types of modelling:

1\) Generative Modelling

2\) Retrieval based Modelling

This post mostly covers the Retrieval based models. They are much easier to build and provide more predictable results. The chatbot uses the intent and context of conversation for selecting the best response from a predefined list of bot messages. You probably won’t get 100% accuracy of responses, but at least you know all possible responses and can make sure that there are no inappropriate or grammatically incorrect responses. Retrieval-based models are more practical at the moment. Also many algorithms and APIs are readily available for developers.![](https://miro.medium.com/max/60/1\*fZhoWEKcWrGTSyYWZCAWxQ.png?q=20)![](https://miro.medium.com/max/670/1\*fZhoWEKcWrGTSyYWZCAWxQ.png)

Generative Response Model is the future of chatbots where the output not only depends on the current input, but to a series of input given in the past. I will write a separate post on Generative response model, but for now it is out of the scope of this post. However, 90% of the chatbot in the market today is build using Retrieval based modelling because of its huge capability and ability to solve maximum problem and making life easy for people
