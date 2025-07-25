# Lab 05 - Migrating CI/CD Flows from Azure DevOps to GitHub Actions

## Exercise 1 - Set up your environment

In this lab, we are integrating a DevOps strategy into the processes. In
this section, you make sure that your environment reflects the team's
efforts so far.

To do this, you:

- Add a user to ensure Azure DevOps can connect to your Azure
  subscription.

- Set up an Azure DevOps project for this module.

- Add the build pipeline.

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
    unique, replace XXX with number)enter the characters shown in your
    screen and then click on **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

7.  In the top right corner of the screen, click **User settings**.
    Click Personal access tokens.

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

    - **GitHub Connections: Read & manage**

    - **Deployment Groups : write, & manage**

    - **Pipeline Resources: Use and manage**

    - **Pull Request Threads: Read & write**

    - Project and Team: **Read, write, & manage**

    - Release: **Read, write, execute, & manage**

    - Service Connections: **Read, query, & manage**

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

2.  Choose **Agent pools**. Select the **Default** pool.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  Select the **Agents** tab, and choose **New agent**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  On the **Get the agent** dialog box, choose **Windows**. On the left
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
is **required**. You must not use [**Windows PowerShell
ISE**](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/ise/introducing-the-windows-powershell-ise) to
configure the agent.

** Important**

For security reasons we strongly recommend making sure the agents folder
(C:\agents) is only editable by admins.

** Note**

Please avoid using mintty based shells, such as git-bash, for agent
configuration. Mintty is not fully compatible with native Input/Output
Windows API
([**here**](https://github.com/mintty/mintty/wiki/Tips#inputoutput-interaction-with-alien-programs) is
some info about it) and we can't guarantee the setup script will work
correctly in this case.

### Task 3 : Configure and run the agent

1.  Start an elevated (PowerShell) window and set the location to where
    you unpacked the agent.

cd “C:\agents“

2.  Run config.cmd. This will ask you a series of questions to configure
    the agent. When setup asks for your server URL, for Azure DevOps
    Services, answer https://dev.azure.com/{your-organization}.

.\config.cmd

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image15.png)

3.  The authentication method used for registering the agent is used
    only during agent registration. Press Enter accepting the
    Authentication method is PAT when prompts for -**Enter
    authentication type (press enter for PAT) and then enter your PAT
    and press Enter.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

4.  Press Enter when prompts “Enter agent pool (press enter for
    default)”

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

5.  Type Agent name of your choice and press Enter

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

6.  Press Enter when prompt “Enter work folder (press enter for \_work)”

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image19.png)

7.  Just Enter when prompt s Enter run agent as service? (Y/N) (press
    enter for N) and Enter configure autologin and run agent on startup?
    (Y/N) (press enter for N)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

8.  Run the following the command to start the agent.

.\run.cmd

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image21.png)

9.  Minimize the window and continue with next tasks.

Note : To restart the agent, press Ctrl+C to stop the agent, and then
run **run.cmd** to restart it.

## Exercise 2 - Set up your Azure DevOps environment

In this section, you'll ensure that your Microsoft Azure DevOps
organization is set up to complete the rest of this module.

The modules in this learning path form a progression, in which you
follow the Tailspin web team through its DevOps journey.

### Task 1 : Get the Azure DevOps project

Here, you ensure that your Azure DevOps organization is set up to
complete the rest of this module. You do this by running a template that
creates a project for you in Azure DevOps.

1.  Open Visual Studio from Desktop and sign in with your accounts.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

2.  Click on **Clone a repository**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

3.  Enter Repository location as
    <https://github.com/microsoft/AzDevOpsDemoGenerator.git> and select
    location and then click on **Clone**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A computer screen shot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  **Set ADOGenerator as the Startup Project** In Visual Studio.
    Right-click on the ADOGenerator project in the Solution Explorer.
    Select **Set as Startup Project**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image26.png)

