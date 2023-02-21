# Module's Architecture 
## Introduction: 
  
   When migrating to a swift packages in Xcode, there are several challenges that developers may face. This document outlines some of the potential issues and the struggles we faced during the migration of modules.
   
### Documentation

- [Challenges](#Challenges)
- [Dependency conflicts](#Dependency-conflicts)
- [Mitigation](#Mitigation)
- [Learning curve](#Learning-curve)
- [File Structure](#File-Structure)
- [Package Setup](#Package-Setup)


## Challenges

There are few challenges we used to face in Module migration. 
   - Swift packages are struggle to update the latest dependencies and version updating conflicts.
   - May require updating or replacing existing version to work with a new swift packages.
   - Changes to the file structure and build settings may be required, which can be time-consuming and disruptive.
   - Xcode may not support all types of packages, which can require manual integration or workarounds.
   - Smooth development workflow can be maintained with proper planning.
        
## Dependency conflicts

  Configuring new packages can be tricky, especially for projects with many customization needs. Conflicting version update during migration can   lead to errors or unexpected behavior that's hard to resolve if the new package's has different dependencies than the old one.
    
## Minimisation 

   - Before deploying the migration to production, test the changes in a staging environment to ensure that everything is working as expected.
   - Review existing dependencies version changes in Git can help you keep track of changes and provide a way to roll back changes if                necessary.
   - Communicate with other developers when migrating a package, inform them about the migration process, notify them of any necessary changes        they may need to make.
   - Make sure that any dependencies used by the package are up to date and compatible with the new version of Swift.

## Learning curve 

  - Migrating to a new package dependency can be challenging for developers, requiring them to learn accurate module naming conventions of           dependency, and versions computability.
  - To overcome these challenges, developers should carefully plan and test the migration process without errors.
  - Steps that can be taken to ensure a smooth transition include resolving dependency conflicts, and unit-testing.

## File Structure
```
Module
    ├── A_Model                      # Model refers either to a domain model, which represents real state content
    ├── B_View                       # View is the structure, layout, and appearance of what a user sees on the screen.
    ├── C_ViewModel                  # The view model has been described as a state of the data in the model. (Business Logic)
    ├── D_ViewController             # View Controller is responsible for displaying the data of our iOS application on the screen
    ├── E_Services                   # The service is used to send HTTP requests or communicate with CoreData, AudioPlayer, and other serivces.
    ├── A_<Module>Module.swift        # Root file container all the interface and callback initialization
    └── A_<Module>Interface.swift     # File consists of protocols that are needed for Module
```

## Package Setup

### Adding the dependency

Adding module's in package dependency:
``` swift
// MARK: - Products 
var products: [product] = [
        .library(name: "ContactsModel", targets: ["ContactsModel"]),
        .library(name: "ContactsApi", targets: ["ContactsApi"]),
        .library(name: "ContactModule", targets: ["ContactModule"])
    ],
```
dependencies:
``` swift 
// MARK: - Dependencies
var dependencies: [PackageDescription.Package.Dependency] = [
    .package(url: "git@github.com:Adaptavant/IOS-Interface-Module.git", .upToNextMinor(from: .init(0, 0, 97))),
```
Targets:
``` swift 
// MARK: - Targets
var targets: [Target] = [
    .target(
        name: "ContactsModel",
        dependencies: [
            .product(name: "AnywhereFoundationKit", package: "IOS-Core-Kit"),
            .product(name: "AnywhereSerializationKit", package: "IOS-Core-Kit"),
            .product(name: "CommonInterfaceModule", package: "IOS-Common-Interface"),
            .product(name: "NavigationKit", package: "IOS-View-Kit")
        ],
        path: "Sources/ContactsModel"
    ),
  ]
```
> **Note**\
When using a Module's architecture-related dependency, there are no more out-breaks conflicts to use module dependency. Indeed, kit's as well
