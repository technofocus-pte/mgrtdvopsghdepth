# Lab 07 - Enable GitHub Collaboration and Code Review Workflows with Azure Pipelines

**Objectives** 

By the end of this lab, you will:

- Enable GitHub branch protections to require Pull Request reviews
  before merge.

- Practice creating Pull Requests from a feature/release branch to main
  in GEC.

- Perform code reviews and approvals in GitHub Enterprise Cloud.

- Integrate Azure Pipelines so that PR validation builds run
  automatically.

- Understand how PR-driven workflows improve code quality, security, and
  compliance.

**Note for this lab**

All code changes will be merged to main **only via Pull Requests**.  
We will not push changes directly to main from the pipeline — this
reinforces review culture and simulates real-world governance where
branch policies protect production code.

## Exercise 1 - Set up your Azure DevOps environment

In this section, you'll ensure that your Microsoft Azure DevOps
organization is set up to complete the rest of this module.

The modules in this learning path form a progression, in which you
follow the Tailspin web team through its DevOps journey.

### Task 1: Move the work item to Doing

Here, you assign a work item to yourself on Azure Boards. You also move
the work item to the Doing state. In practice, you and your team would
create work items at the start of each *sprint* or work iteration.

This work assignment gives you a checklist from which to work. It gives
other team members visibility into what you're working on and how much
work is left. The work item also helps enforce work-in-progress (WIP)
limits so that the team doesn't take on too much work at one time.

 Note: Within an Azure DevOps organization, work items are numbered
sequentially. In your project, the number for each work item might not
match what you see here.

Here you move the first item, Create a multistage pipeline, to
the Doing column. Then you assign yourself to the work item. Create a
multistage pipeline relates to defining each stage of deploying
the *Space Game* website.

1.  Switch back to your **Azure DevOps** tab, click on the existing
    project name.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image1.png)

2.  Hover on **Boards** from left navigation menu and select **Boards**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image2.png)

3.  Click on **New Item** under **To Do** column as shown in the below
    image.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image3.png)

4.  Select **Backlog Items** and then click on **New item.**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image4.png)

5.  Create the items below. Enter the item name and then click on **Add to top**.


    +++Stabilize the build server+++

    +++Create a Git-based workflow+++

    +++Create unit tests+++

    +++Check code for vulnerabilities+++

    +++Move model data to its own package+++

    +++Investigate hosted vs private build servers+++


    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image5.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image6.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image7.png)

7.  In the Create a multistage pipeline card, select the **Stabilize the
    build server**. Then, assign the work item to yourself. Move the
    work item from the **To Do** column to the **Done** column.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image8.png)

8.  In the Create a multistage pipeline card, select the **Create a
    Git-based workflow**. Then, assign the work item to yourself. Move
    the work item from the **New** column to the **Committed** column.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image9.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image10.png)

### Task 2: Fetch the branch from GitHub

1.  In Visual Studio Code, Click on **File-\> Open Folder**

2.  Navigate to **C:\Labfiles\Lab07** and open **mslearn-tailspin-spacegame-web-deploy**  

3.  Click on **Terminal -> New Terminal- > GitBash**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image11.png)

4.  Open **Terminal** and run below command to go the folder

    +++cd mslearn-tailspin-spacegame-web-deploy+++

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image12.png)

5.  A *remote* is a Git repository where team members collaborate (like a repository on GitHub). Here you list your remotes and add a remote that points to Microsoft's copy of the repository so that you can get the latest sample code.

6.  Run the following command to list your remotes:

    +++git remote -v+++

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image13.png)

7.  Origin specifies your repository on GitHub. When you fork code from
    another repository, the original remote (the one you forked from) is
    commonly named upstream.

8.  Run the following command to create a remote named *upstream* that points to the Microsoft repository:

    +++git remote add upstream https://github.com/MicrosoftDocs/mslearn-tailspin-spacegame-web-deploy.git+++

    +++git remote -v+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image14.png)

9.  To fetch a branch named release from the Microsoft repository, and
    to switch to that branch, run the following git commands.

    +++git fetch upstream release+++

    +++git checkout -B release upstream/release+++

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image15.png)

### Task 3: Create Repos in ADO

1.  Switch back to DevOps Organization page and click on the existing
    project.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image16.png)

2.  Hover on **Repos** from left navigation menu and select **Files**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image17.png)

3.  Click on the project drop down form top navigation menu and select
    **New repository**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image18.png)

4.  Enter the Repository name as  +++Lab07-tailspin-spacegame-web-deploy+++. **Unselect** "Add a README" checkbox and then click on **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image19.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image20.png)

5.  Copy the HTTPS url to use in next exercise 2

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image21.png)

### Task 4: Push Repo in Azure DevOps

1.  Replace you@example.com with your GitHub account and Your Name to be
    replaced with your GitHub account username in the below commands and
    run them.

    +++git config --global user.email "you@example.com"+++

    +++git config --global user.name "Your Name"+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image22.png)

2.  **Run below commands to push the repo.**

    +++git init+++

    +++git add .+++

    +++git commit -m "Initial commit for migration lab"+++

    ![A computer screen shot of a program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image23.png)

3.  Run below command to sign into Azure. Select Work or school account
    option and click on Continue.

    +++az login+++

    ![A screenshot of a computer screen AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image24.png)