5.  **Build the Solution** Build the solution to ensure all dependencies
    are restored and the project compiles successfully.

6.  In Visual Studio, **right-click on the solution in the Solution
    Explorer** and select **Build Solution.**

> Note : Alternatively, you can use the command line: dotnet build

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

7.  Wait for the build to complete.

![A computer screen shot of a computer AI-generated content may be
incorrect.](./media/image28.png)

8.  Run the Project To run the project as a console application. In
    Visual Studio**, press F5** or click on the **Start** button.

Note : Alternatively, you can run the project from the command line:

> **dotnet run --project src/ADOGenerator/ADOGenerator.csproj**
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image29.png)

9.  Enter **1** (Create a new project….)when prompted for options

![A computer screen with text on it AI-generated content may be
incorrect.](./media/image30.png)

10. When prompted to **Enter the template number from the list of
    templates**, enter **29** for **Create a release pipeline with Azure
    Pipelines**, then press **Enter**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image31.png)

11. Choose your authentication method as Personal Access Token (PAT).
    Type 2 and press Enter.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

** Note**

If you set up a PAT, make sure to authorize the
necessary [**scopes**](https://learn.microsoft.com/en-us/azure/devops/integrate/get-started/authentication/oauth#scopes).
For this module, you can use **Full access**, but in a real-world
situation, you should ensure you grant only the necessary scopes.

12. Enter your Azure DevOps organization name (eg :**tffabrikanXX**),
    then press **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

13. Enter your Azure DevOps PAT, then press **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

14. Enter a project name such as ***Space Game - web - Release***, then
    press **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image36.png)

15. Once your project is created, go to your Azure DevOps organization
    in your browser
    (at https://dev.azure.com/\<your-organization-name\>/) and select
    the project.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

### Task 2 : Fetch the branch from GitHub

1.  On GitHub, go to the mslearn-tailspin-spacegame-web-deploy
    - <https://github.com/MicrosoftDocs/mslearn-tailspin-spacegame-web-deploy>
    repository.

2.  Select **Fork** at the top-right of the screen.

![](./media/image38.png)

3.  Choose your GitHub account as the Owner, then select **Create
    fork**.

![](./media/image39.png)

4.  Select Code, and then, from the HTTPS tab, select the copy button to
    copy the URL to your clipboard.

![](./media/image40.png)

5.  In Visual Studio Code, go to the terminal window you opened earlier.

6.  Update below command with your forked repo link and run the git
    clone command.

> git clone \<\<your forked repo link\>\>
>
> ![A computer screen shot of a black screen AI-generated content may be
> incorrect.](./media/image41.png)

7.  Open the cloned project.

![](./media/image42.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

8.  Open Terminal and run below command to go the folder

cd mslearn-tailspin-spacegame-web-deploy

9.  A *remote* is a Git repository where team members collaborate (like
    a repository on GitHub). Here you list your remotes and add a remote
    that points to Microsoft's copy of the repository so that you can
    get the latest sample code.

10. Run the following command to list your remotes:

git remote -v

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image45.png)

11. Origin specifies your repository on GitHub. When you fork code from
    another repository, the original remote (the one you forked from) is
    commonly named upstream.

12. Run the following command to create a remote named *upstream* that
    points to the Microsoft repository:

> git remote add upstream
> https://github.com/MicrosoftDocs/mslearn-tailspin-spacegame-web-deploy.git
>
> git remote -v

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image46.png)

13. Fetch and checkout release-pipeline

> git fetch microsoftdocs release-pipeline
>
> git checkout -b release-pipeline microsoftdocs/release-pipeline

![](./media/image47.png)

14. Run the following commands in new terminal to fetch
    the *release-pipeline* branch from the MicrosoftDocs repository, and
    check out a new branch *upstream/release-pipeline*.

git fetch upstream release-pipeline

git checkout -B release-pipeline upstream/release-pipeline

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

### Task 3 : Run the pipeline

