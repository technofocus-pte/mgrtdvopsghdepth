# Lab 11 - Secure your repository's supply chain

## Exercise 1: Review and add dependencies using dependency graph

**What's the big deal about securing your repository's supply chain?**:
With the accelerated use of open source, most projects depend on
hundreds of open-source dependencies. This poses a security problem:
what if the dependencies you're using are vulnerable? You could be
putting your users at risk of a supply chain attack. One of the most
important things you can do to protect your supply chain is to patch
your vulnerable dependencies and replace any malware.

GitHub offers a range of features to help you understand the
dependencies in your environment, know about vulnerabilities in those
dependencies, and patch them. The supply chain features on GitHub are:

- Dependency graph

- Dependency review

- Dependabot alerts

- Dependabot updates

  - Dependabot security updates

  - Dependabot version updates

**What is a dependency graph**: The dependency graph is a summary of the
manifest and lock files stored in a repository and any dependencies that
are submitted for the repository using the dependency submission API
(beta). For each repository, it shows:

- Dependencies, the ecosystems and packages it depends on

- Dependents, the repositories and packages that depend on it

### Task 1: Verify that dependency graph is enabled

Dependency graph is enabled by default for all new public repositories.

1.  Open a browser and navigate to -
    <https://github.com/skills/secure-repository-supply-chain.git>. Sign
    in with your GitHub account.

2.  Click on **COPY EXERCISE** .

![](./media/image1.png)

3.  Keep default values and then click on **Create repository.**

![](./media/image2.png)

4.  Navigate to the **Settings** tab.Click **Advanced
    Security**.Verify **Dependency Graph** is **Enabled**

![](./media/image3.png)

### Task 2: Add a new dependency and view your dependency graph

1.  Navigate to the **Code** tab and locate
    the **code/src/AttendeeSite** folder.

2.  Commit the following content on the main branch to
    the **package-lock.json** file as the last item on
    the dependencies map *(after the third to last bracket } and before
    the last two brackets)*

🪧 **Note:** You can edit and commit the file on github.com directly or
hit the . key to open the lightweight editor to edit and commit changes.

,

"follow-redirects": {

"version": "1.14.1",

"resolved":
"https://registry.npmjs.org/follow-redirects/-/follow-redirects-1.14.1.tgz",

"integrity":
"sha512-HWqDgT7ZEkqRzBvc2s64vSZ/hfOceEol3ac/7tKwzuvEyWx3/4UegXh5oBOIotkGsObyk3xznnSRVADBgWSQVg=="

}

3.  Navigate to the **Insights** tab.

4.  Select **Dependency graph** from the side navigation bar.

5.  Review all the dependencies on the **Dependencies** tab.

6.  Search for follow-redirects and review the new dependency you just
    added.  
    ![Screen Shot showing the "follow-redirects"
    dependency.](./media/image4.png)

7.  With the new dependency added, Mona should already be busy checking
    your work. Give her a moment and keep watch in the comments. You
    will see her respond with progress info and the next lesson.
