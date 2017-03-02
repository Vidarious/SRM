# SharePoint Reference Material 
 
---

A SharePoint [C# / ASP.NET / JavaScript] reference guide for developers.

---

**TABLE OF CONTENTS**
 
* [App vs. Solution vs. Web Part vs. Feature](#AppSolutionWebPartFeature) 
* [To Dispose or Not?](#DisposeOrNot) 
 
#### <a name="AppSolutionWebPartFeature"></a>App vs. Solution vs. Web Part vs. Feature

Solutions come in two flavours:
+ **Farm Solutions**: Which are fully trusted and have access to the full server-side API. These can only be deployed by the Farm Admin.

+ **Sandbox Solutions**: Which are partially trusted and only have access to a sub-set of service-side API. For example, the SPWeb.Properties() method is not accessable in a Sandbox Solution. These can be deployed via a Site Collection Admin.

App's come in 3 flavours:
+ **SharePoint-Hosted Apps**: Which leverage the SharePoint server and resources vs. external dependencies. Code developed in this type of App can utilize the CSOM (Client-Side Object Model) and/or REST API models. These types of App's typically utilize SharePoint libraries & lists to store content.

+ **Provider-Hosted Apps**: Which utilize external web servers / cloud services.

+ **Azure Auto-Hosted Apps**: No longer exists. Microsoft cancelled this in 2014.

---

#### <a name="DisposeOrNot"></a>To Dispose or Not? 

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