Next, you'll manually trigger the pipeline to run. This step ensures
that your project is set up to build from your GitHub repository. The
initial pipeline configuration builds the application and produces a
builds artifact.

1.  Navigate to your project in Azure Devops, and then select project.

![](./media/image49.png)

2.  Click on Pipelines from left navigation menu, select the pipeline
    and click **Edit** button as shown in below image

![](./media/image50.png)

3.  Select **release-pipelines** branch. Replace the
    **Azure-pipeline.yml** code with below code and then click on
    **Validate and save**.

> trigger:
>
> \- '\*'
>
> pool:
>
>   name: 'Default'  # Self-hosted agent pool
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
>     packageType: sdk
>
>     version: '$(dotnetSdkVersion)'
>
> \- task: Npm@1
>
>   displayName: 'Run npm install'
>
>   inputs:
>
>     command: 'install'
>
>     verbose: false
>
> \- powershell: |
>
>     $scssPath = "$(wwwrootDir)/scss"
>
>     if (Test-Path $scssPath) {
>
>       Write-Host "SCSS directory found. Compiling..."
>
>       npx sass $scssPath:$(wwwrootDir)/css
>
>     } else {
>
>       Write-Host "SCSS directory not found. Skipping Sass
> compilation."
>
>     }
>
>   displayName: 'Compile Sass assets'
>
> \- script: 'npx gulp'
>
>   displayName: 'Run gulp tasks'
>
>   workingDirectory: Tailspin.SpaceGame.Web
>
> \- script: |
>
>     echo "$(Build.DefinitionName), $(Build.BuildId),
> $(Build.BuildNumber)" \> buildinfo.txt
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
>   inputs:
>
>     pathToPublish: '$(Build.ArtifactStagingDirectory)'
>
>     artifactName: 'drop'
>
>     publishLocation: 'Container'

![](./media/image51.png)

4.  Click on **Save** button.

![](./media/image52.png)

5.  Click on **Run**.

![](./media/image53.png)

6.  Click on **Run** again on Run pipeline window

![](./media/image54.png)

7.  Click on **View** next to warning message “  
    **This pipeline needs permission to access a resource before this
    run can continue”**

![](./media/image55.png)

8.  Click on **Permit** -\> **Permit** button.

![](./media/image56.png)

9.  Click on Job and wait for the job is successful.

![](./media/image57.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.png)

10. After the build finishes, select the back button to return to the
    summary page.

![](./media/image60.png)

11. Select your published artifact.

![](./media/image61.png)

12. The ***Tailspin.Space.Game.Web.zip*** is your build artifact. This
    file contains your built application and its dependencies.

> ![](./media/image62.png)

13. Select **Triggers-\> trailspin-spacegame-web-deploy**, select
    **Override the YAML continuous integration trigger from here** check
    box and then select **release-pipeline** branch as branch
    specification.

> ![](./media/image63.png)

14. Select **YAML-\>Pipeline**. Select **Default** from **Default agent
    pool for YAML.**

> ![](./media/image64.png)

15. Click on **Get sources** and select **GitHub** and then click on
    **Authorize using** **OAuth**. Sign in with your GitHub account.

![](./media/image65.png)

16. Click on Repository selection button, select
    **username/mslearn-tailspin-spacegame-web** and then click on
    **Select** button.

![](./media/image66.png)

17. Select **Save & queue -\> Save.**

![](./media/image67.png)

18. Type comment and then click on **Save** button.

![](./media/image68.png)

19. Click on Pipeline from left navigation menu and then select pipeline
    to **Edit**.

![](./media/image69.png)

20. Replace the **azure-pipeline.yml** code with below code and then
    click on **Validate and save** button.

