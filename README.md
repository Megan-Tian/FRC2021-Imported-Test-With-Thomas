# FRC2021-Imported-Test-With-Thomas

1. Cloned FRC2021 code
2. Followed Thomas's magic steps:

## Thomas works his magic: An Explanation

### 0. Need to get correct submodule from robototes
- get 2022_updates branch and pull the branch down

3 command line arguments:
- `git submodule add https://github.com/robototes/2910Common.git common`
	- clones master branch
	- this is the branch we used originally, may change dependending on how 2910/robototes updates the common library
	- to change the version, just swap out the github link
- may have to do a `git init` if the last step doesn't work
- `git config -f .gitmodules submodule.common.branch 2022_update`
	- switches the branch to the 2022_update one
	- may need to do a git fetch (?)
- `git submodule update --remote common`
	- updates to 2022 branch

### 1. build.gradle and settings.gradle in main robot project
`include 'common'` in settings.gradle (at bottom of page)

`implementation project(':common')` inside dependencies{} in build.gradle

### 2. Set `id 'net.ltgt.errorprone' version '2.0.2'` in plugins{} function in the Common build.gradle
- we added `version '2.0.2'` where there previously was no version specification 
- net.ltgt.errorprone is a github repo with version # 2.0.2.; the com.google.errorprone stuff below in dependencies{} is different 

this step may no longer be needed if 2910/robototes updates 

### 3. Build 2910 Common stuff FIRST 
- The main project is dependent on the smaller project's jar file
- If the main project tries to compile before 2910 it's a problem because it needs the jar file from the 2910 compilation
- following steps manually determine build order

To fix:
- Put `gradlew build   -Dorg.gradle.java.home="C:\Users\Public\wpilib\2022\jdk"` (or whatever VSCode terminal runs) into the command prompt. Do this inside the COMMON directory FIRST. 
- Then put the same prompt into the MAIN (robot code w/ subsystems/commands) NEXT.
- Should be able to build in VSCode fine after. 

Additional notes:
- there is probably a way to specify build order in gradle

### 4. When there's new versions of the 2910/robototes updates
- The version used in the corrections done with Thomas are: de9c0a5b10a2ea9063ec1013bceab0de5925bcb7
- When you clone this repo (or the FRC2022 repo on @Lakemonsters 2635 that was cloned off of this project), the common folder is there but it's empty
- To get the code in the common library, do as follows:

1. `git submodule init` inside the overall project directory (ex. FRC2022)
2. `git submodule update` also inside the overall project directory (ex. FRC2022)

You may have to run through steps 1-3 again 

[Link to Git submodule instructions](https://git-scm.com/book/en/v2/Git-Tools-Submodules#:~:text=The%20DbConnector%20directory%20is%20there%2C%20but%20empty.%20You%20must%20run%20two%20commands%3A%20git%20submodule%20init%20to%20initialize%20your%20local%20configuration%20file%2C%20and%20git%20submodule%20update%20to%20fetch%20all%20the%20data%20from%20that%20project%20and%20check%20out%20the%20appropriate%20commit%20listed%20in%20your%20superproject%3A)
