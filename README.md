# GitHub Actions Terraform Cloud Configuration

This README provides an overview of the Terraform configuration for automating infrastructure management using GitHub Actions and Terraform Cloud. It outlines the steps to set up and use the GitHub Actions workflows for Terraform Plan and Terraform Apply.

## Setup

### Terraform Cloud Setup
1. **Create an Account**: If you don't already have one, sign up for a Terraform Cloud account at [Terraform Cloud](https://app.terraform.io/).
2. **Create an Organization**: Once logged in, create a new organization or select an existing one.
3. **Create a Workspace**: Inside your organization, create a new workspace that will be connected to your GitHub repository.
4. **Generate API Token**: 
   - Go to "User Settings" in Terraform Cloud.
   - Navigate to "Tokens" and create a new API token.
   - Save this token securely as it will be used in GitHub Actions.

### GitHub Setup
1. **Add Secrets to GitHub Repository**: 
   - Navigate to your GitHub repository.
   - Go to 'Settings' > 'Secrets'.
   - Add the following secrets:
     - `TF_API_TOKEN`: The Terraform Cloud API token you generated.
     - `GITHUB_TOKEN`: A GitHub token for repository access (usually auto-generated by GitHub Actions).
2. **Update Workflow Files**: 
   - Edit the provided GitHub Actions workflow files to match your setup.
   - Replace `My-TFC-Tutorial` with your Terraform Cloud organization name.
   - Replace `github_actions_tfc` with your Terraform Cloud workspace name.
   - Adjust `CONFIG_DIRECTORY` if your Terraform configurations are in a different directory.

## Workflow
 ### GitHub Flow
 In our project, we use a structured branching strategy involving `prod`, `master`, and feature branches. The `master` branch serves as the primary development branch where all the feature development takes place. Contributors create feature branches off of `master` for each new feature or bug fix. After completing the development, the changes are merged back into `master` through pull requests.

For releases, we use the `prod` branch, which represents the production-ready state of our application. When it's time to deploy, the latest stable version of code from `master` is merged into `prod`


 #### Updating the Master Branch:

 Here’s how to update the master branch using this workflow:

 1. **Create a Feature Branch:**
    - From  `master` branch, create a new branch. This is where you'll work on your changes.
    ```bash
    git checkout master
    git pull origin master
    git checkout -b your-feature-branch
    ```

 2. **Develop Your Feature:**
    - Make your changes in this feature branch. Commit these changes to the branch.
    ```bash
    git add .
    git commit -m "Your commit message"
    ```

 3. **Create a Pull Request (PR):**
    - Push your branch to the remote repository and open a pull request to the `master` branch.
    ```bash
    git push origin your-feature-branch
    ```
    - On GitHub, create a new PR from `your-feature-branch` to `master` 

 4. **Merge the PR:**
    - After Pull Request review
    - Choose either **"Squash and merge"** or **"Rebase and merge"**.

 #### Updating the Production (Prod) Branch: 

 For deploying to production, the workflow involves merging changes from the `master` branch into a `prod` branch. 

 1. **Create a Pull Request to Prod Branch:** 
    - From the `master` branch, create a new pull request to the `prod` branch. 
    ```bash 
    git checkout master 
    git pull origin master 
    git push origin master  # Ensure the remote master is up to date 
    ``` 
    - On GitHub, create a new PR from `master` to `prod` 

 2. **Merge with a Merge Commit:** 
    - Select "Create a merge commit" while merging the pull request on GitHub.

 3. **Update Local Prod and Master Branches:** 
    - After merging, make sure to update both your local `prod` and `master` branches. This ensures that both branches reflect the current state of the repository after the merge. 
    ```bash 
    # Update local prod branch 
    git checkout prod 
    git pull origin prod 

    # Update local master branch 
    git checkout master 
    git merge origin prod 
    git push origin master 
    ``` 


### Terraform Plan Workflow
- **Trigger**: The workflow is triggered on pull requests to the `master` and `prod` branches.
- **Steps**:
  1. **Checkout**: Checks out the repository code.
  2. **Upload Configuration**: Uploads the Terraform configuration to Terraform Cloud.
  3. **Create Plan Run**: Creates a speculative plan in Terraform Cloud.
  4. **Get Plan Output**: Retrieves the output of the Terraform plan.
  5. **Update PR**: Posts the plan output as a comment in the pull request.

### Terraform Apply Workflow
- **Trigger**: The workflow is triggered on pull requests to the `prod` branch.
- **Steps**:
  1. **Checkout**: Checks out the repository code.
  2. **Upload Configuration**: Uploads the Terraform configuration to Terraform Cloud.
  3. **Create Apply Run**: Creates an apply run in Terraform Cloud.
  4. **Apply**: Applies the Terraform configuration if it is confirmable.

## Additional Information
- **Terraform Configuration**: The provided Terraform configuration manages Google Cloud resources such as compute instances and project services.

## Additional Resources
- [Terraform Cloud Documentation](https://www.terraform.io/cloud-docs)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Automate Terraform with GitHub Actions Tutorial](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
