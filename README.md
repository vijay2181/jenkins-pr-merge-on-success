# jenkins-pr-merge-on-success

- The Aim of this POC is, we need to merge PR only if jenkins build is successful and if build is happening on PR raise, soon if another commit is added into PR, then previous build should fail and new latest changes commit build has to trigger


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
<img width="1223" alt="Screenshot 2024-10-19 at 12 45 03 PM" src="https://github.com/user-attachments/assets/16a5188a-0b31-4360-a6f4-60cdda5b4c18">

<img width="426" alt="Screenshot 2024-10-19 at 11 51 17 AM" src="https://github.com/user-attachments/assets/706a4ac3-3c60-4271-af71-e6b97020873c">

- only select PR's

<img width="346" alt="Screenshot 2024-10-19 at 11 52 04 AM" src="https://github.com/user-attachments/assets/f5aaa2fc-4bdb-4705-b1f1-273f5c739025">

<img width="398" alt="Screenshot 2024-10-19 at 11 52 45 AM" src="https://github.com/user-attachments/assets/51dd5ce9-eb33-4541-a754-c24d99f0a9c5">

<img width="822" alt="Screenshot 2024-10-19 at 12 45 43 PM" src="https://github.com/user-attachments/assets/db6c1f60-5b8f-4044-b8a0-563937ed626d">

### Create a Jenkins Pipeline Job