4.  **Sign in with your cloud slice accounts.**

    - Username: +++@lab.CloudPortalCredential(User1).Username+++

    - Temporary Access Pass (TAP) Token: +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![A screenshot of a computer screen AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image25.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image26.png)

5.  Click on **Yes,all apps** button and then click on **Done.**

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image27.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image28.png)

6.  Select your subscription and then press enter.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image29.png)

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image30.png)

7.  **Run below command to push local folder to Azure Devops .(Replace **https://dev.azure.com/ADOCourseOrg04/dev-github-proj-53969426/_git/azure-search-openai-migrated **with the https link you copied in previous step -Azure DevOps repos**

    +++git remote set-url origin <Devops Repo Https url>+++

    ![A screen shot of a computer program AI-generated content may be  incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image31.png)

8.  Run below command to push the repo to Devops . if it prompts to sign in ,sign in with your DevOps account.

    +++git push -u origin --all++++

    ![A screen shot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image32.png)

    >[!note]**Note**: If you already have repo in Devops then follow steps to pull and resolve conflict
    >1. Pull from DevOps with unrelated history: **git pull origin main --allow-unrelated-histories**
    >2. Resolve conflicts in files like .gitignore, README.md.
    >3. Stage the resolved files - git add . >git commit -m "Resolved merge conflicts"

9.  Go back to Azure Devops project and check Repos-> Files .You should see your repo here .

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image33.png)

### Task 5: Migrate Azure DevOps Repo to GitHub Enterprise Cloud

1.  Switch back to Visual Studio and run below commands to set
    environment varaibles. Make sure to update variables with your
    values and then run them.

    +++export AZURE_DEVOPS_PAT="YOUR_AZDO_PAT"+++

    +++export GH_PAT="ghp_xxxxxxxxxxxxxxxxxxxxx"+++

    +++export ADO_ORG=https://dev.azure.com/your-ado-org+++ # or simply "your-ado-org" if you prefer

    +++export GEC_ORG="your-githubEC-org"+++ # e.g. devopstogtihub1234

    +++export ADO_PROJECT=" **dev-github-proj-@lab.LabInstance.Id**"+++ #ADO project 

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image34.png)

2.  Run below commands to verify GitHub login status. If not logged in
    then run 2^(nd) command gh auth login and follow the process to
    loginto GitHub.

    +++gh auth status+++

    +++gh auth login+++

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image35.png)

3.  Run below command to install GitHub Enterprise importer extension.

    +++gh extension install github/gh-ado2gh+++

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image36.png)

4.  Run below command to grant migrator role to your account.(replace
    your-github-username with your username)

    +++gh gei grant-migrator-role --github-org $GEC_ORG --actor <your-github-username> --actor-type USER+++

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image37.png)

    >Example: gh gei grant-migrator-role --github-org devopstogtihub --actor chintharlamanjula --actor-type USER

5.  Run below dry-run command to migrate repos to GEC and copy migration id **(Note: We have used repo -** **tailspin-spacegame-web-deploy . You can check this in your ADO-\>Org-\> Project-\> Repo )**

    +++gh ado2gh migrate-repo --ado-org  $ADO_ORG --ado-team-project $ADO_PROJECT --ado-repo Lab07-tailspin-spacegame-web-deploy --github-org $GEC_ORG --github-repo Lab07-tailspin-spacegame-web-deploy  --ado-pat $AZURE_DEVOPS_PAT --github-pat $GH_PAT --queue-only+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image38.png)

6.  Copy repository migration and update below command with migration id
    and run.

    +++gh ado2gh wait-for-migration  --migration-id <MIGRATION_ID> --github-pat $GH_PAT+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image39.png)

7.  Switch back to GitHub browser tab, click profile and select **Your enterprises**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image40.png)

8.  Click on your enterprise account.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image41.png)

9.  Click on **Organizations** tab and then select the organization you
    have created.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image42.png)

10.  Click on **Repositories** tab and you should see migrated repo

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image43.png)

---

## Exercise 2 : Create the Azure App Service environments

Here, you create the environments that define the pipeline stages. You
create one App Service instance for each stage: ***Dev*, *Test*,
and *Staging***.

In Create a release pipeline with Azure Pipelines, you brought up App
Service through the Azure portal. Although the portal is a great way to
explore what's available on Azure or to do basic tasks, bringing up
components such as App Service can be tedious.

In this module, you use the Azure CLI to bring up three App Service
instances. You can access the Azure CLI from a terminal or through
Visual Studio Code. Here, you access the Azure CLI from Azure Cloud
Shell. This browser-based shell experience is hosted in the cloud. In
Cloud Shell, the Azure CLI is configured for use with your Azure
subscription.

### Task 1: Create the App Service instances

Here, you create the App Service instances for the three stages you
deploy to: *Dev*, *Test*, and *Staging*. Here's a brief overview of the
process you follow:

1.  Go to the Azure portal- +++https://portal.azure.com+++  and sign in.
    From the menu, select **Cloud Shell**. When prompted, select
    the **Bash** experience.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image44.png)

2.  Select No storage account needed radio button and select your
    subscription and then click on **Apply**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image45.png)

3.  Run below command to get existing resource group and set it as
    default.

    +++az group list --output table+++

    +++az config set defaults.group=<YourResourceGroupName>+++

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image46.png)

