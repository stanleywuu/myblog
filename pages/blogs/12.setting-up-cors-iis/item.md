---
title: PSA: Install the CORS module if you are using IIS
date: 05/20/2021
---
So CORS is a mechanism used to enforce security of your web application, if you work on a server client application, and haven't needed to know about it yet, you probably will at some point.

Here is a pretty good overview on what it is.
[3 Ways to Fix the CORS Error — and How the Access-Control-Allow-Origin Header Works](https://medium.com/@dtkatz/3-ways-to-fix-the-cors-error-and-how-access-control-allow-origin-works-d97d55946d9)


Even if you aren't the person managing the servers, the following scenario is bound to happen once or twice:

1. You build a server local, you test your site against your local server, it works perfectly.
2. Then you host the server on cloud and start fixing up your web client, then BOOM!!
3. Your requests can't be made, you start seeing errors such as this on your developer console
```
Access to fetch at 'https://{your-site} from origin 'https://localhost' has been blocked by CQRS:
```
And followed by a variety of reasons.

And you might react this way:
* Jeez, I don't want to manage all this complicated IT stuff, I just want to test my work! It's me!! It's safe!! Just let me do it!
I'm just going to go turn it off.

First, let me make another obligatory announcement:
## DO NOT DISABLE CORS ON A PRODUCTION ENVIRONMENT
If you can reach your cloud server from your basement, an attacker can reach your server the same way so we have to be careful about that.

The scenario here is only when you need to do something quick, and safe, and in a **dev** environment. This is likely the reason why you would encounter CORS in the first place.

Now if you host your site on Azure, it's easy, you go to policies, CORS and add * as your origin, you can follow a guide such as [this](https://github.com/uglide/azure-content/blob/master/articles/app-service-api/app-service-api-cors-consume-javascript.md)

- If you use IIS
- If you maintain a web application and a server application for it
- You want to update your CORS settings

Now if you are using **IIS**, you might have come across many stack overflow posts that tells you to add such and such configuration to web.config, but I am here to tell you the most important thing.
For example, you might have gone into the config file and added things like
```
<system.webServer>
    <httpProtocol>
        <customHeaders>
            <add name="Access-Control-Allow-Origin" value="*" />
            <add name="Access-Control-Allow-Methods" value="GET,POST,OPTIONS" />
            <add name="Access-Control-Allow-Headers" value="Content-Type, soapaction" />
        </customHeaders>
    </httpProtocol>
</system.webServer>
```

And then you might have found out that it didn't do anything.
**Really, this looks like something that would work, though?**

## You are right, you are just missing one more thing.
## MAKE SURE YOU INSTALL THE CORS Module Extension for IIS
[Download the extension here](https://www.iis.net/downloads/microsoft/iis-cors-module)
Otherwise, whatever setting you copy from Stack overflow **will not do anything**

```
Once installed, the IIS CORS module is configured via a site or application web.config and has its own **cors** configuration section within system.webserver.
```

That is all, in response to a wasted day. 
Where I thought I ran out of options, but it was only because our servers did not have the CORS extension installed.

Hope this saves someone's day.
