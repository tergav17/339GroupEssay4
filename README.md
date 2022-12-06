# Infinity for Reddit Codebase Analysis

## Source Code Organization

An important part of large software projects is how source code is created and organized throughout the project. A complex application such as *Infinity for Reddit* can easily have hundreds, if not thousands, of individual source code files. If these files are poorly organized it can quickly impair a developers ability to navigate a project. This becomes increasingly critical in open source projects, where many developers will be working on different components with little communication between each other. Active open source projects also usually have many new contributors coming to the project who need to familiarize themself with the codebase. If not diligently enforced, code organization can spiral out of control and make it difficult for new and existing developers to interact with the codebase.

*Infinity for Reddit* makes an attempt at organizing its code. All Java source files for the application are found in the package `ml.docilealligator.infinityforreddit`. In this base package, there are about 51 different files in here, each pertaining to a different purpose. Files such as `Infinity.java` are high level instances which manage the initialization and general operation of the application. Other classes found in this package such as `ReportThing.java` pertain to very specific functions of the app. Additionally, this base package contains another 30 subpackages. These subpackages contain 2 - 30 child files, and usually pertain to a certain function or pattern type. Subpackages, even the larger ones, rarely have any further internal organization. One package may serve a number of different functions.

![Figure 1](./assets/root-directory.png)
<p align = "center">
Figure 1 - Loosely organized source files in the root directory
</p>

As alluded to in the previous paragraph, there are a number of issues with the way that *Infinity for Reddit* is organized. The first major issue is due to the amount and variety of classes found in the root package. There are 51 java source files found here. Between the different files, there are few consistent rules moderating what sort of classes are stored there. To remedy this cluttered organizational situation, better naming and sorting conventions can be used [[1](#references)]. These include:

- Keeping high level functional classes in the root package. These will serve to "link" the subpackages together. These are also the first packages that a new developer will encounter when reading the source code, and therefore should give an understanding on how the app as a whole works. Examples of these classes are `Infinity.java`, `AppComponent.java`, and `AppModule.java`.

- Source files that refer to specific atomized functions should be placed in their respective subpackage. This will allow developers to easily find them when the need arises. For example, the file `CustomFontReceiver.java` should be moved to the `font` package, as it serves the function of handling fonts. Other classes that could be sorted include `ImgurMedia.java`, `PollPayload.java`, `UserFlair`, and most other classes found in the root package.

One other thing that could be improved in the organizational strategy of *Infinity for Reddit* is the naming convention of the packages themself. Naming convention of this form is less of a defined science and more opinion based, but there still are commonly accepted strategies [[2](#references)]. A helpful change that *Infinity for Reddit* could make its package naming scheme to organize files by function and not data structure or pattern. Packages such as `award`, `message`, and `multireddit` work well because they clearly define what all of the components in the package are working to achieve. Names such as `activities`, `adapters`, and `services` are more confusing because they refer to what pattern the classes are, instead of what they do. Source files found in these packages refer to many different, unique functions. When a developer is attempting to work on a subsystem, they will have to check these packages in addition to the subsystem package to fully understand its function. To improve organization, packages of the latter type should be broken up, and source files should be moved to their respective functional package.

![Figure 2](./assets/adapters.png)

<p align = "center">
Figure 2 - Adapter patterns referring to various unrelated subsystems
</p>

## Static Code Analysis

Static code analysis is the process of inspecting source code to find possible defects. Many clean code bases enforce a strict static code analysis process. By statically analyzing source code, the introduction of poor practices such as security flaws, formatting issues, and code smells can be avoided. Ideally, issues such as these are identified and fixed before merging into master. Unfortunately, Infinity for Reddit does not strictly adhere to these practices, leading to an untidy codebase.


<p align = "center">
<img src="./assets/android-studio-inspection-report.png"/><br>
Figure 3: Android Studio inspection report. This static code analysis discovered 13 errors, 5,715 warnings, and 2,373 potential typos.
</p>


A common method to inspect an Android project is to run a code analysis using Android Studio. Figure 3, above, shows the results of this analysis on Infinity for Reddit. A significant number of errors and warnings were discovered, indicating room for improvement within the codebase.

> Note: to run your own static code analysis of the repository in Android Studio, click 'Code', 'Inspect Code', 'Whole Project', and 'OK'.

The inspection calls out a variety of issues within the codebase, each with a varying level of severity. Examples of the critical issues include:
- Using deprecated Java APIs
- Unchecked compiler warnings
- Using unstable Java APIs
- Accessing an object that may be null

These critical issues are important to address as they yield the greatest risk of builds failing or runtime errors. The good news about the analysis is many of the reported issues are low severity and unlikely to impact the end user. Many of these issues can be resolved manually or even automatically by an IDE.

Although no single issue is a showstopper for the project on its own, Team 3 would argue the state of the codebase—through the lense of static code analysis—is poor. The culmination of thousands of issues has led to a sloppy and potentially unstable codebase. Without resolving these issues, compiling on a slightly different Java version, updating a package, or changing the wrong line of code could cause major issues within the application.

Going forward, there are a few steps we recommend be taken to achieve better code quality:

1. There should be a well-documented set standards present in the repository which are enforced when considering all changes. Any PRs going against these standards must justify the reasoning.
2. Time should be taken to perform rework and reduce the statically identified issues.


# References

[1] [Java package naming conventions]
(https://www.geeksforgeeks.org/java-naming-conventions/)

[2] [Best practices for Java package organization]
(https://stackoverflow.com/questions/3226282/are-there-best-practices-for-java-package-organization)

[3] [Improve wiki link handling (#1184)](https://github.com/Docile-Alligator/Infinity-For-Reddit/pull/1184)
