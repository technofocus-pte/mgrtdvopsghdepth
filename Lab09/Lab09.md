# Lab 09 -Testing & Code Quality in GitHub After Azure DevOps Migration

## Exercise 1 - Set up your Azure DevOps environment

In this unit, you'll ensure that your Microsoft Azure DevOps
organization is set up to complete the rest of this module.

To do this, you:

- Set up an Azure DevOps project for this module.

- Move the work item for this module on Azure Boards to
  the **Doing** column.

- Make sure your project is set up locally so that you can push changes
  to the pipeline.

### Task 1 : Create an Azure DevOps personal access token

1.  Create an Azure DevOps personal access token (PAT). Open a new tab
    in your browser and navigate to - <https://portal.azure.com/> and
    sign in with assigned account.

2.  Search for **Azure DevOps** and select **Azure DevOps
    organizations**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Click on **My Azure DevOps Organization** hyper link.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Click on **Create new organization** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  Click on **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  Enter the organization name as : **tffabrikamXXX** (should be
    unique, replace XXX with nunber)enter the characters shown in your
    screen and then click on **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

7.  In the top right corner of the screen, click **User
    settings**.Click Personal access tokens.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

8.  Select **+ New Token**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

9.  Enter the name as : **devopstoken** and select the following scopes
    (you may need to select Show all scopes at the bottom of the page to
    reveal all scopes):

    - Agents Pool: **Read & write**

    - Build: **Read & execute**

    - Code: **Read, write, & manage**

    - Deployment Groups : Read & manage

    - GitHub Connections: Read & manage

    - Pipeline Resources: Use and manage

    - Project and Team: **Read, write, & manage**

    - Pull Request Threads: Read & write

    - Release: **Read, write, execute, & manage**

    - Service Connections: **Read, query, & manage**

    - Team Dashboard: Read & manage

    - Task Groups: **Read, create, & manage**

    - **Test Management: Read & write**

    - Variable Groups: **Read, create, & manage**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

10. Copy the generated API token and save it in a safe location. For
    your security, it won't be shown again.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

### Task 2 : Download and configure the agent

1.  From Left down corner, click on **Organization settings**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image10.png)

2.  Choose **Agent pools**.Select the **Default** pool.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  Select the **Agents** tab, and choose **New agent**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  On the **Get the agent** dialog box, choose **Windows**.On the left
    pane, select the processor architecture of the installed Windows OS
    version on your machine. The x64 agent version is intended for
    64-bit Windows, whereas the x86 version is intended for 32-bit
    Windows. If you aren't sure which version of Windows is
    installed, [follow these instructions to find
    out](https://learn.microsoft.com/en-us/windows/client-management/windows-version-search).

5.  On the right pane, click the **Download** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

6.  Follow the instructions on the page to download the agent.

7.  Unpack the agent into the directory of your choice. Make sure that
    the path to the directory contains no spaces because tools and
    scripts don't always properly escape spaces. A recommended folder
    is C:\agents. Extracting in the download folder or other user
    folders may cause permission issues.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

** Important**

We strongly recommend you configure the agent from an elevated
PowerShell window. If you want to configure as a service, this
is **required**.You must not use [**Windows PowerShell
ISE**](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/ise/introducing-the-windows-powershell-ise) to
configure the agent.

** Important**

For security reasons we strongly recommend making sure the agents folder
(C:\agents) is only editable by admins.

** Note**

Please avoid using mintty based shells, such as git-bash, for agent
configuration. Mintty is not fully compatible with native Input/Output
Windows API and we can't guarantee the setup script will work correctly
in this case.

### Task 3 : Configure and run the agent

1.  Start an elevated (PowerShell) window as administrator and set the
    location to where you unpacked the agent.

cd “C:\agents“

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image15.png)

2.  Run config.cmd. This will ask you a series of questions to configure
    the agent. When setup asks for your server URL, for Azure DevOps
    Services, answer https://dev.azure.com/{your-organization}.

.\config.cmd

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

3.  The authentication method used for registering the agent is used
    only during agent registration. Press Enter accepting the
    Authentication mthos is PAT when prompts for -**Enter authentication
    type (press enter for PAT) and then enter your PAT and press
    Enter.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image17.png)