4.  Run below command to get location of your resource group and then
    run az configure to set your default resource group region.
    Replace \<REGION\> with the region of your resource group.

    +++az configure --defaults location=centralindia+++

    ![A screenshot of a computer screen AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image47.png)

5.  From the Cloud Shell, generate a random number that makes your web
    app's domain name unique.

    +++webappsuffix=$RANDOM+++

    +++export RESOURCE_GROUP=ResourceGroup1+++

    ![A screenshot of a computer screen AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image48.png)

6.  Run below command to register Microsoft.web service provider.

    +++az provider register --namespace Microsoft.Web+++

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image49.png)

7.  To create the App Service plan named *tailspin-space-game-asp*, run
    the following az appservice plan create command.

    +++az appservice plan create --name tailspin-space-game --resource-group $RESOURCE_GROUP --sku B1 --is-linux+++

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image50.png)

    >[!alert]**Important**: This Azure subscription is limited to the SKU value B1.

8.  To create the three App Service instances, one for each environment
    (*Dev*, *Test*, and *Staging*), run the following az webapp
    create commands.

    +++az webapp create --name tailspin-space-game-web-dev-$webappsuffix --resource-group $RESOURCE_GROUP --plan tailspin-space-game --runtime "DOTNETCORE|8.0"+++

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image51.png)

    +++az webapp create --name tailspin-space-game-web-test-$webappsuffix --resource-group $RESOURCE_GROUP --plan tailspin-space-game --runtime "DOTNETCORE|8.0"+++

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image52.png)

    +++az webapp create --name tailspin-space-game-web-staging-$webappsuffix --resource-group $RESOURCE_GROUP --plan tailspin-space-game --runtime "DOTNETCORE|8.0"+++

    ![A screen shot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image53.png)

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image54.png)

    >[!note]**Note**:
    >For learning purposes, you apply the same App Service plan, B1 Basic, to
    >each App Service instance here. In practice, you'd assign a plan that
    >matches your expected workload.
    >
    >For example, for the environments that map to
    >the *Dev* and *Test* stages, B1 Basic might be appropriate because you
    >want only your team to access the environments.
    >
    >For the *Staging* environment, you'd select a plan that matches your
    >production environment. That plan would likely provide greater CPU,
    >memory, and storage resources. Under the plan, you can run performance
    >tests, like load tests, in an environment that resembles your production
    >environment. You can run the tests without affecting live traffic to
    >your site.

9.  To list each App Service instance's host name and state, run the
    following az webapp list command.

    +++az webapp list --resource-group $RESOURCE_GROUP --query "[].{hostName: defaultHostName, state: state}" --output table+++

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image55.png)

10. Go back to the Azure portal tab and click on **Resource groups**  tile.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image56.png)

11. Click on resource group name

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image57.png)

12. Click on **Dev** app service name +++tailspin-space-game-web-dev-XXXX+++ replace XXXX with unique number .

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image58.png)

13. Click on default domain link.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image59.png)

13. Default home page appears.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image60.png)

### Task 2: Create pipeline variables in Azure Pipelines

In Create a release pipeline with Azure Pipelines, you added a variable
to your pipeline that stores the name of your web app in App Service.
Here you do the same. But this time, you add one variable for each App
Service instance that corresponds to a *Dev*, *Test*, or *Staging* stage
in your pipeline.

You could hard-code these names in your pipeline configuration, but if
you define them as variables, your configuration is more reusable.
Additionally, if the names of your App Service instances change, you can
update the variables and trigger your pipeline without modifying your
configuration.

1.  In Azure DevOps, go back to your  project.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image61.png)

2.  Select **Pipelines ->** **Library** from left navigation menu.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image62.png)

3.  Select **+ Variable group**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image63.png)

4.  Under **Properties**, enter +++**Release+++** for the variable group
    name. Under **Variables**, select **+ Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image64.png)

5.  For the name of your variable, enter +++***WebAppNameDev+++***. For
    the value, enter the name of the App Service instance that
    corresponds to your *Dev* environment, such
    as **+++tailspin-space-game-web-dev-@lab.LabInstance.Id+++**.(these are your host name
    and you can also get from Azure portal-\> Resource group-\> App
    service name for dev,test and staging)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image65.png)

6.  Repeat the previous two steps twice more to create variables for
    your *Test* and *Staging* environments. Replace XXXX with your app
    number and **Save**.

    |||
    |--|--|
    |Variable name|Value|
    |+++WebAppNameTest+++|+++tailspin-space-game-web-test-@lab.LabInstance.Id+++|
    |+++WebAppNameStaging+++|+++tailspin-space-game-web-staging-@lab.LabInstance.Id+++|

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image66.png)

7.  Click on Environment from left navigation menu and click on **New
    environment**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image67.png)

8.  Enter Name as +++**test+++** and then click on **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image68.png)

9.  Repeat above step and create +++**staging+++** environment.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image69.png)

### Task 3: Create a service connection for ARM and GitHub

Here, you create a service connection that enables Azure Pipelines to
access your Azure subscription. Azure Pipelines uses this service
connection to deploy the website to App Service. You created a similar
service connection in the previous labs.

>[!alert]**Important**: Make sure that you're signed in to both the Azure portal and Azure DevOps under the same Microsoft account.

