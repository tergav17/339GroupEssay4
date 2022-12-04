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

![Figure 3](./assets/android-studio-inspection-report.png)
<p align = "center">
Figure 3: Android Studio inspection report. This static code analysis discovered 13 errors, 5,715 warnings, and 2,373 potential typos.
</p>


An easy method to inspect the current state of a project is to run code analysis on the entire project using Android Studio. Figure 1, above, shows the results of this analysis on Infinity for Reddit. A significant number of errors and warnings were discovered. It should be noted, not all the discovered issues necessarily need to be resolved. For example, a warning is being generated due to an unused no-argument constructor, but it is common practice to always expose a constructor with no arguments.

There are varying levels of importance when analyzing the issues discovered. The high priority issues are critical to application security, longevity, and performance. Issues found pertaining to this include nullability issues, using deprecated code from third party dependencies, and compiler warnings.

Lower priority issues often do not impact the end-user, but are still important for maintaining a sustainable code repository. Examples of these issues include linting issues, unused imports, if statements with identical branches, and commented out code. Many of these issues can be quickly resolved manually, or even automatically by an IDE.

Whenever someone contributes code to Infinity for Reddit, the changes are analyzed by CodeQL as a part of the GitHub Actions pipeline. The pipeline fails if any new issues are introduced to the codebase. Recently, a pull request (PR) was opened that attempted to improve wiki link handling. Within the PR, a regular expression used to detect wiki links was modified. This modified expression successfully matched the appropriate cases, but in instances where a dash was repeated many times, the regular expression could grind to a halt due to recursive backtracking. The pipeline automatically reported this issue and allowed the issue to be resolved [[3](#references)].

Over time, the technical debt introduced to the codebase by not enforcing merge standards adds up. Ideally, a set of well-documented standards would be present in the repository and enforced when considering all changes. If a PR contains changes that go against the standard, the modifications would not be accepted until these issues are either resolved or deemed acceptable. Additionally, taking time to perform rework and reduce the statically identified issues will help ensure a healthier codebase.


# References

[1] [Java package naming conventions]
(https://www.geeksforgeeks.org/java-naming-conventions/)

[2] [Best practices for Java package organization]
(https://stackoverflow.com/questions/3226282/are-there-best-practices-for-java-package-organization)

[3] [Improve wiki link handling (#1184)](https://github.com/Docile-Alligator/Infinity-For-Reddit/pull/1184)

[4] [Fix reply markdown (#974)](https://github.com/Docile-Alligator/Infinity-For-Reddit/actions/runs/3095391479/jobs/5009749979)