4.  Press Enter when prompts “Enter agent pool (press enter for
    default)”

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

5.  Type Agent name of your choice and press Enter

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image19.png)

6.  Press Enter when prompt “Enter work folder (press enter for \_work)”
    as we want to use **Default** Agent

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image20.png)

7.  Just Enter when prompt Enter run agent as service? (Y/N) (press
    enter for N) and Enter configure autologon and run agent on startup?
    (Y/N) (press enter for N)

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)

8.  Run the following the command to start the agent.

.\run.cmd

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image22.png)

9.  Minimize the window and continue with next tasks.

Note : To restart the agent, press Ctrl+C to stop the agent, and then
run **run.cmd** to restart it.

### Task 4 : Get the Azure DevOps project

Here, you ensure that your Azure DevOps organization is set up to
complete the rest of this module. You do this by running a template that
creates a project for you in Azure DevOps.

1.  Open Visual Studio from Desktop and sign in with your accounts.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  Click on **Clone a repository**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  Enter Repository location as
    <https://github.com/microsoft/AzDevOpsDemoGenerator.git> and select
    location and then click on **Clone**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

![A computer screen shot of a computer AI-generated content may be
incorrect.](./media/image26.png)

4.  **Set ADOGenerator as the Startup Project** In Visual Studio.
    Right-click on the ADOGenerator project in the Solution Explorer.
    Select **Set as Startup Project**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

5.  **Build the Solution** Build the solution to ensure all dependencies
    are restored and the project compiles successfully.

6.  In Visual Studio, **right-click on the solution in the Solution
    Explorer** and select **Build Solution.**

> Note : Alternatively, you can use the command line: dotnet build

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

7.  Wait for the build to complete.

![A computer screen shot of a computer AI-generated content may be
incorrect.](./media/image29.png)

8.  Run the Project To run the project as a console application. In
    Visual Studio**, press F5** or click on the **Start** button.

Note : Alternatively, you can run the project from the command line:

> **dotnet run --project src/ADOGenerator/ADOGenerator.csproj**
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

9.  Enter **1** (Create a new project….)when prompted for options

![A computer screen with text on it AI-generated content may be
incorrect.](./media/image31.png)

10. When prompted to **Enter the template number from the list of
    templates**, enter ** 24 **for** Run quality tests in your build
    pipeline using Azure Pipelines using Git and GitHub**, then
    press **Enter**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

11. Choose your authentication method as Personal Access Token (PAT).
    Type **2** and press Enter.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

** Note:**If you set up a PAT, make sure to authorize the
necessary **scopes**. For this module, you can use **Full access**, but
in a real-world situation, you should ensure you grant only the
necessary scopes.

12. Enter your Azure DevOps organization name (eg :**tffabrikanXX**),
    then press **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

13. Enter your Azure DevOps PAT, then press **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

14. Enter a project name such as ***Space Game - web - Tests***, then
    press **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image37.png)

15. Once your project is created, go to your Azure DevOps organization
    in your browser
    (at https://dev.azure.com/\<your-organization-name\>/) and select
    the project.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

### Task 5 : Fetch the branch from GitHub

1.  On GitHub, go to
    the https://github.com/MicrosoftDocs/mslearn-tailspin-spacegame-web.git repository.

2.  Select **Fork** at the top-right of the screen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

3.  Choose your GitHub account as the Owner, then select **Create
    fork**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

4.  Select Code, and then, from the HTTPS tab, select the copy button to
    copy the URL to your clipboard.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

5.  Open Visual Studio Code, go to the terminal window.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image42.png)

6.  Update below command with your forked repo link and run the git
    clone command. Replace YOURUSERNAME with your GitHub username.

> git clone
> https://github.com/\<\<YOURUSERNAME\>\>/mslearn-tailspin-spacegame-web.git
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image43.png)

7.  Open Terminal and run below command to go the folder

cd mslearn-tailspin-spacegame-web

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image44.png)

8.  A *remote* is a Git repository where team members collaborate (like
    a repository on GitHub). Here you list your remotes and add a remote
    that points to Microsoft's copy of the repository so that you can
    get the latest sample code.

9.  Run the following command to list your remotes:

