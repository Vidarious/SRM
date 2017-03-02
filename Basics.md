# SharePoint Reference Material 

A SharePoint [C# / ASP.NET / JavaScript] reference guide for developers.

---

**TABLE OF CONTENTS**
 
* [App vs. Solution vs. Web Part vs. Feature](#AppSolutionWebPartFeature) 
* [To Dispose or Not?](#DisposeOrNot) 
 
### <a name="AppSolutionWebPartFeature"></a>App vs. Solution vs. Web Part vs. Feature

These terms are thrown around a lot when talking about SharePoint and it can be confusing at first. For example, a Feature can enable or disable a Solution which can have one or more Web Part's. However, a Feature does not have to include a Solution.

**Feature**: A feature is simply package or container that can be activated or deactived in SharePoint. What is included into the package is up to the developer but may include: Web Parts, Lists/Libraries, Site Columns, Content Types, Event Received, etc. Features can be deployed at various levels: Farm, Web Application, Site Collection or Site. When a feature is enabled, the corresponding components (web parts, lists, site columns, etc.) are added to the Farm/Web App/Site Collection or Site.

Solutions or Apps don't actually have to have a feature associated to them. They can be deployed to the SharePoint server (Farm, Site Collection, Site or Web Application) as is and will be automatically activated. However tieing a feature to your Solution or App is a good idea so you may easily turn it on or off without having to uninstall it completely.

**Web Part**: Web Parts in SharePoint are everywhere. Apps & Solutions can both include Web Parts (if the developer wants). A App or Solution may even be entirely created to be a Web Part. This is just a component or piece of what a App or Solution can be.

**App vs. Solution**: This is the heart of the discussion. Up until SharePoint 2013 Microsoft's solution to creating little mini applications in SharePoint were called "Solutions". One type of Solution was called a "Sandbox Solution". These were special because they operated in their own little walled off sandbox, away from the SharePoint environment. This was a safe way to allowing a developer to create mini applications for SharePoint while protecting the SharePoint environment. Microsoft took the idea of a Sandbox Solution and evolved it into what is now called a "App" or "App Model". Both still exist but the Sandbox solution is basically deprecated in 2013 and it is recommended to create a App in place of a Sandbox Solution.

There is however one major downfall to the App Model vs. Sandbox Solution. That is that App's are unable to connect/communicate to other App's. Whereas Sandbox Solutions _can_ connect to other Sandbox Solutions (think: linking multiple Web Parts together like Search/Results/Filter).

Solutions are typically created to extend the capabilities of a SharePoint farm, site collection or site. Whereas Apps act as standalone applications that solve end-user or business needs. 

Apps Cannot:
+ Call SharePoint server side code.
+ Access SharePoint components that are not on the same site.
+ Communicate with each other.
+ Create custom site definitions or themes.
+ Create action groups and custom action hiding.
+ Create user controls or delegate controls.

Solutions come in two flavours:
+ **Farm Solutions**: Which are fully trusted and have access to the full server-side API. These can only be deployed by the Farm Admin and can run full trust code.

+ **Sandbox Solutions**: [_DEPRECATED_] Which are partially trusted and only have access to a sub-set of service-side API. For example, the SPWeb.Properties() method is not accessable in a Sandbox Solution. In 2013 it is recommended to build an App over a Sandbox Solution. 

App's come in 3 flavours:
+ **SharePoint-Hosted Apps**: Which leverage the SharePoint server and resources vs. external dependencies. Code developed in this type of App can utilize the CSOM (Client-Side Object Model) and/or REST API models. These types of App's typically utilize SharePoint libraries & lists to store content. These app's create their own "App" site. This is its own little slice of space.

+ **Provider-Hosted Apps**: Which utilize external web servers / cloud services.

+ **Azure Auto-Hosted Apps**: No longer exists. Microsoft cancelled this in 2014.

**What to choose**: If at all possible always choose the App Model for a project unless you need functionality available in a Farm Solution. This is because a App runs entirely on the client computer and free's up resources from the SharePoint server. Also, as App's run off of the client machine and are independant it means they can be run on any compatible platform. Migrating from SharePoint 2013 -> 2016 is no issue with Apps. However if you have Farm Solutions you will need to make modifications.

---

### <a name="DisposeOrNot"></a>To Dispose or Not? 

Programming for SharePoint in C# comes with garbage collection. This means that you technically do not need to delete/unset/dispose of objects after you are finished with them. However, it is still _strongly_ recommended to do so.

There are two methods of cleaning up after you've used an object:

1. Manual Disposing

Using the **Dispose()** method allows you to remove a created object.

**REFERENCE**: https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx

```cs
//Create a new SPSite object and call it "oSPSite".
oSPSite = new SPSite("http://server");

//Remove the created object from memory.
oSPSite.Dispose();
```

2. Automatic Disposing

Using the **using(){ .. }** clause allows you to create the object, then, after the using clause if finished, the object is removed from memory for you without the need for a dispose statement.

You can continue to use **using() { .. }** within other **using() { .. }** statements. The **using** statement is similar to a try {} / catch {} statement.

**REFERENCE**: https://msdn.microsoft.com/en-us/library/yh598w02.aspx

```cs
using(SPSite oSPsite = new SPSite("http://server"))
{
  using(SPWeb oSPWeb = oSPSite.OpenWeb())
   {
       str = oSPWeb.Title;
       str = oSPWeb.Url;
   }
}
```

---
