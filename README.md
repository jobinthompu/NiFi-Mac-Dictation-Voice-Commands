# NiFi+ Mac-Dictation With Voice Commands

## Short Description:

Here is a small demo how NiFi can let you retrieve Stock quotes on voice commands and respond back with the help of Mac Dictation.

## Introduction
- Here is a small demo how NiFi can let you retrieve Stock quotes on voice commands and respond back with the help of Mac Dictation.

- Here you can view the screen recording session that Demonstrates how it works!


[![NiFi + Mac Dictation Demo](https://github.com/jobinthompu/NiFi-Mac-Dictation-Voice-Commands/blob/master/Resources/images/youTube.jpg)](https://youtu.be/tQEoCARfPso "Voice Command with Mac Dictations and NiFi - Click to Watch!")


## Prerequisite

- Make sure you have your HDP cluster/Sandbox up and running.
- Make sure you have Mac dictation feature, and its working!
- NiFi_0.6.1/HDF_1.2 is available up and running. Else execute below after ssh connectivity to sandbox is established:

## Steps:

1) Use GetHttp processor to Retrieve any Stock price from “Google Finance”, you can use the below URL to fetch YAHOO stock price:
```
http://finance.google.com/finance/info?client=ig&q=YHOO
```
- Processor Properties would look like:

![alt tag](https://github.com/jobinthompu/NiFi-Mac-Dictation-Voice-Commands/blob/master/Resources/images/Get-Http-Yarn-Rest-API.jpg)

2) Use ReplaceText and EvaluateJsonPath processors to trim json result and extract stock value and variation in stock price today. Use UpdateAttribute processor to save values to attributes.

3) Now you can use a RouteOnAttribute to decide when to trigger mail or/and give voice feedback.

4) Finally Use PutEmail processor for email Alert and/or Use ExecuteStreamCommand for voice feedback using Mac OS X’s “say” command, values can be extracted from attributes.

I am Attaching my flow template, which will help you understand better.

[retrieving-real-time-quotes.xml](https://github.com/jobinthompu/NiFi-Mac-Dictation-Voice-Commands/blob/master/Resources/flow/retrieving-real-time-quotes.xml)

5) you may dry run the flow to see if it works as expected!

6) Now we have to link Mac Dictation to communicate with NiFi. For that we have to create custom commands in Dictation that fire NiFi API calls to make changes in the GetHttp processor based on the stock requested via voice command. [Now you would be thinking about better Speech recognition software ;) ]

A Sample API call to update processor based on command would look like [update it with your processor-ids and client-ids]:

```
curl -i -X PUT -H 'Content-Type: application/json' -d '{"revision":{"version":176,"clientId":"93a9b515-1643-4ccf-8db1-71ef38238ae5"},"processor":{"id":"7f033b6f-e13e-4295-85de-33444aae6c4b","parentGroupId":"02888a59-ee2b-483e-8860-b0fdf948de97","config":{"properties":{"URL":"http://finance.google.com/finance/info?client=ig&q=AAPL","Filename":"APPLE"}}}}' http://localhost:8080/nifi-api/controller/process-groups/02888a59-ee2b-483e-8860-b0fdf948de97/processors/7f033b6f-e13e-4295-85de-33444aae6c4b
```

7) Now you can launch Dictation and try out!!

Thanks,

Jobin George