git remote -v

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image45.png)

10. Origin specifies your repository on GitHub. When you fork code from
    another repository, the original remote (the one you forked from) is
    commonly named upstream.

11. Run the following command to create a remote named *upstream* that
    points to the Microsoft repository:

> git remote add upstream
> https://github.com/MicrosoftDocs/mslearn-tailspin-spacegame-web.git
>
> git remote -v

![](./media/image46.png)

## Exercise 2 - Add unit tests to your application

In this unit, we'll add unit tests to the automated build that we
created with Microsoft Azure Pipelines. Regression bugs are creeping
into your team's code and breaking the leaderboard's filtering
functionality. Specifically, the wrong game mode keeps appearing.

The following image illustrates the problem. When a user selects "Milky
Way" to show only scores from that game map, they get results from other
game maps, such as Andromeda.

Here's the process to follow:

1.  Fetch a branch from the GitHub repository that contains the unit
    tests.

2.  Run the tests locally to verify that they pass.

3.  Add tasks to your pipeline configuration to run the tests and
    collect the results.

4.  Push the branch to your GitHub repository.

5.  Watch your Azure Pipelines project automatically build the
    application and run the tests.

###  Task 1 : Create a service connection for ARM and GitHub

Here, you create a service connection that enables Azure Pipelines to
access your Azure subscription. Azure Pipelines uses this service
connection to deploy the website to App Service. You created a similar
service connection in the previous labs.

** Important:**Make sure that you're signed in to both the Azure portal
and Azure DevOps under the same Microsoft account.

1.  In Azure DevOps, go to your **Space Game - web – Tests** project.

![](./media/image47.png)

2.  From the lower-left corner of the page, select **Project settings**.

![](./media/image48.png)

3.  Click on **Service connections** under **Pipelines** and then click
    on **New Service connection** button.

![](./media/image49.png)

4.  Select **GitHub** and then click **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

5.  Select **AzurePipelines** from **OAuth Configuration** drop down and
    then click on **Authorize** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

6.  Sign into GitHub .Keep the default service name, select **Grant
    access permission to all pipelines** security check box and then
    click on **Save** .

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

![](./media/image53.png)

7.  Click on project name from top navigation menu.

![](./media/image54.png)

8.  Select the Pipelines from left navigation menu and select the
    pipeline to **Edit**.

![](./media/image55.png)

9.  Select **unit-tests** branch and then click next to **Run** button
    and select **Triggers**.

![](./media/image56.png)

10. Configure CI settings under **Triggers** tab, **select Override the
    YAML continuous integration trigger from here** checkbox and then
    include **unit-tests** branch

![](./media/image57.png)

11. Select **YAML** tab and select **Pipeline and select Default agent
    pool under Default agent pool for YAML drop down**

![](./media/image58.png)

12. Select **YAML** tab and select **Get sources-\>GitHub** and then
    click on **Repository**

![](./media/image59.png)

13. Select the repo - **mslearn-tailspin-spacegame-web** and then click
    on **Select** button.

![](./media/image60.png)

14. Keep all the default values and then click on **Save & queue-\>
    Save**.

![](./media/image61.png)

15. Enter some comment and click on Save to save the build pipeline.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image62.png)

### Task 2 : Add and Run unit tests

Here, you'll fetch the unit-tests branch from GitHub and check out, or
switch to, that branch.

This branch contains the *Space Game* project that you worked with in
the previous modules and an Azure Pipelines configuration to start with.

1.  In Visual Studio Code, open the integrated terminal.

2.  Run the following git commands to fetch a branch
    named unit-tests from the Microsoft repository, and then switch to
    that branch.

git fetch upstream unit-tests

git checkout -B unit-tests upstream/unit-tests

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image63.png)

3.  The format of this command enables you to get starter code from the
    Microsoft GitHub repository, known as upstream. Shortly, you'll push
    this branch to your GitHub repository, known as origin.

4.  As an optional step, open the *azure-pipelines.yml* file in Visual
    Studio Code and familiarize yourself with the initial configuration.

5.  Run dotnet build to build each project in the solution.

dotnet build --configuration Release

![A computer screen shot of text AI-generated content may be
incorrect.](./media/image64.png)

