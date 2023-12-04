# Git
- [Github's note on GIT](https://docs.github.com/en/get-started/using-git/about-git)
- [Git full command list reference - GitHub](https://git-scm.com/docs)
- [Collection of reusable gitignores from GitHub](https://github.com/github/gitignore)

Git is a distributed version control system that is widely used for tracking changes in source code during software development. It allows multiple developers to work on a project simultaneously, keeping track of changes, and merging their work efficiently. Here are some fundamental concepts and commands in Git:

## Basic Concepts:

1. **Repository (Repo):**
   - A repository is a directory or storage space where your projects can live. It can exist locally on your machine or remotely on a server like GitHub.

2. **Commit:**
   - A commit is a snapshot of the changes to the repository. Each commit has a unique identifier (hash) and includes information about what changes were made.

3. **Branch:**
   - A branch is a parallel version of a repository. It allows you to work on different features or bug fixes independently from the main codebase.

4. **Merge:**
   - Merging is the process of combining changes from one branch into another. It is typically used to incorporate changes from feature branches into the main branch.

5. **Pull Request (PR):**
   - A pull request is a way to propose changes to a repository. It is often used when working in a collaborative environment to review and discuss code before merging it into the main branch.

## Basic Commands:

1. **Initialize a Repository:**
   ```bash
   git init
   ```

2. **Clone a Repository:**
   ```bash
   git clone <repository_url>
   ```

3. **Check Status:**
   ```bash
   git status
   ```

4. **Add Changes to Staging:**
   ```bash
   git add <file(s)>
   ```

5. **Commit Changes:**
   ```bash
   git commit -m "Your commit message"
   ```

6. **Create a Branch:**
   ```bash
   git branch <branch_name>
   ```

7. **Switch to a Branch:**
   ```bash
   git checkout <branch_name>
   ```

   (or using `git switch` in recent Git versions)
   ```bash
   git switch <branch_name>
   ```

8. **Merge Changes from Another Branch:**
   ```bash
   git merge <branch_name>
   ```

9. **Pull Changes from a Remote Repository:**
   ```bash
   git pull origin <branch_name>
   ```

10. **Push Changes to a Remote Repository:**
    ```bash
    git push origin <branch_name>
    ```

11. **View Commit History:**
    ```bash
    git log
    ```

12. **Create a Pull Request (GitHub/GitLab):**
    - Create a new branch with your changes, push it to your fork, and then open a pull request on the original repository.

These are just the basics, and Git has a rich set of features for managing more complex workflows. It's highly recommended to explore Git documentation and tutorials to get a deeper understanding of its capabilities.

## Git Basics

### 1. Initializing a Git Repository

To start using Git for version control in a project, you need to initialize a Git repository. Follow these steps:

- **Initialize a New Repository:**
  - Open a terminal in your project's root directory.
  - Run the following command:

    ```bash
    git init
    ```

  This command initializes a new Git repository in the current directory.

- **Clone an Existing Repository:**
  - To clone an existing repository from a remote source (like GitHub), use:

    ```bash
    git clone https://github.com/example/repository.git
    ```

  Replace the URL with the actual repository URL.

### 2. Committing Changes and Viewing Commit History

Once your repository is set up, you can start making changes and committing them to the version history.

- **Staging Changes:**
  - Use the following command to stage changes for the next commit:

    ```bash
    git add file1 file2 ...
    ```

    Replace `file1`, `file2`, `.` - "for all files" etc., with the names of the files you want to stage.

- **Committing Changes:**
  - After staging changes, commit them to the repository:

    ```bash
    git commit -m "Your commit message"
    ```

    Replace `"Your commit message"` with a descriptive message summarizing your changes.

- **Viewing Commit History:**
  - To view the commit history, use:

    ```bash
    git log
    ```

    This command shows a list of commits, including the commit hash, author, date, and commit message.

- **Viewing Changes in the Working Directory:**
  - To see the changes in your working directory that are not yet staged, use:

    ```bash
    git status
    ```

    This command provides information about untracked, modified, and staged files.

- **Viewing Changes in a Specific Commit:**
  - If you want to see the changes introduced in a specific commit, use:

    ```bash
    git show commit_hash
    ```

    Replace `commit_hash` with the actual commit hash you want to inspect.

Understanding these Git basics is crucial for effective version control. Initiating a repository, committing changes, and reviewing commit history are fundamental actions that form the foundation of collaborative development workflows.

## Branching and Merging

### 1. Creating and Managing Branches

Effective branching strategies help organize development efforts and facilitate collaboration. Here's how to create and manage branches:

- **Creating a New Branch:**
  - To create a new branch, use the following command:

    ```bash
    git checkout -b feature-branch
    ```

    Replace `feature-branch` with the name of your new branch.

- **Switching Between Branches:**
  - Use the following command to switch between branches:

    ```bash
    git checkout existing-branch
    ```

    Replace `existing-branch` with the name of the branch you want to switch to.

- **Listing Branches:**
  - To see a list of existing branches and identify the current branch, use:

    ```bash
    git branch
    ```

### 2. Merging Changes and Resolving Conflicts

Merging is a crucial aspect of collaboration. It involves combining changes from different branches. Here's how to manage the process:

- **Merging Branches:**
  - To merge changes from one branch into another, use:

    ```bash
    git checkout target-branch
    git merge feature-branch
    ```

    Replace `target-branch` with the branch you want to merge changes into.

- **Resolving Merge Conflicts:**
  - If there are conflicting changes between branches, Git will indicate a conflict.
  - Manually resolve conflicts in the affected files by editing them.
  - After resolving conflicts, mark them as resolved:

    ```bash
    git add conflicted-file
    ```

  - Complete the merge:

    ```bash
    git merge --continue
    ```

- **Aborting a Merge:**
  - If you encounter issues during a merge and want to start over, use:

    ```bash
    git merge --abort
    ```

- **Deleting a Branch:**
  - Once changes from a branch are merged, you can delete the branch:

    ```bash
    git branch -d feature-branch
    ```

    The `-d` option stands for delete.

Branching and merging are fundamental concepts in Git that allow teams to work concurrently on different features or bug fixes. Resolving conflicts is an inevitable part of collaboration, and understanding how to manage branches and merges efficiently is key to a successful version control workflow.


## Collaborative Workflows

### 1. Forking and Cloning Repositories

Collaboration often begins with forking and cloning repositories. Follow these steps to contribute to a project:

- **Forking a Repository:**
  - Visit the repository on GitHub (or your preferred platform).
  - Click the "Fork" button in the top-right corner.
  - This creates a copy of the repository under your GitHub account.

- **Cloning a Forked Repository:**
  - Copy the URL of your forked repository.
  - Open your terminal and use the following command:

    ```bash
    git clone https://github.com/YourUsername/repository.git
    ```

    Replace `YourUsername` and `repository` with your GitHub username and the repository name.

### 2. Pull Requests and Code Reviews

Pull requests (or merge requests) and code reviews are crucial for maintaining code quality and collaboration.

- **Creating a Pull Request:**
  - After making changes in your local repository, push them to your fork on GitHub.
  - Navigate to your fork on GitHub and click on the "New pull request" button.
  - Select the branches you want to merge and create the pull request.

- **Code Reviews:**
  - Team members review your code within the pull request.
  - Address feedback and make necessary changes.
  - Continue the iterative review process until the code is approved.

- **Handling Merge Conflicts:**
  - If there are conflicting changes between your branch and the target branch, resolve them locally.
  - Push the resolved changes to your fork and update the pull request.

- **Merging Pull Requests:**
  - Once the pull request is approved and passes continuous integration tests, it can be merged.
  - Use the "Merge" button on GitHub to incorporate changes into the target branch.

- **Branch Cleanup:**
  - After a successful merge, delete the feature branch (if not needed for future reference).

By following these collaborative workflows, you contribute to projects, receive feedback, and ensure that changes meet project standards before being integrated. This promotes a smooth and organized development process within a team.


## Git Best Practices

### 1. Guidelines for Clean and Effective Version Control

Maintaining a clean and effective version control history is essential for collaboration and project management. Here are some best practices:

- **Atomic Commits:**
  - Make each commit a self-contained, atomic change.
  - Avoid combining unrelated changes in a single commit.

- **Descriptive Commit Messages:**
  - Write clear and concise commit messages.
  - Include a brief summary and, if needed, additional details.

- **Frequent Commits:**
  - Commit regularly to capture logical units of work.
  - Avoid large commits that make it difficult to understand changes.

- **Use Branches Wisely:**
  - Create feature branches for new development.
  - Use topic branches for bug fixes or specific tasks.
  - Merge branches when the work is complete and tested.

### 2. Commit Message Conventions and Branching Strategies

A consistent commit message format and branching strategy enhance collaboration and code maintenance.

- **Commit Message Conventions:**
  - Start with a concise one-line summary (50 characters or less).
  - Optionally, provide a more detailed description after a blank line.
  - Use present tense and imperative mood (e.g., "Add feature" instead of "Added feature").
  - Reference relevant issues or tickets.

  Example:
  ```plaintext
  Add feature XYZ

  This commit adds a new feature XYZ to improve user experience.
  Fixes #123.
  ```

- **Branching Strategies:**
  - Follow a branching model that fits your project (e.g., Gitflow, GitHub Flow).
  - Main branches:
    - `main` or `master`: Stable and deployable code.
    - `develop`: Ongoing development and integration.
  - Feature branches:
    - Create a new branch for each feature or bug fix.
    - Merge branches into `develop` when complete.
  - Release branches (if applicable):
    - Prepare for a release in a dedicated branch.
    - Merge into `main` or `master` and `develop` after testing.

- **Pull Requests:**
  - Use pull requests or merge requests for code reviews.
  - Discuss changes before merging.
  - Leverage CI/CD to ensure quality.

By adhering to these Git best practices, you can create a more collaborative and maintainable version control history for your projects.