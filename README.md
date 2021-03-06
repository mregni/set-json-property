## SetJsonProperty

Update a specific json object's property with a specified value. You can target nested json objects, and even objects that belong to an array. Available on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/publishers/ThatBlokeCalledJay)
  
> Note: SetJsonProperty has been designed to work with **Azure DevOps** pipelines.
  
## Set your property path

This couldn't be easier, simply specify the property path as you would in javascript.  

```json
{
    "MyProperty":"TheValueToUpdate"
}
```

To target the above property `MyProperty` and update it's value, you would set `MyProperty` as your Property Path.
  
### Nested objects

```json
{
    "Property1": "Value1",
    "MyNestedObject": {
        "MyProperty": "TheValueToUpdate"
    }
}
```

In the above example, you would target `MyProperty` the same as you would in javascript `MyNestedObject.MyProperty` You don't have to stop there, you can access properties on objects nested much deeper than that.
  
### Nested objects and arrays

```json
{
    "Property1": "Value1",
    "MyNestedObject": {
        "NestedProperty": [
            {
                "Key": "Value1"
            },
            {
                "Key": "Value2"
            },
            {
                "Key": "Value3"
            }
        ]
    }
}
```

In the above example, let's say you wanted to target the second instance of `Key` and update it's value. Again, as with javascript, you would specify the property path and an array index like so `MyNestedObject.NestedProperty[1].Key`

## Providing a new value

There are a number of ways you can provide a value with which to update your property. You can simply enter a value into the "Property Value" field e.g. `MyNewValue`. You can specify an environment variable too, for example your build number `$(Build.BuildNumber)`  
  
This means you can also access an environment variable that has been set by another task, previously in your pipeline.  

## Need help setting up

 Check out the [wiki](https://github.com/ThatBlokeCalledJay/set-json-property/wiki/Getting-Started) for more information.

## Minimum supported environments

- [Minimum agent version](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops): 2.123.0

## My use case example

**I need a version number.**  
I use [BugSnag](https://www.bugsnag.com/) for logging and alerting me of any errors in my app. One feature of BugSnag is, if I set a version number in my project's `appsettings.json` file (.net core app), I can associate errors with a specific app version. So, with that in mind, it would be handy if I could somehow get the current version number of my app and apply it to the BugSnag setting.  

**Acquiring a version number.**  
I also have an extension published in the market place which automatically updates you app's version number, [AutoAppVersion](https://marketplace.visualstudio.com/items?itemName=ThatBlokeCalledJay.thatblokecalledjay-autoappversion) or AAV for short. One of the settings for this extension enables you  to store a newly generated app version number in an environment variable of your choice.  Let's  say I specified `MyNewVersionNumber` as the environment variable. AAV would write the new value to this environment variable.

> **Note:** These environment variables don't need to be specified in your build definition's variables tab.

**Using the version number.**  
So, AAV has provided me with a useful value in the means of a version number, stored in an environment variable `MyNewVersionNumber`. All I need to do now is set SetJsonProperty's Property Value field to `$(MyNewVersionNumber)` and the previously generated version number can be saved into my appsettings.json file, lovely-jubbly.  

To achieve this, My SetJsonProperty task fields my look something like this:

Json File:      `MyApp/appsettings.json`  
Property Path:  `BugSnag.AppVersion`  
Property Value: `$(MyNewVersionNumber)`  

And that's it.

> **More Info:** Don't panic if that use case made no sense, you can check out the [my use case wiki](https://github.com/ThatBlokeCalledJay/set-json-property/wiki/My-Use-Case) for more information about how I achieved this.

#### All the version numbers

Check out the following scenario:

1. Increment your app's current version.
2. Apply new version number to FileVersion.
3. Apply new version number to AssemblyVersion.
4. Ensure .Net pack uses your new version number when generating new packages.
5. Make sure all new bugs that are sent to Bugsnag include the new version number.
6. Finally, notify Bugsnag of your latest release, and it's new version number.

If you find yourself in this scenario, [click here](https://thatblokecalledjay.com/blog/view/justanotherday/continuous-integration-and-version-number-madness-b95d40aaf761) to find out how my Azure DevOps extensions can be made to work together to automate this entire process.

#### On GitHub

- [ThatBlokeCalledJay](https://github.com/ThatBlokeCalledJay)
- [AutoAppVersion](https://github.com/ThatBlokeCalledJay/auto-app-version)  
- [SetJsonProperty](https://github.com/ThatBlokeCalledJay/set-json-property)  
  
#### On Visual Studio Marketplace

- [ThatBlokeCalledJay](https://marketplace.visualstudio.com/publishers/ThatBlokeCalledJay)
- [AutoAppVersion](https://marketplace.visualstudio.com/items?itemName=ThatBlokeCalledJay.thatblokecalledjay-autoappversion)  
- [SetJsonProperty](https://marketplace.visualstudio.com/items?itemName=ThatBlokeCalledJay.thatblokecalledjay-setjsonproperty)  
