# github-pr-merge-on-success-from-jenkins-status

- The Aim of this POC is, we need to merge PR only if jenkins build is successful and if build is happening on PR raise, soon if another commit is added into PR, then previous build should fail and new latest changes commit build has to trigger
- This is for Multi branch pipeline

```
Install jenkins on aws:

sudo apt update -y
sudo apt-get install openjdk-17-jdk -y
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl status jenkins
```

- create a multi branch pipeline

<img width="1384" alt="Screenshot 2024-10-20 at 7 28 40 PM" src="https://github.com/user-attachments/assets/04dd9c07-df2a-4123-a27a-e78ca754c2df">

- add github branch sources

<img width="1559" alt="Screenshot 2024-10-20 at 7 32 21 PM" src="https://github.com/user-attachments/assets/7833c8e8-1615-4a23-b749-6c7c404a5521">

- add behavious

<img width="1606" alt="Screenshot 2024-10-20 at 7 34 04 PM" src="https://github.com/user-attachments/assets/38ceef04-9f68-462a-9211-dd8d95572701">

- add regular expression to build only PR branches not regualar branches, in multi branch pipeline, each pr is a branch
- [*] will build all regualr and PR branches
- PR-[0-9]+ this will only build PR branches
- you can also add "Git LFS pull after checkout" option if you have lfs files in the git repo
- rest keep as it is
  
<img width="1601" alt="Screenshot 2024-10-20 at 7 37 16 PM" src="https://github.com/user-attachments/assets/a5b79ae3-1599-4ac4-b2c2-5c657672eb66">

- discard old builds

<img width="1628" alt="Screenshot 2024-10-20 at 7 41 29 PM" src="https://github.com/user-attachments/assets/329ea771-2669-4c42-83a9-7eb181aa413d">

- we need to make sure point the correct Jenkinsfile path, otherwise build wont hapeen, multi barnch pipeline will trigger only if you have Jenkinsfile

<img width="1209" alt="Screenshot 2024-10-20 at 8 14 17 PM" src="https://github.com/user-attachments/assets/922fd17f-7174-406c-b85e-4c3d9caadaf4">


- apply and save, now multi barnch pipeline will scan for any PR branches to build
- if you already have PR's then it will build all the PR's

<img width="1656" alt="Screenshot 2024-10-20 at 7 43 31 PM" src="https://github.com/user-attachments/assets/5ed2ebf6-b040-4aba-b43a-adcc799c6b5d">


### Configure WEBHOOK 

<img width="1653" alt="Screenshot 2024-10-20 at 7 49 33 PM" src="https://github.com/user-attachments/assets/a3ea7932-689b-48a0-8524-5b812dcd3a1b">

<img width="376" alt="Screenshot 2024-10-20 at 7 52 38 PM" src="https://github.com/user-attachments/assets/83cf0050-07df-4638-9c98-96bb8840fe10">

<img width="827" alt="Screenshot 2024-10-20 at 7 53 20 PM" src="https://github.com/user-attachments/assets/32316fc2-23fe-406b-aadb-678fde6118c0">


### Create a PR

- create a feature-add-change branch from main, do some changes and raise PR

<img width="1246" alt="Screenshot 2024-10-20 at 8 04 55 PM" src="https://github.com/user-attachments/assets/988a429c-60f0-4c0a-adbb-d8cbb5a638c0">

<img width="1124" alt="Screenshot 2024-10-20 at 8 05 46 PM" src="https://github.com/user-attachments/assets/53e4f65b-b007-4312-b493-36fb34fa25b3">

- create PR, jenkins will scan the repo and starts the build

<img width="1209" alt="Screenshot 2024-10-20 at 8 14 17 PM" src="https://github.com/user-attachments/assets/fe24249c-cc91-4ac4-8453-21615092c3c8">

- you can see build in PR section

<img width="763" alt="Screenshot 2024-10-20 at 8 25 05 PM" src="https://github.com/user-attachments/assets/de8eed52-58c6-44d6-9d51-fb3c2fab30bc">

<img width="1665" alt="Screenshot 2024-10-20 at 8 29 37 PM" src="https://github.com/user-attachments/assets/39638d1d-9986-4e44-9376-4ac744d80589">

- set github-token

<img width="1665" alt="Screenshot 2024-10-20 at 8 29 37 PM" src="https://github.com/user-attachments/assets/52695493-bddb-41df-8ecd-592a6c625cfe">


### update Jenkins build status in GitHub pull requests
- we need to get jenkins build status back to github PR, so that if build got success, then only allow to merge otherwise block merge
- Jenkins Multi branch pipeline has defalt integration to status check
- if you want you can create own context for status check

```
GitHub's default integration with Jenkins can automatically create a status check named continuous-integration/jenkins/pr-head.

GitHub can automatically create status checks named continuous-integration/jenkins/pr-head when a Jenkins job is configured
to build pull requests using the GitHub branch source plugin. This happens when you configure a Jenkins multibranch pipeline
job with a GitHub source and have it set to build pull requests.
```

<img width="1650" alt="Screenshot 2024-10-20 at 9 30 32 PM" src="https://github.com/user-attachments/assets/cd1e5f25-b628-458b-b852-1ebbd9c98176">

### Fail the status check
- if build is failed, then it should fail status check
- even though status check is failed, still we can able to merge
- we should block merge if status check fails

<img width="976" alt="Screenshot 2024-10-20 at 9 40 34 PM" src="https://github.com/user-attachments/assets/fbf8fe7b-a95e-4c20-a36f-fe0eb1dffca6">


### Block PR to Merge
- if build is failed, then it should block the PR merge
- we need to ensure that a pull request cannot be merged if the status check fails, we need to configure the required status checks in our GitHub repository settings. Here is a step-by-step guide:

### Step 1: Configure GitHub Repository

1. **Navigate to Repository Settings**:
   - Go to your GitHub repository.
   - Click on `Settings`.

2. **Branch Protection Rules**:
   - In the left sidebar, click on `Branches`.
   - Under `Branch protection rules`, click on `Add rule`.

3. **Set Up Rule**:
   - Specify the branch name pattern you want to protect (e.g., `main` or `master`).
   - Check the box `Require status checks to pass before merging`.
   - Select the status check `continuous-integration/jenkins/pr-head` from the list.
   - Optionally, check `Require branches to be up to date before merging` to ensure the branch is up-to-date with the base branch before it can be merged.
   - Click on `Create`


<img width="1651" alt="Screenshot 2024-10-20 at 9 48 57 PM" src="https://github.com/user-attachments/assets/c3b43699-9de4-439c-8607-3df37e86d974">

<img width="1437" alt="Screenshot 2024-10-20 at 9 49 51 PM" src="https://github.com/user-attachments/assets/0339b883-5572-49c0-803a-db3fc5c9e379">

- goto your PR, now the merge will be blocked for failed status


<img width="989" alt="Screenshot 2024-10-20 at 9 50 34 PM" src="https://github.com/user-attachments/assets/8aec9f02-3594-4398-bb30-7101e2fb3192">

- if build is success then it will allow to merge
  
<img width="973" alt="Screenshot 2024-10-20 at 9 52 36 PM" src="https://github.com/user-attachments/assets/aa13bb8b-859d-48b4-bc0c-1a167bed511d">






































