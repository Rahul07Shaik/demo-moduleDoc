# Module's Architecture 
## Introduction: 
  
   When migrating to a swift packages in Xcode, there are several challenges that developers may face. This document outlines some of the potential issues and the struggles we faced during the migration of modules.
   
### Documentation

- [Challenges](#Challenges)
- [Dependency conflicts](#Dependency-conflicts)
- [Minimisation](#Minimisation)
- [File Structure](#File-Structure)
- [Architecture Diagram](#Architecture-Diagram)
- [Package Setup](#Package-Setup)


## Challenges

There are few challenges we used to face in Module migration. 
   - Modules typically involve complex operations, often involving numerous callback functions that are integrated with container progressions.      As a result, they can be resource-intensive and require careful management to ensure optimal performance.
   - May require updating or replacing dependency target versions to work with a new swift packages.
   - Updating certain modules to their latest versions can cause internal errors in other modules that share the same dependencies.
   - Xcode may not support all types of packages, which can require manual integrations 
        
## Dependency conflicts

 Configuring new packages can present challenges, for projects with complex customization requirements. Version conflicts, dependencies           migration can result in errors.
 
## Minimisation 

   - Test the changes in a staging environment before deploying them to production to ensure proper functionality.
   - Use Git to review changes in existing dependencies versions and to provide an avenue for rolling back changes, if needed.
   - Ensure that all dependencies used by the package are up to date and compatible with the new Swift version.
   
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
# Architecture Diagram

![module-architecture](https://user-images.githubusercontent.com/11072850/209356532-bed36e86-1e21-47cd-812c-90fe1f1e72bb.png)

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