> trigger:
>
> \- '\*'
>
> pool:
>
>   name: 'Default'  # Self-hosted agent pool
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
>     packageType: sdk
>
>     version: '$(dotnetSdkVersion)'
>
> \- task: Npm@1
>
>   displayName: 'Run npm install'
>
>   inputs:
>
>     command: 'install'
>
>     verbose: false
>
> \- powershell: |
>
>     $scssPath = "$(wwwrootDir)/scss"
>
>     if (Test-Path $scssPath) {
>
>       Write-Host "SCSS directory found. Compiling..."
>
>       npx sass $scssPath:$(wwwrootDir)/css
>
>     } else {
>
>       Write-Host "SCSS directory not found. Skipping Sass
> compilation."
>
>     }
>
>   displayName: 'Compile Sass assets'
>
> \- script: 'npx gulp'
>
>   displayName: 'Run gulp tasks'
>
>   workingDirectory: Tailspin.SpaceGame.Web
>
> \- script: |
>
>     echo "$(Build.DefinitionName), $(Build.BuildId),
> $(Build.BuildNumber)" \> buildinfo.txt
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
>   inputs:
>
>     pathToPublish: '$(Build.ArtifactStagingDirectory)'
>
>     artifactName: 'drop'
>
>     publishLocation: 'Container'
>
> ![](./media/image70.png)

21. Click on **Save** now once the pipeline is validated.

![](./media/image71.png)

22. Click on Run button to run the pipeline.

![](./media/image72.png)

23. Keep the default values and then click on Run.

![](./media/image73.png)

24. Click on **View** next to the warning message”  
    **This pipeline needs permission to access a resource before this
    run can continue”**

![](./media/image74.png)

25. Click on **Permit-\> Permit** to grant permissions to run pipeline.

![](./media/image75.png)

26. Click on the **Job**.

![](./media/image76.png)

27. Wait for the job to run completely.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image77.png)

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image78.png)

28. After the build finishes, select the back button to return to the
    summary page.

![](./media/image79.png)

29. Select your published artifact.

![](./media/image80.png)

30. The ***Tailspin.Space.Game.Web.zip*** is your build artifact. This
    file contains your built application and its dependencies.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image81.png)

You now have a build pipeline for the Space Game web project. Next,
you'll add a deployment stage to deploy your build artifact to Azure App
Service.

## Exercise 3 - Deploy the web application to Azure App Service

In this exercise, you create a multistage pipeline to build and deploy
your application to Azure App Service. You learn how to:

- Create an App Service instance to host your web application.

- Create a multistage pipeline.

- Deploy to Azure App Service.

### **Task 1: Create the App Service instance**

1.  Open a new tab in your browser and go to https://portal.azure.com
    and sign in with your Azure subscription.

2.  Search for  **App Services** and select it.

![](./media/image82.png)

3.  Select **Create** \> **Web App** to create a new Web App.

![](./media/image83.png)

4.  On the **Basics** tab, enter the following values. Select **Review +
    create**.

[TABLE]

> ![](./media/image84.png)
>
> ![](./media/image85.png)

5.  Select **Create**.

![](./media/image86.png)

6.  The deployment takes a few moments to complete

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image87.png)

7.  When deployment is complete, select **Go to resource**. The **App
    Service** Essentials displays details related to your deployment.

![](./media/image88.png)

8.  Select the URL to verify the status of your App Service.

![](./media/image89.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.png)

### Task 2 : Create a service connection

1.  Switch back to Azure DevOps tab, go to your **Space Game - web -
    Release** project.

![](./media/image91.png)

2.  From the lower-left corner of the page, select **Project settings**.

![](./media/image92.png)

3.  Under **Pipelines**, select **Service connections**. Select **Create
    service connection**.

![](./media/image93.png)

4.  Select **Azure Resource Manager** then select **Next**.

![](./media/image94.png)

5.  Fill out the required fields as follows: If prompted, sign in to
    your Microsoft account.

    - Scope level : Subscription

    - Subscription : Your Azure subscription

    - Resource Group : **select your resource group.**

    - Service connection name : ***Resource Manager - Tailspin - Space
      Game***

