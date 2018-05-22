# Azure App Service
How to create and deploy a Java webapp on Azure

The general steps are:
1. Create the Azure App Service
2. Configure the Azure App Service for Java and Jetty
3. Code your Webapp
4. (Re)Deploy your app 

## Create the Azure App Service
1. Log in to the Azure Portal, ensure you use your college email
2. Create a resource
3. Search for and select Web App
    1. Give it a unique name 
    2. Ensure subscription is **Azure for Students**
    3. Create a new resource group (use a similar name so you know they are related)
    4. Select **Windows** (not because I work in Microsoft, but because you want to use Java/Jetty, and the Linux and Docker containers won't let you configure that as easily!)
    5. Select a new App Service Plan (give it a name, select **North Europe** as the location and Pricing tier of **F1 Free**)
    6. Leave **Application Insights** *off*
    7. Pin to Dashboard and **Create**

## Configure the Azure App Service for Java and Jetty
1. In your app dashboard, go to **Settings** - **Application Settings**
    1. Java version - **Java 8** 
    2. Java minor version - **Newest** 
    3. Java web container - **Newest Jetty 9.3** 

## Code your Webapp
1. Create your Webapp as normal, in this case we'll use a generic Vaadin application quickstart webapp
2. Maven new project: **mvn archetype:generate -DarchetypeArtifactId="vaadin-archetype-application" -DarchetypeGroupId="com.vaadin"**
3. 'groupId': **ie.examples**
4. 'artifactId': **VaadinGenericApp**
5. Build your app with Maven Install, test it with Jetty on localhost
6. Find the final **target/appname.war** file and rename it to **ROOT.war** (we could automate that but no harm in learning what's happening behind the scenes)
7. On the command line, this works:
```
cd .\target\
ren .\VaadinGenericApp-1.0-SNAPSHOT.war ROOT.war
```
8. (On Windows) If you want to open a File Explorer here, type **explorer .** The full stop is important, it means 'this folder'! 

## (Re)Deploy your app 
There are a few different ways to deploy, but a simple way is to **FTP** (File Transfer Protocol) your app to the webapps folder on your App Service.
First, we need to tell your app that you might be FTP'ing to it
1. Open the app in the Azure portal
2. In **Deployment** - **Deployment Credentials** add an FTP username and password (save and remember them)
3. In Overview, copy **FTP hostname** 

If you want to use a dedicated FTP client you can, if you have a recent version of Windows, it's built into Windows File Explorer:
1. **Windows Key + E** opens File Explorer
2.  Paste **FTP hostname** into the address bar (in my case it was **ftp://waws-prod-db3-095.ftp.azurewebsites.windows.net**)
3.  It should prompt you for the FTP username and password you set above
4.  Navigate to **site - wwwroot - webapps** and drop/paste/copy **ROOT.war** into that folder (you renamed your .war file in the previous section)
5.  Wait for it to finish copying and close the window

Now you should be able to visit the app name (something.azurewebsites.net) and see your app running. If you don't immediately, give it a few seconds to reload...
