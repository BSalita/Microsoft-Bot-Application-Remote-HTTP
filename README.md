# Microsoft-Bot-Application-Remote-HTTP
This is an example Microsoft Bot application, a simple echo app, that uses HTTP (not HTTPS) to communicate with a remote server (e.g. www.example.com). This example is intended for use as a proof of concept only. Warning: It uses HTTP and is therefore completely insecure. 

This document assumes that you have installed the requisites for a Microsoft Bot application and have a general understanding of how they are work, and how they are registered. See https://dev.botframework.com/ for more info.

The project was created in Visual Studio 2015 Community Edition using New->Project->Bot Application. The following changes were made to the newly created project to enable HTTP over the internet.

1. In MessageController.cs, commented out [BotAuthentication] attribute. This change disables Basic Authentication. HTTP causes AppID and AppSecret to not be sent to the server. Disabling Basic Authentication is required to avoid HTTP authentication errors.

2. In .vs/applicationhost.config, an XML file, added a second binding (first being local host) which contains my system's hostname. This enables access to this app from the internet. Change www.example.com to be your systems ip address/name. I developed this app on a computer on my home network. To make my development notebook into a internet server, I had to punch a hole in the internet router and forward port 3978 to my server's IP address (port 3978). I used a dynamic dns address of my internet router, worked fine.

  ```xml
  <binding protocol="http" bindingInformation="*:3978:www.example.com" />
  ```
  
3. I see that .vs/applicationhost.config contains some hard coded paths my dev system. You may need to change them to correctly compile.

4. Note that Web.config has not been changed. If we were using HTTPS, YourAppId and YourAppSecret would have to be updated by the documented procedure. Since HTTP is being used, and [BotAuthentication] is disabled, these values are ignored.

  ```xml
    <add key="AppId" value="YourAppId" />
    <add key="AppSecret" value="YourAppSecret" />
  ```

Steps to execute:

1. Create a Windows Firewall rule that opens port 3978 for input.

2. Open this repos in Visual Studio 2015 as Administrator. Administrator is required for remote access, although not for localhost.

3. Execute. You should see IIS Express appear in the status bar. Your default browser will be invoked using http://localhost:3978.

4. Use Microsoft Bot Framework Emulator to communicate with the app. You should see echoing of anything that you've entered. Enter the following, use default values for other boxes.

  ```
  Url: http://www.example.com:3978/api/messages
  App Id: YourAppId
  App Secret: YourAppSecret
  ```
