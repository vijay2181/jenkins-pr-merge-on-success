# jenkins-pr-merge-on-success

- The Aim of this POC is, we need to merge PR only if jenkins build is successful and if build is happening on PR raise, soon if another commit is added into PR, then previous build should fail and new latest changes commit build has to trigger
- This is only for Single branch pipeline
- we will use "Generic Webhook Trigger Plugin"


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
- install below plugin after login

<img width="1304" alt="Screenshot 2024-10-19 at 5 19 20 PM" src="https://github.com/user-attachments/assets/927652bd-0fe8-448d-ae54-9c6082a65142">

### create a pipeline

<img width="480" alt="Screenshot 2024-10-19 at 5 29 12 PM" src="https://github.com/user-attachments/assets/c6dd469c-8e59-4b06-98b9-fe085614ba2c">

### Manual Trigger
- we can trigger build manually, but our aim is trigger only on a PR raise

### Generic Webhook Trigger

<img width="1141" alt="Screenshot 2024-10-19 at 5 30 41 PM" src="https://github.com/user-attachments/assets/a393e082-f55c-4069-8146-b6292bb8dd3a">


```
- goto build triggers section of job and click on Generic webhook trigger

https://github.com/jenkinsci/generic-webhook-trigger-plugin/tree/master/src/test/resources/org/jenkinsci/plugins/gwt/bdd/github

https://github.com/jenkinsci/generic-webhook-trigger-plugin/blob/master/src/test/resources/org/jenkinsci/plugins/gwt/bdd/github/github-pull-request.feature

- we get get below values from webhook payload

Feature: It should be possible to trigger for GitHub pull request events.

  Scenario: Trigger a job when pull request is opened, reopened and synchronized.

    Given the following generic variables are configured:
      | variable        | expression                       | expressionType  | defaultValue | regexpFilter  |
      | action          | $.action                         | JSONPath        |              |               |
      | pr_id           | $.pull_request.id                | JSONPath        |              |               |
      | pr_state        | $.pull_request.state             | JSONPath        |              |               |
      | pr_title        | $.pull_request.title             | JSONPath        |              |               |
```

- select below options

- in the "Post content parameters" section itself add below 4 values

```
      | variable        | expression                       | expressionType  | defaultValue | regexpFilter  |
      | action          | $.action                         | JSONPath        |              |               |
      | pr_id           | $.pull_request.id                | JSONPath        |              |               |
      | pr_number       | $.pull_request.number            | JSONPath        |              |               |
      | pr_state        | $.pull_request.state             | JSONPath        |              |               |

```

<img width="1533" alt="Screenshot 2024-10-19 at 5 37 24 PM" src="https://github.com/user-attachments/assets/8647fb33-e57e-4e4f-8096-31b57c66a20f">

<img width="1540" alt="Screenshot 2024-10-19 at 5 38 05 PM" src="https://github.com/user-attachments/assets/1c44a759-f518-4007-9d3e-f6b5acf01070">

<img width="1576" alt="Screenshot 2024-10-19 at 5 39 06 PM" src="https://github.com/user-attachments/assets/c3a6adf3-fd60-4de0-b92c-037a87a6541f">


<img width="1536" alt="Screenshot 2024-10-19 at 5 39 36 PM" src="https://github.com/user-attachments/assets/4abac348-230b-4d33-86bc-60923119b287">

-> u can add token for security reason or else optional
-> this token should match at webhook setup

<img width="1611" alt="Screenshot 2024-10-19 at 5 42 35 PM" src="https://github.com/user-attachments/assets/eb2f5cd8-0322-49e9-a3a4-431f126b6ba3">

<img width="1145" alt="Screenshot 2024-10-19 at 5 43 44 PM" src="https://github.com/user-attachments/assets/53d8241f-ab5a-44ff-a121-2dd4570d47ba">

<img width="1238" alt="Screenshot 2024-10-19 at 5 44 31 PM" src="https://github.com/user-attachments/assets/58d794d5-20cc-4d03-905b-d4488aec755c">


### Configure SCM

<img width="1569" alt="Screenshot 2024-10-19 at 5 45 17 PM" src="https://github.com/user-attachments/assets/1dd729bf-ed81-4011-b7f2-72d2fcc18e87">