> ![](./media/image95.png)

6.  Ensure that **Grant access permission to all pipelines** is
    selected. Select **Save**.

> ![](./media/image96.png)

7.  Click on **Service connections** from left navigation menu and then
    click on **New service connection.**

![](./media/image97.png)

8.  Select **GitHub** and then click on **Next**.

![](./media/image98.png)

9.  Select **OAuth Configuration ; AzurePipelines** and then click on
    **Authorize** button. Sign in with your GitHub account.

![](./media/image99.png)

10. Keep the default connection name and select **Grant access
    permission to all pipelines** check box and then click on **Save**
    button.

![](./media/image100.png)

11. Select the project name from top navigation menu.

![](./media/image101.png)

### Task 3 : Add the Build stage to your pipeline

A *multistage pipeline* allows you to define distinct phases that your
change passes through as it's promoted through the pipeline. Each stage
defines the agent, variables, and steps required to carry out that phase
of the pipeline. In this section, you define one stage to perform the
build. You define a second stage to deploy the web application to App
Service.

To convert your existing build configuration to a multistage pipeline,
you add a stages section to your configuration, and then you add one or
more stage sections for each phase of your pipeline. Stages break down
into jobs, which are a series of steps that run sequentially as a unit.

1.  Click on **Pipeline** from left navigation menu and then select
    three dots and select **Edit**

![](./media/image102.png)

2.  Select **release-pipeline** branch and click next to **Run** button
    and then select **Triggers**.

![](./media/image103.png)

3.  Select **Triggers-\> tailspin-spacegame-web-deploy** , select
    **Override the YAML continuous integration trigger from here**
    checkbox and then include **release-pipeline** branch.

![](./media/image104.png)

4.  Click on **YAML** tab and select **Default** agent from **Default
    agent pool for YAML** drop down.

> ![](./media/image105.png)

5.  Select **Get sources**, Select a source **GitHub** and then click on
    **Repository**.

![](./media/image106.png)

6.  Select **mslearn-tailspin-spacegame-web-deploy** and then click on
    **Select** button.

![](./media/image107.png)

7.  Click on **Save & queue** drop down and select **Save** button.

![](./media/image108.png)

8.  Enter the comment and then **Save**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image109.png)

9.  Switch back to Visual Studio code and replace the code
     ***azure-pipelines.yml*** with below code **save**:

> trigger:
>
> \- release-pipeline
>
> variables:
>
>   buildConfiguration: 'Release'
>
>   wwwrootDir: 'Tailspin.SpaceGame.Web/wwwroot'
>
>   dotnetSdkVersion: '8.x'
>
> stages:
>
> \- stage: Build
>
>   displayName: 'Build the web application'
>
>   jobs:
>
>   - job: BuildJob
>
>     displayName: 'Build job'
>
>     pool:
>
>       name: 'Default'  
>
>     steps:
>
>     - task: UseDotNet@2
>
>       displayName: 'Use .NET SDK $(dotnetSdkVersion)'
>
>       inputs:
>
>         packageType: sdk
>
>         version: '$(dotnetSdkVersion)'
>
>     - task: Npm@1
>
>       displayName: 'Run npm install'
>
>       inputs:
>
>         command: 'install'
>
>         verbose: false
>
>     - powershell: |
>
>         $scssPath = "$(wwwrootDir)/scss"
>
>         if (Test-Path $scssPath) {
>
>           Write-Host "SCSS directory found. Compiling..."
>
>           npx sass $scssPath:$(wwwrootDir)/css
>
>         } else {
>
>           Write-Host "SCSS directory not found. Skipping Sass
> compilation."
>
>         }
>
>       displayName: 'Compile Sass assets'
>
>     - script: 'npx gulp'
>
>       displayName: 'Run gulp tasks'
>
>       workingDirectory: Tailspin.SpaceGame.Web
>
>     - script: |
>
>         echo "$(Build.DefinitionName), $(Build.BuildId),
> $(Build.BuildNumber)" \> buildinfo.txt
>
>       displayName: 'Write build info'
>
>       workingDirectory: $(wwwrootDir)
>
>     - task: DotNetCoreCLI@2
>
>       displayName: 'Restore project dependencies'
>
>       inputs:
>
>         command: 'restore'
>
>         projects: '\*\*/\*.csproj'
>
>     - task: DotNetCoreCLI@2
>
>       displayName: 'Build the project - $(buildConfiguration)'
>
>       inputs:
>
>         command: 'build'
>
>         arguments: '--no-restore --configuration
> $(buildConfiguration)'
>
>         projects: '\*\*/\*.csproj'
>
>     - task: DotNetCoreCLI@2
>
>       displayName: 'Publish the project - $(buildConfiguration)'
>
>       inputs:
>
>         command: 'publish'
>
>         projects: '\*\*/\*.csproj'
>
>         publishWebProjects: false
>
>         arguments: '--no-build --configuration $(buildConfiguration)
> --output $(Build.ArtifactStagingDirectory)/$(buildConfiguration)'
>
>         zipAfterPublish: true
>
>     - task: PublishBuildArtifacts@1
>
>       displayName: 'Publish Artifact: drop'
>
>       inputs:
>
>         pathToPublish: '$(Build.ArtifactStagingDirectory)'
>
>         artifactName: 'drop'
>
>         publishLocation: 'Container'