6.  Run the following dotnet test command to run the unit tests:

dotnet test --configuration Release --no-build

The --no-build flag specifies not to build the project before running
it. You don't need to build the project because you built it in the
previous step.

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image65.png)

7.  Notice that there were five total tests. Although we defined just
    one test method, FetchOnlyRequestedGameRegion, that test is run five
    times, once for each game map as specified in the TestCase inline
    data.

8.  Run the tests a second time. This time, provide the --logger option
    to write the results to a log file.

dotnet test Tailspin.SpaceGame.Web.Tests --configuration Release
--no-build --logger trx

You see from the output that a TRX file is created in
the **TestResults** directory.

A TRX file is an XML document that contains the results of a test run.
It's a popular format for test results because Visual Studio and other
tools can help you visualize the results.

![](./media/image66.png)

Later, you'll see how Azure Pipelines can help you visualize and track
your test results as they run through the pipeline.

** Note:**TRX files are not meant to be included in source control.
A *.gitignore* file allows you to specify which temporary and other
files you want Git to ignore. The project's *.gitignore* file is already
set up to ignore anything in the *TestResults* directory.

9.  As an optional step, in Visual Studio Code, open
    the *DocumentDBRepository_GetItemsAsyncShould.cs* file from
    the *Tailspin.SpaceGame.Web.Tests* folder and examine the test code.
    Even if you're not interested in building .NET apps specifically,
    you might find the test code useful because it resembles code you
    might see in other unit test frameworks.

**Add tasks to your pipeline configuration**

Here, you'll configure the build pipeline to run your unit tests and
collect the results.

1.  In Visual Studio Code, modify *azure-pipelines.yml* as follows:

> trigger:
>
> \- '\*'
>
> pool:
>
>   name: 'Default'
>
>   demands:
>
>   - npm
>
> variables:
>
>   buildConfiguration: 'Release'
>
>   wwwrootDir: 'Tailspin.SpaceGame.Web/wwwroot'
>
>   dotnetSdkVersion: '8.x'
>
> steps:
>
> \- task: UseDotNet@2
>
>   displayName: 'Use .NET SDK $(dotnetSdkVersion)'
>
>   inputs:
>
>     version: '$(dotnetSdkVersion)'
>
> \- task: Npm@1
>
>   displayName: 'Run npm install'
>
>   inputs:
>
>     verbose: false
>
> \- script: './node_modules/.bin/node-sass $(wwwrootDir) --output
> $(wwwrootDir)'
>
>   displayName: 'Compile Sass assets'
>
> \- script: 'npx gulp'
>
>   displayName: 'Run gulp tasks'
>
> \- script: 'echo "$(Build.DefinitionName), $(Build.BuildId),
> $(Build.BuildNumber)" \> buildinfo.txt'
>
>   displayName: 'Write build info'
>
>   workingDirectory: $(wwwrootDir)
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Restore project dependencies'
>
>   inputs:
>
>     command: 'restore'
>
>     projects: '\*\*/\*.csproj'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Build the project - $(buildConfiguration)'
>
>   inputs:
>
>     command: 'build'
>
>     arguments: '--no-restore --configuration $(buildConfiguration)'
>
>     projects: '\*\*/\*.csproj'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Run unit tests - $(buildConfiguration)'
>
>   inputs:
>
>     command: 'test'
>
>     arguments: '--no-build --configuration $(buildConfiguration)'
>
>     publishTestResults: true
>
>     projects: '\*\*/\*.Tests.csproj'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Publish the project - $(buildConfiguration)'
>
>   inputs:
>
>     command: 'publish'
>
>     projects: '\*\*/\*.csproj'
>
>     publishWebProjects: false
>
>     arguments: '--no-build --configuration $(buildConfiguration)
> --output $(Build.ArtifactStagingDirectory)/$(buildConfiguration)'
>
>     zipAfterPublish: true
>
> \- task: PublishBuildArtifacts@1
>
>   displayName: 'Publish Artifact: drop'
>
>   condition: succeeded()

![](./media/image67.png)

2.  In the integrated terminal, add *azure-pipelines.yml* to the index,
    commit the changes, and push the branch up to GitHub.

git add azure-pipelines.yml

