# Static Code Analysis Guide For Visual Studio

### Auto remove unused using statements and format file on save

Install productivity power tools as per your visual studio version.

Productivity Power Tools 2013

https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.ProductivityPowerTools2013

Productivity Power Tools 2015

https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.ProductivityPowerTools2015

When installed follow these simple steps:
Tools->Options->Productivity Power Tools->Power Commands->General

Mark "Format Document on Save" and "Remove and Sort Usings on Save" as checked

#### Setting StyleCop

Usage of StyleCop is mandatory for all .NET projects. Following nuget packages must be installed in all projects under a solution.

https://www.nuget.org/packages/StyleCop.MSBuild

https://www.nuget.org/packages/StyleCop.Error.MSBuild


#### Updating Settings.StyleCop File

In order to modify rules in provided settings file install the visual style cop and update the rules
https://marketplace.visualstudio.com/items?itemName=ChristopheHEISER.VisualStyleCop

Note: Only TL should update the rules as per requirement.