1.  From the lower-left corner of the page, select **Project settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image70.png)

2.  Under **Pipelines**, select **Service connections**. Select **Create
    service connection**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image71.png)

3.  Select **Azure Resource Manager**, and then select **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image72.png)

4.  Fill in these fields. Then, select **Grant access permission to all
    pipelines** check box and then click on **Save**.

    - Scope level : **Subscription**
    - Subscription : @lab.CloudSubscription.Id
    - Resource Group : **ResourceGroup1**
  
    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image73.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image74.png)

5.  Click on **Service connections** from left navigation menu again.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image75.png)

6.  Click on the **New Service connection** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image76.png)

7.  Select **GitHub** and then click **Next**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image77.png)

8.  Select **AzurePipelines** from **OAuth Configuration** drop down and
    then click on **Authorize** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image78.png)

9.  Keep the default service name, select **Grant access permission to
    all pipelines** security check box and then click on **Save** .

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image79.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image80.png)

10. Click on project name from top navigation menu.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image81.png)

### Task 4: Create and Configure pipeline to use GEC repo

1.  Click on **Pipelines** from left navigation menu and then select
    Pipeline.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image82.png)

2.  Click on **Create Pipeline**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image83.png)

3.  Select **Azure Repo git** tile

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image84.png)

4.  Select the **Lab07-** **tailspin-spacegame-web-deploy** repo.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image85.png)

5.  Select **ASP.NET**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image86.png)

6.  Click on **Save and run** drop down and select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image87.png)

7.  Keep the default commit message and then click on **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image88.png)

8.  Select the **Pipelines** from the left navigation menu and select
    the pipeline to **Edit**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image89.png)

9.  Select **release** branch and then click next to **Run** button and
    select **Triggers**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image90.png)

10. Configure CI settings under **Triggers** tab, **select Override the
    YAML continuous integration trigger from here** checkbox and then
    include **release** branch

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image91.png)

11. Select **YAML** tab and select **Get sources-\>GitHub** and then
    click on **Repository**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image92.png)

12. Select the repo
    (**devopstogithubXXXX/Lab07-migrate-multistagerepos**) which was
    migrated in above tasks and then click on **Select** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image93.png)

13. Click on **Pipeline** under YAML

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image94.png)

14. Select **Azure Pipeline** under **Default agent pool for YAML** drop
    down and then click on **Save & queue-\> Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image95.png)

15. Enter some comments and click on **Save** to save the build
    pipeline.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image96.png)

16. Switch back to GitHub browser tab and navigate to the Lab07 repo
    and click on Code-> copy Https url to use in next task

    >Example: https://github.com/devopstogtihub1234/Lab07-tailspin-spacegame-web-deploy.git

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image97.png)

---

## Exercise 3 - Promote to the Dev stage

The team has a plan and is ready to begin implementing their release
pipeline. Your Azure DevOps project is set up, and your Azure App
Service instances are ready to receive build artifacts.

At this point, remember that the team's pipeline has only two stages.
The first stage produces the build artifact. The second stage deploys
the *Space Game* web app to App Service. Here, you follow along with
Andy and Mara as they modify the pipeline. They're going to deploy to
the App Service environment that corresponds to the *Dev* stage.

The *Dev* stage resembles the deployment stage that you made in
the Create a release pipeline in Azure Pipelines module. There, you used
a CI trigger to start the build process. Here you do the same.

### Task 1: Promote changes to the Dev stage

Here, you modify your pipeline configuration to promote the build to
the *Dev* stage.

