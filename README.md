# Github Pull Request Integration

- The Aim of this Project is, we need jenkins build only at PR requests.
- we need to merge PR only if jenkins build is successful.
- If Build is failed, then PR pmerge should be blocked
- if build is happening on PR request, soon if another commit is added into PR while build is still in progress, then that in-progress build should fail and new latest changes commit build has to trigger

### Project Objectives

1. **Jenkins Build on Pull Requests Only**: The primary aim is to trigger Jenkins builds exclusively when a pull request (PR) is created or updated. This ensures that the focus remains on validating code changes before they are merged.

2. **Merge Condition Based on Build Success**: Pull requests should only be merged if the Jenkins build completes successfully. This promotes code quality and minimizes the risk of introducing bugs into the main branch.

3. **Block Merging on Build Failure**: If the Jenkins build fails, the PR merge should be blocked. This enforces a safeguard, ensuring that only thoroughly tested code is integrated.

4. **Handling New Commits During Build**: If a new commit is pushed to the PR while the Jenkins build is still in progress, the in-progress build should be marked as failed. Subsequently, a new build should be triggered to validate the latest changes. This allows for continuous feedback and faster iteration.
