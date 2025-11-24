# Lab 10 - Implement Secret Scanning and Protection in GitHub

GitHub scans repositories for known types of secrets, to prevent
fraudulent use of secrets that were committed accidentally. In this
GitHub Skills course, you'll learn how to enable secret scanning to
identify secrets and prevent them from being committed to your
repository.

- **Who this is for**: Developers, DevOps Engineers, Security
  practitioners

- **What you'll learn**: How to identify plain-text credentials in your
  repository and how to prevent them from being written in the first
  place

- **Prerequisites**: Basics of git and GitHub functionality

- **Timing**: This course takes less than 15 minutes to complete

## Exercise 1 : Enable Secret Protection

1.  Open a browser and navigate to  +++https://github.com/technofocus-pte/secretscanningrepo.git+++

2.  Scroll down and click on **COPY EXERCISE** button. Sign in with your
    GitHub account

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image1.png)

    ![A screenshot of a login form AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image2.png)

3.  Click on **Create repository.**

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image3.png)

4.  Wait for 20 sec and then refresh the page and then click on Got to
    Exercise once its available.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image4.png)

### Task 1: Configure secret protection

1.  In the header of your repository, open **Settings** in a new browser
    tab.

2.  In the left navigation, under the **Security** section,
    select **Advanced Security**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image5.png)

3.  Scroll down past the **Code Scanning** and **Dependabot** sections
    until you find the **Secret Protection** section.

    ![A screenshot of a computer AI-generated content may be  incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image6.png)

4.  Adjust the default configuration to match the below.

    - **Secret Protection:** enabled

    - **Push Protection:** disabled

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image7.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image8.png)

### Task 2: Commit a sensitive file

Now let's (accidentally) commit a sensitive file to see how it works.
Don't worry, these are inactive credentials.

1.  In the header of your repository, click the **Code** tab.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image9.png)

2.  Above the list of files, click the **Add file dropdown** and select
    **Create new file**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image10.png)

3.  Enter the file name +++**credentials.yml+++** and copy following
    inactive example credentials into it. In the top right, use the
    **Commit changes**... button to commit directly to the main branch.

    ```
    default:
    aws_access_key_id: AKIAQYLPMN5HNM4OZ56B
    aws_secret_access_key: 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
    mongodb: mongodb+srv://svc-admin:kLeioeBne5lsopPf@mergington-high.avocado.mongodb.net
    output: json
    region: us-east-2
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image11.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image12.png)

4.  Refresh the page and click on **GO TO EXCERCISE**

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image13.png)

---

## Exercise 2: Review and close secret scanning alerts

In the last exercise, you enabled secret protection and committed a
sensitive file to the repository. Now, let's review our open secret
scanning alerts and close one.

### Task 1: Triage secret scanning alerts

1.  In the header of your repository, click the **Security** tab.In the
    left navigation, select the **Secret scanning** option.

2.  Notice various options in the header bar that can help triage our
    alerts.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image14.png)

### Task 2: Review a secret scanning alert

1.  In the list of open alerts, select the **Amazon AWS Access Key
    ID alert**. This will open a details page with more information.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image15.png)

2.  At the top of the page, we can quickly view the alert status, when
    it was opened, the exposed secret, and some remediation steps.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image16.png)

3.  Scroll down slightly to the **Detected in X locations** area and you
    will see all the places where this secret was detected, including
    the credentials.yml file that you created. Notice that secret
    protection doesn't create duplicate alerts for the same secret found
    across multiple locations, for example in our learning issue.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image17.png)

### Task 3: Close an alert

When secret protection finds a secret in your repository, the first
thing you should do is **disable that secret with the provider**. You
should assume it has been exposed.

1.  Assuming you have taken remediation steps, we can update the status
    of our alert. In the top right, select the **Close as** dropdown.

2.  Choose the **Revoked** option and enter a useful description of your
    remediation steps in the comment box (example below). Then
    choose **Close alert**.

    +++The secret owner was contacted. They provided proof that the exposed secret was replaced.+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image18.png)

3.  The alert status now displays Closed and the audit trail includes
    our explanation.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image19.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image20.png) 

4.  With at least one of our alerts resolved, let's add a comment to get
    next step.

    +++Hello @professortocat, I've resolved the security alert. What's next?+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image21.png)

---

## Exercise 3: Enable push protection

In this exercise, you will configure your repository to prevent new
secrets from being exposed.

**What is push protection?**

When someone tries to send code changes to GitHub (a push), secret
scanning checks for high-confidence secrets. Secret scanning lists any
secrets it detects so the author can review the secrets and remove them
or, if needed, allow those secrets to be pushed.

### Task 1: Configure push protection

We disabled push protection in our first step for learning practice. It
is normally enabled by default for all public repositories.

1.  In the header of your repository, click the **Settings** tab.In the
    left navigation, under the **Security** section, select **Advanced
    Security**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image22.png)

2.  Scroll down past the **Code Scanning** and **Dependabot** sections
    until you find the **Secret Protection** section.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image23.png)

3.  Adjust the default configuration to match the below.

    - **Secret Protection:** enabled

    - **Push Protection:** enabled

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image24.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image25.png)

### Task 2: Attempt to push a secret

Now that secret push protection is enabled, let's give it a test!

1.  In the header of your repository, click the **Code** tab.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image26.png)

2.  In the list of files, click on the **credentials.yml** file to
    preview it.Above the content preview, click the **Edit** button.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image27.png)

3.  Copy the following inactive secret to the file, removing
    the \<REMOVE_ME\> text. It should look like the below screenshot. In
    the top right, use the **Commit changes...** button to **try
    to** commit directly to the main branch.

    +++github-token: github_pat_<REMOVE_ME>11A4YXR6Y0v36CYFkuT5I1_ZRWX91c8k0waSN6x7AiVJ6zZ9ZHUQXBblBqFQpKd23V6CL7MWMPopnmBxzn+++

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image28.png)

4.  Enter a comment and then click on **Commit changes**.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image29.png)

5.  Instead of committing the updated file, a push protection alert
    appeared.

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image30.png)

### Task 3: Bypass push protection

In some cases, you may write code that looks similar to a secret and a
commit is incorrectly blocked. For example writing tests for an
authorization process. In those situations, you can choose to bypass
push protection. Let's practice that.

1.  Select the radio button next to **It's used in tests**. Notice the
    description matches our current learning use case.

2.  Click **Allow secret**. A notification banner reports that you can
    now try committing again.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image31.png)

3.  In the top right, use the **Commit changes...** button to commit
    directly to the main branch.

    ![](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image32.png)

    ![A screenshot of a computer AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/mgrtdvopsghdepth/refs/heads/Cloud-slice/Lab10/media/image33.png)