git commit -m "Run and publish unit tests"

git push origin unit-tests

![](./media/image68.png)

6.  Switch back to Azure Pipelines, Click on running pipeline.

![](./media/image69.png)

7.  Click on the running job.

![](./media/image70.png)

8.  Click on **View** button next to warning message “  
    **This pipeline needs permission to access a resource before this
    run can continue**”

![](./media/image71.png)

9.  Click on **Permit** and permit access for the job to run.

![](./media/image72.png)

10. Click on Job,You see that the **Run unit tests - Release** task runs
    the unit tests just as you did manually from the command line.

![](./media/image73.png)

11. Navigate back to the pipeline summary.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image74.png)

![](./media/image75.png)

12. Move to the **Tests** tab.You see a summary of the test run. All
    five tests have passed.

![](./media/image76.png)

13. In Azure DevOps, select **Test Plans**, and then select **Runs**.You
    see the most recent test runs, including the one you just ran.

![](./media/image77.png)

14. Double-click the most recent test run.You see a summary of the
    results.

![](./media/image78.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image79.png)

## Exercise 3 - Add a testing widget to your dashboard

In this unit, you'll add a widget to your dashboard to help visualize
your test runs over time.

### Task 1 : Add the widget to the dashboard

1.  In your Azure DevOps project, select **Overview**, and then
    select **Dashboards**.

![](./media/image80.png)

** Note:**If you ran the template to create the Azure DevOps project,
you won't see the dashboard widgets you set up in previous modules.

2.  Select **Add a widget**.

![](./media/image81.png)

3.  In the **Add Widget** pane, search for **Test Results
    Trend**.Select **Test Results Trend** and then click on **Add**.

![](./media/image82.png)

4.  Select the **Gear** icon to configure the widget.

![](./media/image83.png)

5.  Under Build pipeline, select your pipeline.Keep the other default
    settings.Select **Save**.

> ![](./media/image84.png)

6.  Select **Done Editing**.

![](./media/image85.png)

## Exercise 4 - Perform code coverage testing

Much like the tool you use for unit testing, the tool you use for code
coverage depends on the programming language and application framework.

When you target .NET applications to run on
Linux, [coverlet](https://github.com/tonerdo/coverlet) is a popular
option. Coverlet is a cross-platform, code-coverage library for .NET.

### Task 1 : Run code coverage locally

Before you write any pipeline code, you can try things manually to
verify the process.

1.  In Visual Studio Code, open the integrated terminal.

2.  Run the following dotnet new command to create a local tool manifest
    file. The command creates a file named *.config/dotnet-tools.json*.

dotnet new tool-manifest

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image86.png)

3.  Run the following dotnet tool install command to install
    ReportGenerator. This command installs the latest version
    of ReportGenerator and adds an entry to the tool manifest file.

dotnet tool install dotnet-reportgenerator-globaltool

![](./media/image87.png)

4.  Run the following dotnet add package command to add
    the coverlet.msbuild package to
    the *Tailspin.SpaceGame.Web.Tests* project:

dotnet add Tailspin.SpaceGame.Web.Tests package coverlet.msbuild

![A screen shot of a computer error AI-generated content may be
incorrect.](./media/image88.png)

5.  Run the following dotnet test command to run your unit tests and
    collect code coverage:

