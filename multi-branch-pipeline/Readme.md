# jenkins-pr-merge-on-success

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
- * will build all regualr and PR branches
- PR-[0-9]+ this will only build PR branches
- you can also add "Git LFS pull after checkout" option if you have lfs files in the git repo
- rest keep as it is
  
<img width="1601" alt="Screenshot 2024-10-20 at 7 37 16 PM" src="https://github.com/user-attachments/assets/a5b79ae3-1599-4ac4-b2c2-5c657672eb66">

- discard old builds

<img width="1628" alt="Screenshot 2024-10-20 at 7 41 29 PM" src="https://github.com/user-attachments/assets/329ea771-2669-4c42-83a9-7eb181aa413d">

- apply and save, now multi barnch pipeline will scan for any PR branches to build
- if you already have PR's then it will build all the PR's

<img width="1656" alt="Screenshot 2024-10-20 at 7 43 31 PM" src="https://github.com/user-attachments/assets/5ed2ebf6-b040-4aba-b43a-adcc799c6b5d">


### Configure WEBHOOK 

<img width="1653" alt="Screenshot 2024-10-20 at 7 49 33 PM" src="https://github.com/user-attachments/assets/a3ea7932-689b-48a0-8524-5b812dcd3a1b">

<img width="376" alt="Screenshot 2024-10-20 at 7 52 38 PM" src="https://github.com/user-attachments/assets/83cf0050-07df-4638-9c98-96bb8840fe10">

<img width="827" alt="Screenshot 2024-10-20 at 7 53 20 PM" src="https://github.com/user-attachments/assets/32316fc2-23fe-406b-aadb-678fde6118c0">




























