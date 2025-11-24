# Lab 04 - Simulate Dry Run Migration of Multiple Repositories from ADO to GEC

**Objective**

Learn how to **simulate the dry-run migration** of **multiple Azure
DevOps (ADO) repositories** into **GitHub Enterprise Cloud (GEC)** using
a **batch-driven scripting approach**. This lab leverages **GitHub
CLI**, **GitHub Enterprise Importer (GEI)**, and custom shell scripts to
validate the readiness of large-scale repo migrations before executing
actual transfers.

### Task 1: Create Local Repositories

1.  Switch back to GitBash and run below commands to create directory.

    +++cd **"C:\LabFiles"+++**

    +++mkdir Lab04-MultiRepo && cd Lab04-MultiRepo+++

    ![A black screen with colorful text AI-generated content may be  incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image1.png)

2.  Update below commands with your GitHub account name and email id and
    then run them.

    +++git config --global user.email "you@example.com"+++

    +++git config --global user.name "Your Name"+++

    ![A screen shot of a computer code AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image2.png)

3.  Run below script to create repositories

    ```
    for repo in repo1 repo2 repo3
    do
      mkdir $repo && cd $repo
      git init
      echo "# $repo for migration" > README.md
      git add . && git commit -m "Initial commit for $repo"
      cd ..
    done
    ```

    ![A screen shot of a computer program AI-generated content may be  incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image3.png)

4.  Open GitBash from Desktop and run below command to navigate to the
    project repo and commit changes

    +++git init+++

    +++git add .+++

    +++git commit -m "Lab 04 multi repo migration lab"+++

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image4.png)

    >[!note]**Note**: If not committing then run this rm -rf repo1/.git repo2/.git repo3/.git and re-run above inti,add and commit commands

5.  Switch back to GitBash and run below command to sign into Azure.
    Sign in with your Azure subscription account.

    +++az login+++

    - Username: +++@lab.CloudPortalCredential(User1).Username+++

    - Access Token: +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![A screen shot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image5.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image6.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image7.png)

6.  Select your subscirption.Enter 1 to select your subsdcirption.

    ![A computer screen with text on it AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image8.png)

    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image9.png)

### Task 2: Create Repos in Azure DevOps

1.  Go to back to Azure DevOps browser tab and select your existing
    DevOps project under Azure DevOps Organization.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image10.png)

2.  Hover on **Repo** from the left navigation menu and select **File**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image11.png)

3.  Click the repo dropdown from top navigation menu and select **New
    Repository**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image12.png)

4.  Create the following repositories (leave default settings): **Do NOT
    add README/gitignore** here, as your local repos already have those.

    - +++repo1+++

    - +++repo2+++

    - +++repo3+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image13.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image14.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image15.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image16.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image17.png)

### Task 3: Push local repos to ADO repos

1.  Switch back to GitBash. Run the commands . sign in with your
    assigned DevOps account when prompted

    +++cd repo1+++

    +++git remote add origin https://dev.azure.com/$ADO_ORG/$ADO_PROJECT/_git/repo1+++

    +++git commit -m "Initial commit for migration lab"+++

    +++cd ..+++

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image18.png)

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image19.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image20.png)

2.  Replace <**DEVOPS_ORG**\> with your DevOps org name
    (ADOCourseOrg04) with <**DEV_PROJECT**\> with your project name
    and run the commands . sign in with your assigned DevOps account
    when prompted

    +++cd repo2+++

    +++git remote add origin https://dev.azure.com/$ADO_ORG/$ADO_PROJECT/\_git/repo2+++

    +++git push -u origin –all+++

    +++cd ..+++

    ![A screen shot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image21.png)