** Note:**If you're using the PowerShell terminal in Visual Studio, the
line continuation character is a backtick (**\`**), so use that
character in place of the backslash character (**\\**) for multi-line
commands.

dotnet test --no-build --configuration Release /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura /p:CoverletOutput=./TestResults/Coverage/

![A black screen with white text AI-generated content may be
incorrect.](./media/image89.png)

6.  Run the following dotnet tool run command to use ReportGenerator to
    convert the Cobertura file to HTML.Many HTML files will appear in
    the *CodeCoverage* folder at the root of the project.

> dotnet tool run reportgenerator -reports:./Tailspin.SpaceGame.Web.Tests/TestResults/Coverage/coverage.cobertura.xml -targetdir:./CodeCoverage -reporttypes:HtmlInline_AzurePipelines

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image90.png)

7.  In Visual Studio Code, expand the *CodeCoverage* folder,
    **right-click *index.htm***, and then select **Reveal in File
    Explorer** (**Reveal in Finder** on macOS or **Open Containing
    Folder** on Linux).

![](./media/image91.png)

8.  In Windows Explorer (Finder on macOS),
    double-click ***index.htm*** to open it in a web browser.

![](./media/image92.png)

9.  You'll see the coverage report summary.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.png)

10. Scroll to the bottom of the page to see a coverage breakdown by
    class type.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image94.png)

### Task 2 : Configure pipeline to include code-coverage branch

1.  Select pipeline and click on Edit.

![](./media/image95.png)

2.  Select code-coverage branch ,select Triggers from run.

![](./media/image96.png)

3.  Select CI-repo and include code-coverage branch under Branch filters
    as show in below image.

![](./media/image97.png)

4.  Enter comment and then save.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image98.png)

### Task 3 : Create a branch

Now that you can build a code coverage report locally, you're ready to
add tasks to your build pipeline, which performs the same tasks.

In this section, you'll create a branch named code-coverage, based on
the unit-tests branch, to hold your work. In practice, you'd ordinarily
create this branch from the main branch.

1.  In Visual Studio Code, open the integrated terminal.

2.  In the terminal, run the following git checkout command to create a
    branch named code-coverage:

git checkout -B code-coverage

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image99.png)

3.  Replace  *azure-pipelines.yml* with below code

> trigger:
>
> \- '\*'
>
> pool:
>
>   name: 'Default'
>
>   demands:
>
>   - npm
>
> variables:
>
>   buildConfiguration: 'Release'
>
>   wwwrootDir: 'Tailspin.SpaceGame.Web/wwwroot'
>
>   dotnetSdkVersion: '8.x'
>
> steps:
>
> \- task: UseDotNet@2
>
>   displayName: 'Use .NET SDK $(dotnetSdkVersion)'
>
>   inputs:
>
>     version: '$(dotnetSdkVersion)'
>
> \- task: Npm@1
>
>   displayName: 'Run npm install'
>
>   inputs:
>
>     verbose: false
>
> \- script: './node_modules/.bin/node-sass $(wwwrootDir) --output
> $(wwwrootDir)'
>
>   displayName: 'Compile Sass assets'
>
> \- script: 'npx gulp'
>
>   displayName: 'Run gulp tasks'
>
> \- script: 'echo "$(Build.DefinitionName), $(Build.BuildId),
> $(Build.BuildNumber)" \> buildinfo.txt'
>
>   displayName: 'Write build info'
>
>   workingDirectory: $(wwwrootDir)
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Restore project dependencies'
>
>   inputs:
>
>     command: 'restore'
>
>     projects: '\*\*/\*.csproj'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Build the project - $(buildConfiguration)'
>
>   inputs:
>
>     command: 'build'
>
>     arguments: '--no-restore --configuration $(buildConfiguration)'
>
>     projects: '\*\*/\*.csproj'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Install .NET tools from local manifest'
>
>   inputs:
>
>     command: custom
>
>     custom: tool
>
>     arguments: 'restore'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Run unit tests - $(buildConfiguration)'
>
>   inputs:
>
>     command: 'test'
>
>     arguments: '--no-build --configuration $(buildConfiguration)
> /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura
> /p:CoverletOutput=$(Build.SourcesDirectory)/TestResults/Coverage/'
>
>     publishTestResults: true
>
>     projects: '\*\*/\*.Tests.csproj'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Create code coverage report'
>
>   inputs:
>
>     command: custom
>
>     custom: tool
>
>     arguments: 'run reportgenerator
> -reports:$(Build.SourcesDirectory)/\*\*/coverage.cobertura.xml
> -targetdir:$(Build.SourcesDirectory)/CodeCoverage
> -reporttypes:HtmlInline_AzurePipelines'
>
> \- task: PublishCodeCoverageResults@1
>
>   displayName: 'Publish code coverage report'
>
>   inputs:
>
>     codeCoverageTool: 'cobertura'
>
>     summaryFileLocation:
> '$(Build.SourcesDirectory)/\*\*/coverage.cobertura.xml'
>
> \- task: DotNetCoreCLI@2
>
>   displayName: 'Publish the project - $(buildConfiguration)'
>
>   inputs:
>
>     command: 'publish'
>
>     projects: '\*\*/\*.csproj'
>
>     publishWebProjects: false
>
>     arguments: '--no-build --configuration $(buildConfiguration)
> --output $(Build.ArtifactStagingDirectory)/$(buildConfiguration)'
>
>     zipAfterPublish: true
>
> \- task: PublishBuildArtifacts@1
>
>   displayName: 'Publish Artifact: drop'
>
>   condition: succeeded()

4.  

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image100.png)

5.  Add and commit the *Tailspin.SpaceGame.Web.Tests.csproj* file, which
    now contains a reference to the coverlet.msbuild package:

git add Tailspin.SpaceGame.Web.Tests/Tailspin.SpaceGame.Web.Tests.csproj

git commit -m "Add coverlet.msbuild package"

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image101.png)

6.  Add and commit the tool manifest file, *dotnet-tools.json*:

git add .config/dotnet-tools.json

git commit -m "Add code coverage"

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image102.png)

7.  Add and commit *azure-pipelines.yml*, which contains your updated
    build configuration:

git add azure-pipelines.yml

git commit -m "Add code coverage"

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image103.png)

8.  Push the code-coverage branch to GitHub.

git push origin code-coverage

![A computer screen shot of text AI-generated content may be
incorrect.](./media/image104.png)

9.  In Azure Pipelines, trace the build through each of the steps.\\

> ![](./media/image105.png)

10. Click on running pipeline

> ![](./media/image106.png)

11. Click on the running job.

![](./media/image107.png)

12. Wait for the job to complete successfully and then click back arrow
    to navigate to summary page.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image108.png)

![](./media/image109.png)

13. When the build finishes, go back to the Summary page and select
    the **Code Coverage** tab.You view the same results that you did
    when you ran the tests locally.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image110.png)

### Task 3 : Add the dashboard widget

In the previous section, you added the **Test Results Trend** widget to
your dashboard, which lets others quickly review test result trends over
time.

Here, you'll add a second widget that summarizes code coverage.

1.  In a new browser tab, go
    to [marketplace.visualstudio.com](https://marketplace.visualstudio.com/).On
    the **Azure DevOps** tab, search for **code coverage**.

2.  Select **Code Coverage Widgets** (published by Shane Davis).

![](./media/image111.png)

3.  Select **Get it free**.

![](./media/image112.png)

4.  In the drop-down list, select your Azure DevOps
    organization.Select **Install**.

![](./media/image113.png)

![](./media/image114.png)

5.  Go back to Azure DevOps.Go
    to **Overview** \> **Dashboards**.Select **Edit**.

![](./media/image115.png)

6.  Search for **Code Coverage**, and then select **Code Coverage**.
    Select it and then click on **Add**.

![](./media/image116.png)

7.  Select the **Gear** icon to configure the widget.

![](./media/image117.png)

8.  Keep all the default settings, except for:

    - Width: Enter **2**

    &nbsp;

    - Build definition: Select your pipeline

    &nbsp;

    - Coverage measurement: select **Lines**

    &nbsp;

    - Select **Save**.

![](./media/image118.png)

9.  Select **Done Editing**.The widget shows the percentage of code your
    unit tests cover.

![](./media/image119.png)

## Exercise 6 - Clean up your Azure DevOps environment

You're all done with the tasks for this module. You can now move the
work item to the **Done** state on Microsoft Azure Boards and clean up
your Microsoft Azure DevOps environment.

**Task 1: Delete the Azure DevOps project**

This option deletes your Azure DevOps project, including what's on Azure
Boards and your build pipeline. In future modules, you'll be able to run
another template that brings up a new project in a state where this one
leaves off. Choose this option to delete your Azure DevOps project if
you don't need it for future reference.

To delete the project:

1.  In Azure DevOps, go to your project. Earlier, we recommended that
    you name this project **Space Game - web - Tests**.

2.  Select **Project settings** in the lower-left corner of the Azure
    Devops page.

![](./media/image120.png)

3.  In the **Project details** area, scroll down, and then
    select **Delete**.

4.  In the window that appears, enter the project name, and then
    select **Delete** a second time.

> ![](./media/image121.png)