- APPLY AND SAVE
- Note:
- In the Jenkins job configuration, the "Branches to build" setting typically specifies which branches should be built by the job. When dealing with pull requests (PRs), you generally want this setting to adapt dynamically to the PR's source branch, rather than being hardcoded to a specific branch like main or feature-add-changes. so to get that dynamic nature you should go with multi branch pipeline


### Configure Webhook

- the token should match with build trigger token you configured previously
  
<img width="1343" alt="Screenshot 2024-10-19 at 5 46 36 PM" src="https://github.com/user-attachments/assets/d2a3188d-909d-4225-882a-fbbf70559947">

<img width="335" alt="Screenshot 2024-10-19 at 5 47 10 PM" src="https://github.com/user-attachments/assets/e3b3b17a-7432-4a7a-b287-f9ab38754d06">

<img width="427" alt="Screenshot 2024-10-19 at 5 47 36 PM" src="https://github.com/user-attachments/assets/24f34e97-87b9-43f9-8eb6-78f9aab59de7">

<img width="853" alt="Screenshot 2024-10-19 at 5 48 00 PM" src="https://github.com/user-attachments/assets/f8a0626f-1353-471b-8292-fd6591647111">


### Success Implementation

- create a "feature-add-changes" branch from main branch
- goto jenkins and add this branch to build in job configuration

<img width="1219" alt="Screenshot 2024-10-19 at 6 05 39 PM" src="https://github.com/user-attachments/assets/9112d6c8-6c58-4d57-b8dd-f0855811c87c">



<img width="385" alt="Screenshot 2024-10-19 at 5 49 16 PM" src="https://github.com/user-attachments/assets/0713f1e1-da1e-4cfd-b77d-056b380837e1">

- add some changes and raise PR, Build will only trigger when u raise PR and add commitsto that PR

<img width="944" alt="Screenshot 2024-10-19 at 5 52 00 PM" src="https://github.com/user-attachments/assets/54d21e95-04f8-4655-9664-d70ea413e53e">

<img width="962" alt="Screenshot 2024-10-19 at 5 53 04 PM" src="https://github.com/user-attachments/assets/546cdf9e-f583-45d5-9468-13ea360f4e6a">

- when you click on "Create Pull request", build will trigger

<img width="345" alt="Screenshot 2024-10-19 at 5 53 49 PM" src="https://github.com/user-attachments/assets/c5293d8f-8c16-48a7-a040-b6b52c8e415a">

<img width="999" alt="Screenshot 2024-10-19 at 5 54 49 PM" src="https://github.com/user-attachments/assets/6ca0b023-c04e-4233-aaa8-98b259cd2c4d">

- Build got success, thats why you are able to see "Merge Pull Request"
- If Build got failed, then it should Block "Merge Pull Request"


### Failure Implementation
- If Build got failed, then it should Block "Merge Pull Request"
- intentionall fail the build by adding false changes in "feature-add-changes" branch and commit

<img width="345" alt="Screenshot 2024-10-19 at 6 09 37 PM" src="https://github.com/user-attachments/assets/f31c10c6-706a-48d8-9789-2cf30f956701">

- build is failed, but if you goto PR, eventhough build is failed, still you are able to merge

<img width="953" alt="Screenshot 2024-10-19 at 6 11 03 PM" src="https://github.com/user-attachments/assets/5530b130-7f7a-477c-859e-baf77714c3ac">

- so we need to get a feedback from jenkins to pr status
- i have added changes in jenkinsfile
- so when build got failed, pr merge will be blocked
- when we raise pr make sure we chnage branch name in Jenkinsfile

<img width="779" alt="Screenshot 2024-10-19 at 11 10 02 PM" src="https://github.com/user-attachments/assets/75e26e77-3546-44c5-9508-c10f04e90b0a">

<img width="1160" alt="Screenshot 2024-10-19 at 11 13 36 PM" src="https://github.com/user-attachments/assets/72ddb61c-aa3f-4d89-bbc1-978e817d47f9">

<img width="987" alt="Screenshot 2024-10-19 at 11 08 44 PM" src="https://github.com/user-attachments/assets/e56e755e-fc8d-4495-9448-dccb01a2333a">

<img width="965" alt="Screenshot 2024-10-19 at 11 12 27 PM" src="https://github.com/user-attachments/assets/12378986-f445-4a83-9ea6-2d3f038264b1">

























































































































