1.  In Visual Studio Code, replace **azure-pipelines.yml** code with
    below code and save the file

  ```
  trigger:
  branches:
    include:
      - '*'
  pr:
  branches:
    include:
      - '*'
  
  variables:
  buildConfiguration: 'Release'
  releaseBranchName: 'release'
  dotnetSdkVersion: '8.x'
  wwwrootDir: 'Tailspin.SpaceGame.Web/wwwroot'
  
  stages:
  - stage: 'Build'
  displayName: 'Build the web application'
  jobs:
  - job: 'Build'
    displayName: 'Build job'
    pool:
      vmImage: 'windows-latest'  #  Microsoft-hosted agent
    steps:
    - task: UseNode@1
      displayName: 'Use Node.js 18.20.8'
      inputs:
        version: '18.20.8'
  
    - task: UseDotNet@2
      displayName: 'Use .NET SDK $(dotnetSdkVersion)'
      inputs:
        packageType: sdk
        version: '$(dotnetSdkVersion)'
  
    - task: Npm@1
      displayName: 'Run npm install'
      inputs:
        command: 'install'
        verbose: false
  
    - powershell: |
        $scssPath = "$(wwwrootDir)/scss"
        if (Test-Path $scssPath) {
          Write-Host "SCSS directory found. Compiling..."
          npx sass $scssPath:$(wwwrootDir)/css
        } else {
          Write-Host "SCSS directory not found. Skipping Sass compilation."
        }
      displayName: 'Compile Sass assets'
  
    - script: 'npx gulp'
      displayName: 'Run gulp tasks'
      workingDirectory: Tailspin.SpaceGame.Web
  
    - script: |
        echo "$(Build.DefinitionName), $(Build.BuildId), $(Build.BuildNumber)" > buildinfo.txt
      displayName: 'Write build info'
      workingDirectory: $(wwwrootDir)
  
    - task: DotNetCoreCLI@2
      displayName: 'Restore project dependencies'
      inputs:
        command: 'restore'
        projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
  
    - task: DotNetCoreCLI@2
      displayName: 'Build the project - $(buildConfiguration)'
      inputs:
        command: 'build'
        arguments: '--no-restore --configuration $(buildConfiguration)'
        projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
  
    - task: DotNetCoreCLI@2
      displayName: 'Publish the project - $(buildConfiguration)'
      inputs:
        command: 'publish'
        projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
        publishWebProjects: true
        arguments: '--no-build --configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)/$(buildConfiguration)'
        zipAfterPublish: true
  
    - task: PublishBuildArtifacts@1
      displayName: 'Publish Artifact: drop'
      inputs:
        pathToPublish: '$(Build.ArtifactStagingDirectory)'
        artifactName: 'drop'
        publishLocation: 'Container'
  
  - stage: 'Dev'
  displayName: 'Deploy to the dev environment'
  dependsOn: Build
  condition: |
    and(
      succeeded(),
      eq(variables['Build.SourceBranchName'], variables['releaseBranchName'])
    )
  jobs:
  - deployment: Deploy
    pool:
      vmImage: 'windows-latest'  #  Microsoft-hosted agent
    environment: dev
    variables:
      - group: Release
    strategy:
      runOnce:
        deploy:
          steps:
          - download: current
            artifact: drop
          - task: AzureWebApp@1
            displayName: 'Azure App Service Deploy: website'
            inputs:
              azureSubscription: 'Resource Manager - Tailspin - Space Game'
              appName: '$(WebAppNameDev)'
              package: '$(Pipeline.Workspace)/drop/$(buildConfiguration)/*.zip'
  ```

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image98.png)

    >[!note]**Note**: In practice, you might deploy from some other branch, such as main. You can include logic that allows changes to be promoted to the *Dev* stage from multiple branches, such as release and main.

2.  Run below commands to change origin so it points to your **GEC repo** (the one you migrated to).

    # Remove the old origin
    +++git remote remove origin+++
  
    # Add your GEC repo as the new origin .Replace your-GEC-org with your org name – eg devopstogtihub1234
    +++git remote add origin https://github.com/<your-GEC-org>/lab07-migrate-repos.git+++

    # Verify remotes
    +++git remote -v+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image99.png)

3.  From the integrated terminal, add ***azure-pipelines.yml*** to the
    index. Commit the change, and push it up to GitHub.

    +++git add azure-pipelines.yml+++

    +++git commit -m "Deploy to the Dev stage"+++

    +++git push origin release+++

    ![A computer screen shot of a program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image100.png)

4.  Switch back Azure DevOps project and click on **Pipeline** form left
    navigation menu and then click on the running pipeline.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image101.png)

5.  Click on the running build

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image102.png)

6.  Click on **View** button next to warning message **"This pipeline
    needs permission to access a resource before this run can continue
    to Build the web application**"

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image103.png)

7.  Review the permission and permit access by clicking on **Permit**
    button.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image104.png)

8.  Wait for the Build to complete.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image105.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image106.png)

9.  Now click on **View** again next to the warning message "  
    **This pipeline needs permission to access 2 resources before this
    run can continue to Deploy to the dev environment"**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image107.png)

10. Provide permission to both dev and release

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image108.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image109.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image110.png)

11. Wait for the deployment to be completed.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image111.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image112.png)

12. Switch back to Azure App service tab and click on Dynamic URL of Dev
    app.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image113.png)

13. You see that the *Space Game* website is deployed to App Service,
    and is running.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image114.png)

---

## Exercise 4 - Promote to the Test stage

Your release pipeline still has two stages, but they're now different
than before. The stages are *Build* and *Dev*. Every change you push to
GitHub triggers the *Build* stage to run. The *Dev* stage runs only when
the change is in the *release* branch. Here, you add the *Test* stage to
the pipeline.

Tailspin team decided to use a scheduled trigger to promote the build
from the Dev stage to the Test stage at 3 A.M. each morning. This
simulates a scenario where your application is promoted to Test
automatically at an agreed time for QA and automated testing.

To set up the scheduled trigger:

- Define the schedule in your build configuration.

- Define the Test stage, which includes a condition that runs the stage
  only if the build reason is marked as Schedule.

For learning purposes, we’ll define the schedule but also allow the
build to go directly from Dev to Test without waiting for 3 A.M. This
lets you complete the lab without delay.

### Task 1: Promote changes to the Test stage

Here, you modify your pipeline configuration to deploy the build to
the *Test* stage.

