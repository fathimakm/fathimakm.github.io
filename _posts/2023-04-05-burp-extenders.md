---
date: 2023-04-05
title: Introduction to Burp Extenders
categories:
  - Burp
featured_image: https://uploads-ssl.webflow.com/62efedb360a7998b0e43cb84/6321a0f076706854ff591093_All%20about%20BurpSuite.jpg
---
# Introduction to Burp Extenders

Burp Suite is a popular web application security testing tool used by security professionals. It provides various features like crawling, scanning, intruder, repeater etc. for testing web apps. 

Burp Extender allows us to extend the functionality of Burp Suite by developing plugins using Java/Python. We can create custom plugins for tasks like:

- Processing HTTP requests/responses
- Custom scanning issues
- Custom Intruder payloads
- Automating repetitive tasks

## Creating a Simple Burp Extender

To create a Burp Extender plugin, we need to:

- Set up the project structure
- Implement required interfaces like `IBurpExtender`
- Build the project into a JAR file
- Load the plugin JAR in Burp Suite

Here is a sample code to create a simple plugin that logs all HTTP requests sent by Burp Scanner:

```
java
package BurpExtender;

import burp.*;
import java.io.PrintWriter; 

public class BurpExtender implements IBurpExtender, IHttpRequestResponse {

  private IBurpExtenderCallbacks callbacks;
  private PrintWriter stdout;

  @Override
  public void registerExtenderCallbacks(IBurpExtenderCallbacks callbacks) {
    this.callbacks = callbacks;
    stdout = new PrintWriter(callbacks.getStdout(), true);

    // set our extension name
    callbacks.setExtensionName("Request Logger");
    stdout.println("Request Logger extension loaded");

    // register as HTTP listener
    callbacks.registerHttpListener(this);
  }

  @Override
  public void processHttpMessage(int toolFlag, boolean messageIsRequest, IHttpRequestResponse messageInfo) {
    
    // check if request from scanner
    if (toolFlag == IBurpExtenderCallbacks.TOOL_SCANNER) {
    
      if (messageIsRequest) {
        // log request details
        stdout.println("Got request from scanner:");
        stdout.println(messageInfo.getHttpService());
        stdout.println(new String(messageInfo.getRequest()));
      }
    }
  }
}
```

This covers the basics of creating a simple Burp Extender plugin. We can further extend it by processing requests/responses, sending data to external systems, building UIs etc. Burp Extender provides a powerful way to customize Burp Suite for advanced use cases.