10. 

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image110.png)

11. From the integrated terminal, run the following commands to stage,
    commit, and then push your changes to your remote branch.

> git add azure-pipelines.yml
>
> git commit -m "Add a build stage"

git push origin release-pipeline

> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image111.png)

12. In Azure Pipelines, navigate to your pipeline to view the logs.

![](./media/image112.png)

13. Select the pipeline run.

![](./media/image113.png)

14. Click on **Build job**.

![](./media/image114.png)

15. Wait for the build to complete successful.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image115.png)

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image116.png)

16. After the build finishes, select the back button to return to the
    summary page and check the status of your pipeline and published
    artifact.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image117.png)

### Task 4 : Create the dev environment and Store your web app name in a pipeline variable

An *environment* is an abstract representation of your deployment
environment. You can use environments to define specific criteria for
your release, such as which pipeline is authorized to deploy to the
environment. You can also use environments to set up manual approvals
for specific user/group to approve before deployment is resumed.

The *Deploy* stage that we're creating uses the name to identify to
which App Service instance to deploy; for
example, *tailspin-space-game-web-1234*.

Although you could hard-code this name in your pipeline configuration,
defining it as a variable makes your configuration more reusable.

1.  From Azure Pipelines, select **Environments**.Make sure Dev
    environment got created .

> ![](./media/image118.png)

2.  In Azure DevOps, select **Pipelines** and then
    select **Library**.Select **+ Variable group** to create a new
    variable group.

![](./media/image119.png)

3.  Enter ***Release*** for the **variable group
    name**.Select **Add** under **Variables** to add a new variable.

> ![](./media/image120.png)

4.  Enter ***WebAppName*** for the variable name and your App Service
    instance's name for its value: for
    example, ***tailspin-space-game-web-***.(Webapp name you created in
    previous task-2s).Select **Save**.

![](./media/image121.png)

### Task 5 : Add the deployment stage to your pipeline

We extend our pipeline by adding a deployment stage to deploy the *Space
Game* to App Service using the download and AzureWebApp@1 tasks to
download the build artifact and then deploy it.

1.  From Visual Studio Code, replace the contents
    of ***azure-pipelines.yml*** with the following yaml:

> trigger:
>
> \- '\*'
>
> variables:
>
>   buildConfiguration: 'Release'
>
>   wwwrootDir: 'Tailspin.SpaceGame.Web/wwwroot'
>
>   dotnetSdkVersion: '8.x'
>
> stages:
>
> \- stage: 'Build'
>
>   displayName: 'Build the web application'
>
>   jobs:
>
>   - job: 'Build'
>
>     displayName: 'Build job'
>
>     pool:
>
>       name: 'Default'  # Self-hosted agent pool
>
>     steps:
>
>     - task: UseDotNet@2
>
>       displayName: 'Use .NET SDK $(dotnetSdkVersion)'
>
>       inputs:
>
>         packageType: sdk
>
>         version: '$(dotnetSdkVersion)'
>
>     - task: Npm@1
>
>       displayName: 'Run npm install'
>
>       inputs:
>
>         command: 'install'
>
>         verbose: false
>
>     - powershell: |
>
>         $scssPath = "$(wwwrootDir)/scss"
>
>         if (Test-Path $scssPath) {
>
>           Write-Host "SCSS directory found. Compiling..."
>
>           npx sass $scssPath:$(wwwrootDir)/css
>
>         } else {
>
>           Write-Host "SCSS directory not found. Skipping Sass
> compilation."
>
>         }
>
>       displayName: 'Compile Sass assets'
>
>     - script: 'npx gulp'
>
>       displayName: 'Run gulp tasks'
>
>       workingDirectory: Tailspin.SpaceGame.Web
>
>     - script: |
>
>         echo "$(Build.DefinitionName), $(Build.BuildId),
> $(Build.BuildNumber)" \> buildinfo.txt
>
>       displayName: 'Write build info'
>
>       workingDirectory: $(wwwrootDir)
>
>     - task: DotNetCoreCLI@2
>
>       displayName: 'Restore project dependencies'
>
>       inputs:
>
>         command: 'restore'
>
>         projects: '\*\*/\*.csproj'
>
>     - task: DotNetCoreCLI@2
>
>       displayName: 'Build the project - $(buildConfiguration)'
>
>       inputs:
>
>         command: 'build'
>
>         arguments: '--no-restore --configuration
> $(buildConfiguration)'
>
>         projects: '\*\*/\*.csproj'
>
>     - task: DotNetCoreCLI@2
>
>       displayName: 'Publish the project - $(buildConfiguration)'
>
>       inputs:
>
>         command: 'publish'
>
>         projects: '\*\*/\*.csproj'
>
>         publishWebProjects: false
>
>         arguments: '--no-build --configuration $(buildConfiguration)
> --output $(Build.ArtifactStagingDirectory)/$(buildConfiguration)'
>
>         zipAfterPublish: true
>
>     - task: PublishBuildArtifacts@1
>
>       displayName: 'Publish Artifact: drop'
>
>       inputs:
>
>         pathToPublish: '$(Build.ArtifactStagingDirectory)'
>
>         artifactName: 'drop'
>
>         publishLocation: 'Container'
>
> \- stage: 'Deploy'
>
>   displayName: 'Deploy the web application'
>
>   dependsOn: Build
>
>   jobs:
>
>   - deployment: Deploy
>
>     pool:
>
>       name: 'Default'  # Self-hosted agent pool
>
>     environment: dev
>
>     variables:
>
>     - group: Release
>
>     strategy:
>
>       runOnce:
>
>         deploy:
>
>           steps:
>
>           - download: current
>
>             artifact: drop
>
>           - task: AzureWebApp@1
>
>             displayName: 'Azure App Service Deploy: website'
>
>             inputs:
>
>               azureSubscription: 'Resource Manager - Tailspin - Space
> Game'
>
>               appName: '$(WebAppName)'
>
>               package:
> '$(Pipeline.Workspace)/drop/$(buildConfiguration)/\*.zip'

2.  

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image122.png)

Notice the highlighted section and how we're using
the download and AzureWebApp@1 tasks. The pipeline fetches
the $(WebAppName) from the variable group we created earlier.

Also notice how we're using environment to deploy to
the **dev** environment.

