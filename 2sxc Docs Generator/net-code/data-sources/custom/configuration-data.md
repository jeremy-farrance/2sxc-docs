---
uid: NetCode.DataSources.Custom.ConfigurationData
---

# DataSource Configuration Data

[!include[](~/basics/stack/_shared-float-summary.md)]
<style>
  .context-box-summary .datasource-custom, 
  .context-box-summary .query-params,
  .context-box-summary .data-configuration
  { visibility: visible; } 
</style>

Most Custom DataSources will want to provide a configuration UI to the editor. 

To enable this we create a Content-Type for each DataSource which will contain the fields to configure it. 

## Example

Let's say we need a `Weather` DataSource which must be configured. We would create a DataSource with these fields:

1. `CountryCode` - text field with a dropdown of country codes
1. `ZipCode` - text field to enter the ZIP code
1. `ApiKey` - a key which will let our backend use the WeatherService

Once this Content-Type is defined, we automatically have an editing-UI (thanks to the CMS features) but it's not yet connected to the DataSource. 

## Basic Integration: Single-App Use

If you don't plan on distributing your DataSource and only need it in one or two Apps, then you can just create the Content-Type there and export/import it to other apps. 

You'll need to register the [internal name](xref:Basics.Data.ContentTypes.Names) in the [VisualQuery Attribute](xref:NetCode.DataSources.Custom.VisualQueryAttribute) of your DataSource so that the UI will show the configuration where it's needed. 

Once you've done this, the [Visual Query Designer](xref:Basics.Query.VisualQuery.Index) will know about it and offer your content-type when configuring that part of a Query. 

> [!TIP]
> The data entered will be made available to your DataSource Configuration as the [Settings](xref:Basics.LookUp.Settings).

## Advanced Integration: Multi-App Use

If you plan on using this configuration in many Apps on a server, then export/import of this Content-Type is not practical. 

In this case you'll want to export the Content-Type as a JSON and place it in the `.data-custom/contenttypes` folder for [Global Content-Types](xref:Basics.Data.ContentTypes.Global). If you now restart your DNN, then this new Content-Type is picked up and available in _all_ Apps on this Dnn. 


## Redistribution Scenario

The above example works well if you're fine with installing the JSON Content-Type manually on servers which will use the DataSource. 

For more advanced redistribution, you'll want to include the JSON file in your Dnn Installation package. But in that case it's not in `.data-custom` so 2sxc/EAV can't pick it up. 

To make sure that it's still available to all Apps on a server, you'll need to create a special class to register it on Start of DNN. All you have to do is create a `public` class which inherits `ToSic.Eav.Repositories.RepositoryInfoOfFolder`. On boot all these classes will be found and asked for other folders which have such data. Here's an example:

```c#
public class RegisterContentTypeForConfigUi: RepositoryInfoOfFolder
{
  public override List<string> RootPaths => new List<string>
  {
    HttpContext.Current.Server
      .MapPath("~/desktopmodules/ToSic.Eav.DataSources.SharePoint/.data")
  };
}
```

> [!TIP]
> Remember that the folder (in this case `.data`) is _not_ where the shared Content-Types are stored. They must be placed in a `contenttypes` subfolder. 


## History

1. General Tokens introduced in 2sxc 1.0
1. Most enhancements were in 2sxc 07.00