1.  In Visual Studio Code, replace the code **azure-pipelines.yml** with
    below code and save the file

    ```
    trigger:
    branches:
      include:
        - '*'
  
    pr:
    branches:
      include:
        - '*'
  
    schedules:
    - cron: '0 3 * * *'
    displayName: 'Deploy every day at 3 A.M.'
    branches:
      include:
        - release
    always: false
  
    variables:
    buildConfiguration: 'Release'
    releaseBranchName: 'release'
    dotnetSdkVersion: '8.x'
    wwwrootDir: 'Tailspin.SpaceGame.Web/wwwroot'
  
    stages:
    - stage: 'Build'
      displayName: 'Build the web application'
    jobs:
    - job: 'Build'
      displayName: 'Build job'
      pool:
        vmImage: 'windows-latest'
      steps:
      - task: UseNode@1
        displayName: 'Use Node.js 18.20.8'
        inputs:
          version: '18.20.8'
  
      - task: UseDotNet@2
        displayName: 'Use .NET SDK $(dotnetSdkVersion)'
        inputs:
          packageType: sdk
          version: '$(dotnetSdkVersion)'
  
      - task: Npm@1
        displayName: 'Run npm install'
        inputs:
          command: 'install'
          verbose: false
  
      - powershell: |
        $scssPath = "$(wwwrootDir)/scss"
        if (Test-Path $scssPath) {
          Write-Host "SCSS directory found. Compiling..."
          npx sass $scssPath:$(wwwrootDir)/css
        } else {
          Write-Host "SCSS directory not found. Skipping Sass compilation."
        }
        displayName: 'Compile Sass assets'
  
      - script: 'npx gulp'
        displayName: 'Run gulp tasks'
        workingDirectory: Tailspin.SpaceGame.Web
  
      - script: |
          echo "$(Build.DefinitionName), $(Build.BuildId), $(Build.BuildNumber)" > buildinfo.txt
        displayName: 'Write build info'
        workingDirectory: $(wwwrootDir)
  
      - task: DotNetCoreCLI@2
        displayName: 'Restore project dependencies'
        inputs:
          command: 'restore'
          projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
  
      - task: DotNetCoreCLI@2
        displayName: 'Build the project - $(buildConfiguration)'
        inputs:
          command: 'build'
          arguments: '--no-restore --configuration $(buildConfiguration)'
          projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
  
      - task: DotNetCoreCLI@2
        displayName: 'Publish the project - $(buildConfiguration)'
        inputs:
          command: 'publish'
          projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
          publishWebProjects: true
          arguments: '--no-build --configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)/$(buildConfiguration)'
          zipAfterPublish: true
  
      - task: PublishBuildArtifacts@1
        displayName: 'Publish Artifact: drop'
        inputs:
          pathToPublish: '$(Build.ArtifactStagingDirectory)'
          artifactName: 'drop'
          publishLocation: 'Container'
  
    - stage: 'Dev'
      displayName: 'Deploy to the dev environment'
      dependsOn: Build
      condition: |
        and(
          succeeded(),
          eq(variables['Build.SourceBranchName'], variables['releaseBranchName'])
        )
      jobs:
      - deployment: Deploy
        pool:
          vmImage: 'windows-latest'
        environment: dev
        variables:
          - group: Release
        strategy:
          runOnce:
            deploy:
              steps:
              - download: current
                artifact: drop
              - task: AzureWebApp@1
                displayName: 'Azure App Service Deploy: website'
                inputs:
                  azureSubscription: 'Resource Manager - Tailspin - Space Game'
                  appName: '$(WebAppNameDev)'
                  package: '$(Pipeline.Workspace)/drop/$(buildConfiguration)/*.zip'
  
    - stage: 'Test'
      displayName: 'Deploy to the test environment'
      dependsOn: Dev
      condition: |
        or(
          eq(variables['Build.Reason'], 'Schedule'),
          succeeded()
        )
      jobs:
      - deployment: Deploy
        pool:
          vmImage: 'windows-latest'
        environment: test
        variables:
          - group: Release
        strategy:
          runOnce:
            deploy:
              steps:
              - download: current
                artifact: drop
              - task: AzureWebApp@1
                displayName: 'Azure App Service Deploy: website'
                inputs:
                  azureSubscription: 'Resource Manager - Tailspin - Space Game'
                  appName: '$(WebAppNameTest)'
                  package: '$(Pipeline.Workspace)/drop/$(buildConfiguration)/*.zip'
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image115.png)

2.  From the integrated terminal to the index, add ***azure-pipelines.yml***. Then, commit the change, and push it
    up to GitHub.

    +++git add azure-pipelines.yml+++

    +++git commit -m "Deploy to the Test stage"+++

    +++git push origin release+++

    ![A screenshot of a computer program AI-generated content may be  incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image116.png)

3.  Switch back to **Azure Devops-\> Project-\>Pipelines** and click on
    Pipeline name-**mslearn-tailspin-spacegame-deploy**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image117.png)

4.  Click on **Deploy to Test Stage running** job

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image118.png)

5.  Wait for the deployment to dev stage complete

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image119.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image120.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image121.png)

6.  Click on View next to the warning message **"This pipeline needs
    permission to access a resource before this run can continue to
    Deploy to the test environment"**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image122.png)

7.  Click on **Permit-\>Permit** to permit access to deploy to test
    environment.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image123.png)

8.  Wait for the deployment to be completed

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image124.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image125.png)

9.  Switch back to Azure portal-\>Resource group and click on Azure Test
    app service name.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image126.png)

10. Click on Default domain

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image127.png)

11. App is up and running.

    ![A screenshot of a game AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image128.png)

---

## Exercise 5 : Promote to Staging

Your release pipeline now has three stages: *Build*, *Dev*, and *Test*.
You and the Tailspin team have one more stage to implement: *Staging*.

In this part, you'll:

- Create the **staging** environment in Azure Pipelines, and assign
  yourself as an approver.

- Define the *Staging* stage, which runs only after an approver verifies
  the results of the *Test* stage.

### Task 1: Update approvals in the staging environment

Here, you create an environment in Azure Pipelines for *Staging*. For
learning purposes, you assign yourself as the approver. In practice, you
would assign the users who are required to approve changes before those
changes move to the next stage. For the Tailspin team, Amita approves
changes so that they can be promoted from *Test* to *Staging*.

Earlier in this module, you specified environment settings for
both *Dev* and *Test* stages. Here's an example for the *Dev* stage.

```nocopy
- stage: 'Deploy'
  displayName: 'Deploy the web application'
  dependsOn: Build
  jobs:
  - deployment: Deploy
    pool:
      name: Default s
      environment: dev
    variables:
  - group: Release