3.  From the integrated terminal, add *azure-pipelines.yml* to the
    index. Then commit the change and push it up to GitHub.

git add azure-pipelines.yml

git commit -m "Add a deployment stage"

git push origin release-pipeline

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image123.png)

4.  In Azure Pipelines, navigate to your pipeline to view the logs.

![](./media/image124.png)

5.  Select deployment stage.

![](./media/image125.png)

6.  Wait for the **Build** to complete.You will have manually permit
    Deployment to initiate the process

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image126.png)

7.  Once build is successfully then click on View next to the warning
    message “  
    **This pipeline needs permission to access 2 resources before this
    run can continue to Deploy the web application”**

![](./media/image127.png)

8.  Review and allow both permissions.

![](./media/image128.png)

9.  Wait for the deployment to complete.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image129.png)

10. After the build finishes, select the back button to return to the
    summary page and check the status of your stages. Both stages
    finished successfully in our case.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image130.png)

### Task 6 : View the deployed website on App Service

1.  Switch back to App Service tab open, refresh the page. Otherwise,
    navigate to your Azure App Service in the Azure portal and select
    the instance's **URL**: for
    example, ***https://tailspin-space-game-web-XXXXX.azurewebsites.net***

2.  The *Space Game* website is successfully deployed to Azure App
    Service.

![Screenshot of web browser showing the Space Game
website.](./media/image131.png)

## Exercise 4 - Clean up your environment

Congratulations! You're all done with creating pipelines and resources.
In this unit, you're going to clean up your Azure resources and Azure
DevOps environment.

### Task 1 : Clean up Azure resources

1.  Navigate to [Azure portal](https://portal.azure.com/).

2.  Select **Resource groups** from the left panel.

3.  Select your resource group (**tailspin-space-game-rg**).

4.  Select **Delete resource group**.

5.  Type your resource group name in the text box, and then
    select **Delete**.

6.  Select **Delete** again to confirm deletion.

### Task 2: Disable your pipeline

Each module in this learning path provides a template you can run to
create a clean environment. When you run multiple templates, you will
create multiple projects each pointing to the same GitHub repository.
This setup can trigger multiple pipelines to run every time you push a
change to your GitHub repository consuming free build minutes on our
hosted agents. That's why it's important to disable or delete your
pipeline before you move on to the next module in this learning path.

Choose this option if you want to keep your project and your build
pipeline for future reference. You can re-enable your pipeline later if
you need to.

1.  In your Azure DevOps project, select **Pipelines** and then select
    your pipeline.

2.  Select the ellipsis button at the far right, and then
    select **Settings**.

![A screenshot of Azure Pipelines showing the location of the Settings
menu.](./media/image132.png)

3.  Select **Disabled**, and then select **Save**. Your pipeline will no
    longer process new run requests.

### Task 3 : Delete your project

Choose this option if you don't need your DevOps project for future
reference. This will delete your Azure DevOps project.

1.  Navigate to your Azure DevOps project.

2.  Select **Project settings** in the lower-left corner.

3.  Under **Overview**, scroll down to the bottom of the page and then
    select **Delete**.

![A screenshot of Azure Pipelines showing the location of the Delete
button.](./media/image133.png)

4.  Type your project name in the text box, and then select **Delete**.

## Summary

Great work! You've successfully deployed the *Space Game* website to
Azure App Service by using Azure Pipelines. Continuous delivery helps
you release reliable software updates to your customers as rapidly as
the business demands. Your customers can have the latest features and
fixes as soon as you're ready to release them. By using the analytics
provided by Azure Pipelines, your team can pinpoint hot spots and areas
for improvement.

Setting up Azure Pipelines to publish the *Space Game* website to App
Service is a great first step for the team, but this accomplishment is
just the beginning. In the modules that follow, you'll help the team
expand their release pipeline to include additional stages and tests.
Each improvement will give the team more confidence in the quality of
their releases.
