# Postulate.Mvc

A set of base classes and helpers for improving productivity in ASP.NET MVC5 when used with [Postulate.Orm](https://github.com/adamosoftware/Postulate.Orm).

Nuget package: **Postulate.Mvc**

After you install the Nuget package, open your `~/Views/Web.config `file and add some namespaces to the `pages/namespaces` section:

    <add namespace="Postulate.Mvc"/>
    <add namespace="Postulate.Mvc.Helpers"/>
    <add namespace="Postulate.Mvc.Extensions"/>

Next, you should install one or the other of these: **Postulate.Orm.MySql** or **Postulate.Orm.SqlServer** depending on what back-end you're targeting.

## Highlights

- [ControllerBase](/Postulate.Mvc/ControllerBase.cs) enhances your controllers by offering
    - data access through the [Db](/Postulate.Mvc/ControllerBase.cs#L21) property as well as [SaveRecord](/Postulate.Mvc/ControllerBase.cs#L89), [DeleteRecord](/Postulate.Mvc/ControllerBase.cs#L106), and [UpdateRecord](/Postulate.Mvc/ControllerBase.cs#L72) methods. These can greatly simplify typical Controller CRUD actions if you're using Postulate.Orm.
    - convenient and efficient drop down list filling with the [FillSelectLists](/Postulate.Mvc/ControllerBase.cs#L142) method and its several overloads. FillSelectLists executes multiple list-filling queries in one round trip using Dapper's QueryMultiple method.
    - access to Json-based user profile data with the [LoadUserData](/Postulate.Mvc/ControllerBase.cs#L254) method.
    
- [ProfileControllerBase](/Postulate.Mvc/ProfileControllerBase.cs) builds on `ControllerBase` by integrating strong-typed access to your user profile class within your controller. Just about any web app requires user profile data at the controller level. Inherit from this class to incorporate user profile data directly into your controllers. Your user profile model class must implement [IUserProfile](https://github.com/adamosoftware/Postulate.Orm/blob/master/Core/Interfaces/IUserProfile.cs). In addition, this class offers
    - profile validation with [ProfileRule](/Postulate.Mvc/ProfileControllerBase.cs#L20) property. Specify a rule that describes a well-formed user profile. For example, in a multi-tenant system, you might need to ensure that all users have a tenant selected. By setting the ProfileRule for the controller, users are automatically redirected to the profile page of your choice (set by the [ProfileUpdateRedirect](/Postulate.Mvc/ProfileControllerBase.cs#L25) property) to complete their setup if necessary.
    - access to user profile data through the [CurrentUser](/Postulate.Mvc/ProfileControllerBase.cs#L15) property.

- [GridEditor](https://github.com/adamosoftware/Postulate.Mvc/blob/master/Postulate.Mvc/Helpers/GridEditor.cs) offers inline editing of tables. Demo video coming soon!

- [HtmlHelpers](https://github.com/adamosoftware/Postulate.Mvc/tree/master/Postulate.Mvc/Helpers) provides some miscellaneous helpers. One in particular [ActionNameField](/Postulate.Mvc/Helpers/NavFields.cs#L18) passes the current action name to a controller for easy redirect back to a failing page in case of an error.

- [SelectListQuery](/Postulate.Mvc/SelectListQuery.cs) describes a query used to fill a drop down list, and is used by BaseController.FillSelectLists.

## What's the difference between BaseController.LoadUserData and BaseProfileController?

BaseController.LoadUserData deserializes a class from a Json file from **~/App_Data/UserData/{userName}/{[Filename](/Postulate.Mvc/Abstract/UserData.cs#L20)}**. This is handy when you have user settings that you're too lazy to store in the database because they're too specific, complex, or rarely used. Just be sure you preserve the ~/App_Data folder when publishing! BaseProfileController is preferred for accessing user profile data that is universal in your app and that is strong-typed in the database.