```

You can define an environment through Azure Pipelines that includes
specific criteria for your release. This criteria can include the
pipelines that are authorized to deploy to the environment. You can also
specify the human approvals that are needed to promote the release from
one stage to the next. Here, you specify those approvals.

1.  Switch back to Azure DevOps tab, click on **Environments** and then
    select **staging** environment**.**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image129.png)

2.  Select **Approvals and checks** tab and then select **Approvals**
    check.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image130.png)

3.  Under **Approvers**, select cloud slice account.
    Under **Instructions to approvers**, enter ***Approve this change
    when it's ready for staging*** and then click on **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image131.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image132.png)

### Task 2: Promote changes to Staging

Here you modify your pipeline configuration to deploy the build to
the *Staging* stage.

1.  Switch back to Visual Studio Code, replace the
     *azure-pipelines.yml* code with below code.

    ```
    trigger:
    branches:
      include:
        - '*'
  
    pr:
    branches:
      include:
        - '*'
  
    schedules:
    - cron: '0 3 * * *'
    displayName: 'Deploy every day at 3 A.M.'
    branches:
      include:
        - release
          always: false
  
    variables:
    buildConfiguration: 'Release'
    releaseBranchName: 'release'
    dotnetSdkVersion: '8.x'
    wwwrootDir: 'Tailspin.SpaceGame.Web/wwwroot'
  
    stages:
    - stage: 'Build'
      displayName: 'Build the web application'
      jobs:
        - job: 'Build'
          displayName: 'Build job'
          pool:
            vmImage: 'windows-latest'
          steps:
          - task: UseNode@1
            displayName: 'Use Node.js 18.20.8'
            inputs:
              version: '18.20.8'
  
          - task: UseDotNet@2
            displayName: 'Use .NET SDK $(dotnetSdkVersion)'
            inputs:
              packageType: sdk
              version: '$(dotnetSdkVersion)'
  
          - task: Npm@1
            displayName: 'Run npm install'
            inputs:
              command: 'install'
              verbose: false
  
          - powershell: |
              $scssPath = "$(wwwrootDir)/scss"
              if (Test-Path $scssPath) {
                Write-Host "SCSS directory found. Compiling..."
                npx sass $scssPath:$(wwwrootDir)/css
              } else {
                Write-Host "SCSS directory not found. Skipping Sass compilation."
              }
            displayName: 'Compile Sass assets'
  
          - script: 'npx gulp'
            displayName: 'Run gulp tasks'
            workingDirectory: Tailspin.SpaceGame.Web
  
          - script: |
              echo "$(Build.DefinitionName), $(Build.BuildId), $(Build.BuildNumber)" > buildinfo.txt
            displayName: 'Write build info'
            workingDirectory: $(wwwrootDir)
  
          - task: DotNetCoreCLI@2
            displayName: 'Restore project dependencies'
            inputs:
              command: 'restore'
              projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
  
          - task: DotNetCoreCLI@2
            displayName: 'Build the project - $(buildConfiguration)'
            inputs:
              command: 'build'
              arguments: '--no-restore --configuration $(buildConfiguration)'
              projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
  
          - task: DotNetCoreCLI@2
            displayName: 'Publish the project - $(buildConfiguration)'
            inputs:
              command: 'publish'
              projects: 'Tailspin.SpaceGame.Web/Tailspin.SpaceGame.Web.csproj'
              publishWebProjects: true
              arguments: '--no-build --configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)/$(buildConfiguration)'
              zipAfterPublish: true
  
          - task: PublishBuildArtifacts@1
            displayName: 'Publish Artifact: drop'
            inputs:
              pathToPublish: '$(Build.ArtifactStagingDirectory)'
              artifactName: 'drop'
              publishLocation: 'Container'
  
    - stage: 'Dev'
      displayName: 'Deploy to the dev environment'
      dependsOn: Build
      condition: |
        and(
          succeeded(),
          eq(variables['Build.SourceBranchName'], variables['releaseBranchName'])
        )
      jobs:
      - deployment: Deploy
        pool:
          vmImage: 'windows-latest'
        environment: dev
        variables:
          - group: Release
        strategy:
          runOnce:
            deploy:
              steps:
              - download: current
                artifact: drop
              - task: AzureWebApp@1
                displayName: 'Azure App Service Deploy: website'
                inputs:
                  azureSubscription: 'Resource Manager - Tailspin - Space Game'
                  appName: '$(WebAppNameDev)'
                  package: '$(Pipeline.Workspace)/drop/$(buildConfiguration)/*.zip'
  
    - stage: 'Test'
      displayName: 'Deploy to the test environment'
      dependsOn: Dev
      condition: |
        or(
          eq(variables['Build.Reason'], 'Schedule'),
          succeeded()
        )
      jobs:
      - deployment: Deploy
        pool:
          vmImage: 'windows-latest'
        environment: test
        variables:
          - group: Release
        strategy:
          runOnce:
            deploy:
              steps:
              - download: current
                artifact: drop
              - task: AzureWebApp@1
                displayName: 'Azure App Service Deploy: website'
                inputs:
                  azureSubscription: 'Resource Manager - Tailspin - Space Game'
                  appName: '$(WebAppNameTest)'
                  package: '$(Pipeline.Workspace)/drop/$(buildConfiguration)/*.zip'
  ```

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image133.png)

    >[!note]**Note**: This code adds the *Staging* stage. The stage deploys to
    >the **staging** environment, which includes a release approval.

2.  From the integrated terminal, add ***azure-pipelines.yml*** to the
    index. Next, commit the change and push it up to GitHub.

    +++git add azure-pipelines.yml+++

    +++git commit -m "Deploy to Staging"+++

    +++git push origin release+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image134.png)

3.  Switch back to the Azure Pipeline and click on the running pipeline.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image135.png)

4.  Click on the **Deploy to Staging** run.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image136.png)

5.  Wait for the build, dev and test deployment to be completed.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image137.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image138.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image139.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image140.png)

6.  Click on **View** next to the warning message "**This pipeline needs permission to access a resource before this
    run can continue to Deploy to the staging environment**"

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image141.png)

7.  Click on **Permit ->Permit** to permit access to deploy to the
    staging environment.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image142.png)

8.  Add a comment and then click on **Approve** button. In practice, to
    verify that they meet your requirements, you should inspect the
    changes.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image143.png)

9.  Wait for the deployment to be completed.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image144.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image145.png)

10. Switch back to **Azure portal- \> Resource group** and select
    **Staging Azure app service**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image146.png)

11. Click on **Default domain**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image147.png)

12. You see that the *Space Game* website is deployed to App Service and
    is running.

    ![A screenshot of a game AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image148.png)

### Task 3: Promote Staging changes to main in GEC

Now that your changes have passed manual approval and are successfully
deployed to the Staging environment, it’s time to promote them to the
main branch in GitHub Enterprise Cloud (GEC).

1.  Switch back to GitHub tab and click on profile and select **Your
    enterprises**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image149.png)

2.  Click on Enterprise account name.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image150.png)

3.  Click on Organizations tab and select your enterprise organization.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image151.png)

4.  Click on repositories tab and then click on your
    **Lab07-migrate-multistagerepos** repository

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image152.png)

5.  In yellow banner ,click on **Compare & pull request** button next to
    **"release had recent pushes XX minutes ago"**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image153.png)

6.  Add the pr title as +++**Promote post-Staging approval+++** and
    select base branch as "**main**" and source branch as "**release".**
    Add a brief description (optional for lab) and then click **Create
    pull request**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image154.png)

7.  Scroll down and click on **Merge pull request.**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image155.png)

8.  Keep the default commit message and then click on **Confirm merge**
    button.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image156.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image157.png)

---

## Exercise 6 : Clean up your Azure DevOps environment

You're finished with the tasks for this module. In this unit, you clean
up your Azure resources, move the work item to the **Done** state on
Azure Boards, and clean up your Azure DevOps environment.

### Task 1: Clean up Azure resources

1.  Go to the Azure portal, and click on **ResourceGroup1**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image158.png)

2.  Select all resource and click on delete ( **DO NOT DELETE resource
    group**)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image159.png)

3.  Enter delete in the text box and click on **Delete** and confirm
    deletion.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image160.png)

### Task 2: Move the work item to Done

Now, move the work item that you assigned to yourself earlier in this
module. Move **Create a multistage pipeline** to the **Done** column.

In practice, "Done" often means putting working software into the hands
of your users. For learning purposes, here, you mark this work as done
because you fulfilled the goal for the Tailspin team. They wanted to
define a complete multistage pipeline to deliver new features.

At the end of each *sprint*, or work iteration, you and your team can
hold a retrospective meeting. In the meeting, share the work you
completed, what went well, and what you can improve.

1.  From Azure DevOps project, go to **Boards**, and from the menu,
    select **Boards**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image161.png)

2.  Move the **Create a multistage pipeline** work item, from
    the **Committed** column to the **Done** column.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image162.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image163.png)

### Task 3: Disable the pipeline or delete your project

1.  In Azure Pipelines, go to your pipeline.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image164.png)

2.  From the dropdown, select **Settings**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image165.png)

3.  Under **Processing of new run requests**, select **Disabled**, and
    then select **Save**.Now, your pipeline no longer processes build
    requests.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab07/media/image166.png)

### Summary

This lab mirrored a typical enterprise GitHub workflow:

- Developer commits changes to a branch.

- Opens a Pull Request.

- Code review & approval required before merge.

- Merge triggers pipeline to deploy changes.

This flow ensures every change is reviewed, discussed, and tested before
it reaches main.
