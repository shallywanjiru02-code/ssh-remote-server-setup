SSH Remote Server Setup

The objective of this project is to set up a remote Linux server on AWS, configure it to allow
SSH connections, and manage SSH keys for secure access.

I went through the necessary steps to achieve this, including creating SSH key pairs, 
configuring my SSH client, and implementing security measures.

## 1. Server Setup

- Cloud provider: AWS EC2
- Operating system: Ubuntu 26.04 LTS
- Instance type: t3.micro
- Created a remote Ubuntu server on AWS EC2
- Enabled SSH access using key-based authentication

## 2. SSH Key Authentication

I created two SSH key pairs and configured the server to accept both public keys.

### Key 1
- Used the AWS-generated key pair to initially connect to the server.
- Verified that I could connect successfully using SSH.

### Key 2
- Created a second RSA 2048-bit SSH key pair on my local machine.
- Added the public key to the server's `~/.ssh/authorized_keys`.
- Verified that I could connect successfully using the second key.

## 3. SSH Config

I configured my local SSH client to connect to the server using an alias instead of typing the 
full hostname and key path each time.

The alias used was `my-ec2`.

I verified the configuration by connecting successfully with:

`ssh my-ec2`

## 4. Security

I installed and configured Fail2ban to protect the SSH service from repeated failed login 
attempts.

The SSH jail was enabled with:
- Maximum failed attempts: 5
- Ban duration: 1 hour

I verified that the Fail2ban service was active and that the `sshd` jail was running 
successfully.

## 4. Security

I installed and configured Fail2ban to protect the SSH service from repeated failed login 
attempts.

The SSH jail was enabled with:
- Maximum failed attempts: 5
- Ban duration: 1 hour

I verified that the Fail2ban service was active and that the `sshd` jail was running 
successfully.

## 6. Lessons Learned

This project helped me understand how a local machine and a remote Linux server work together 
through SSH.

I learned how to:

- Set up and connect to an AWS EC2 Ubuntu server.
- Understand the difference between my local machine and the remote server.
- Generate and manage SSH key pairs.
- Add a public key to the server's `authorized_keys` file.
- Configure SSH using `~/.ssh/config` and connect using an alias.
- Keep private SSH keys on my local machine and out of GitHub.
- Install and configure Fail2ban to protect SSH.
- Troubleshoot SSH authentication and service configuration errors.
- Use Git and GitHub to document and submit a Linux project.

## 7. Challenges and Shortcomings

The main challenges I faced were understanding which commands should be run on my local machine
and which should be run on the remote server.

I also initially had problems getting the second SSH key to authenticate and encountered a 
Fail2ban configuration error caused by duplicate `sshd` sections.

Working through these problems helped me understand the purpose of SSH keys, `authorized_keys`,
SSH configuration, and Fail2ban instead of simply following commands without understanding them.

One area I still want to improve is becoming more confident with Linux troubleshooting and 
security configuration without relying heavily on documentation or guidance.
                          
#FYI this took me 2+ hours

## Project URL

https://roadmap.sh/projects/ssh-remote-server-setup
