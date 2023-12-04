# Git
- [Github's note on GIT](https://docs.github.com/en/get-started/using-git/about-git)
- [Git full command list reference - GitHub](https://git-scm.com/docs)
- [Collection of reusable gitignores from GitHub](https://github.com/github/gitignore)

Git is a distributed version control system that is widely used for tracking changes in source code during software development. It allows multiple developers to work on a project simultaneously, keeping track of changes, and merging their work efficiently. Here are some fundamental concepts and commands in Git:

### Basic Concepts:

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

### Basic Commands:

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