3.  Replace <**DEVOPS_ORG**\> with your DevOps org name
    (ADOCourseOrg04) with <**DEV_PROJECT**\> with your project name
    and run the commands. sign in with your assigned DevOps account
    when prompted (URL
    https://ADOCourseOrg04@dev.azure.com/ADOCourseOrg04/dev-github-proj-54081082/\_git/repo2)

    +++cd repo3+++

    ++git remote add origin https://dev.azure.com/$ADO_ORG/$ADO_PROJECT/\_git/repo3+++

    +++git push -u origin –all+++

    +++cd ..+++

    ![A screen shot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image22.png)

4.  Go back to Azure DevOps project and check **Repos -\> Files** .You
    should see your repo here .

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image23.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image24.png)

### Task 4: Simulate Dry-Run Migration of Multiple Repositories using Scripted Batch Mode

1.  Update below commands with your ADO’s PAT and GitHub’s PAT and run

    +++export AZURE_DEVOPS_PAT=your_ado_pat_here+++

    +++export GH_PAT=your_github_pat_here+++

    ![A screen shot of a computer code AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image25.png)

2.  Run below command to create csv file and the below repo data and
    then save the file. This CSV acts as the migration map to control
    which ADO repos go to which GitHub repos.

    +++vi repo-dryrun-map.csv+++

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image26.png)

3.  Add below data to the file and save the file (replace
    ado_org,ado_project,ado_repo,github_org,github_repo with your
    values) (Esc +wq and press enter)

    ```
    ado_org,ado_project,ado_repo,github_org,github_repo
    https://dev.azure.com/<DEVOPS_ORG>,MDEVOPS_PROJ>,repo1,<GITHUB_ORG>,repo1-migrated
    https://dev.azure.com/<DEVOPS_ORG>,MDEVOPS_PROJ>,repo2,<GITHUB_ORG>,repo2-migrated
    https://dev.azure.com/<DEVOPS_ORG>,MDEVOPS_PROJ>,repo3,<GITHUB_ORG>,repo3-migrated
    ```
    
    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image27.png)

    ![A screen shot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image28.png)

4.  Create another file with the name **dryrun-multi.sh**. Each
    migration will be queued in dry-run mode

    +++vi dryrun-multi.sh+++

    ![A computer screen with white text AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image29.png)

5.  Add the code below to it and save the file (Esc +wq and press enter)

    ```
    #!/bin/bash
    echo " Starting Dry Run Migration for Multiple Repos"
    # Skip header
    tail -n +2 repo-dryrun-map.csv | while IFS=',' read -r ado_org ado_project ado_repo github_org github_repo
    do
      # Skip empty lines
      if [[ -z "$ado_repo" || -z "$github_repo" ]]; then
        continue
      fi
      echo "Migrating $ado_repo ➜ $github_repo"
      gh ado2gh migrate-repo \
        --ado-org "$ado_org" \
        --ado-team-project "$ado_project" \
        --ado-repo "$ado_repo" \
        --github-org "$github_org" \
        --github-repo "$github_repo" \
        --ado-pat "$AZURE_DEVOPS_PAT" \
        --github-pat "$GH_PAT" \
        --queue-only
      echo "Dry Run Queued for $ado_repo"
      echo "--------------------------------------"
    done
    ```
    ![A screenshot of a computer program AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image30.png)

6.  Run below commands to allow script to run

    +++chmod +x dryrun-multi.sh+++

    +++./dryrun-multi.sh+++

    ![A screen shot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image31.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image32.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/Cloud-slice/Lab04/media/image33.png)

### Summary: 

In this lab, you learnt:

- **Generate a Personal Access Token (PAT)** in Azure DevOps with full
    scope access for authentication.

- **Create and initialize local repositories** (repo1, repo2, repo3)
    to simulate real project repos.

- **Push these repositories to Azure DevOps**, creating matching
    remote repositories in your ADO project.

- **Build a mapping CSV file** to define the migration path from ADO
    repos to GitHub Enterprise Cloud target repos.

- **Write and execute a shell script** (dryrun-multi.sh) that reads
    from the CSV and runs **dry-run migrations** for each repo using the
    gh ado2gh migrate-repo command in batch mode with --queue-only flag.

- **Validate that each migration has been queued**, and optionally
    monitor results via gh ado2gh wait-for-migration and logs.
