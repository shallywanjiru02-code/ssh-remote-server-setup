# SSH Remote Server Setup

Project: https://roadmap.sh/projects/ssh-remote-server-setup

## Environment Used

- Remote Server: Ubuntu 26.04 LTS on AWS EC2
- Instance Type: t3.micro
- Local Machine: Lenovo T490s running Linux/WSL
- SSH Authentication: AWS-generated key pair + RSA 2048-bit key
- SSH Port: 22

## Steps Taken

### Step 1: Launch EC2 Instance

I created an Ubuntu EC2 instance on AWS.

- Selected a `t3.micro` instance.
- Configured the security group to allow SSH connections on port 22.
- Downloaded the AWS-generated private key pair.
- Obtained the public DNS name of the EC2 instance.

### Step 2: Prepare the SSH Private Key

The AWS private key was originally downloaded to my Windows Downloads folder.

I copied the key to my Linux/WSL home directory:

```bash
cp /mnt/c/Users/ADMN/Downloads/my-key-pair.pem ~/

##  I restricted the permissions of the private key:
chmod 400 my-key-pair.pem
## I verified the permissions:
ls -la ~/my-key-pair.pem

## Step 3: Initial SSH Connection
## I connected to the remote Ubuntu server using the AWS private key:
ssh -i "my-key-pair.pem" ubuntu@ec2-54-175-95-4.compute-1.amazonaws.com

## The connection was successful and I reached the remote Ubuntu server.
## Step 4: Create a Second SSH Key Pair

The second SSH key was generated on my local machine.
ssh-keygen -t rsa -b 2048 -f ~/.ssh/my_second_key

##  This created:
my_second_key — private key
my_second_key.pub — public key

##  The private key remained on my local machine.
##  Step 5: Add the Second Public Key to the Server
##  I used the original AWS private key to authenticate while copying the 
second public key to the remote server:

ssh-copy-id -f -i ~/.ssh/my_second_key.pub -o IdentityFile=~/my-key-pair.pem 
ubuntu@ec2-54-175-95-4.compute-1.amazonaws.com

##  I verified the authorized keys on the remote server:
cat ~/.ssh/authorized_keys

##  Step 6: Test the Second SSH Key
##  I tested the second key independently:
ssh -i ~/.ssh/my_second_key ubuntu@ec2-54-175-95-4.compute-1.amazonaws.com
##  The connection was successful, confirming that the second SSH key could be used to access 
the server.

##  Step 7: Configure SSH Client Alias
##  On my local machine, I created the SSH configuration file:
nano ~/.ssh/config
##  I configured the following alias:
Host my-ec2
    HostName ec2-54-175-95-4.compute-1.amazonaws.com
    User ubuntu
    IdentityFile ~/my-key-pair.pem
##  I then connected using the alias:
ssh my-ec2
##  The connection was successful.

##  Step 8: Install Fail2ban
##  As the stretch goal, I installed Fail2ban on the remote Ubuntu server:
sudo apt update
sudo apt install fail2ban
##  I enabled Fail2ban to start automatically:
sudo systemctl enable fail2ban

## Step 9: Configure Fail2ban
## I created a custom SSH jail configuration:
sudo nano /etc/fail2ban/jail.d/sshd.local
##  The configuration was:
[sshd]
enabled = true
maxretry = 5
bantime = 1h
I tested the configuration:
sudo fail2ban-client -t
##  The configuration test was successful.

##  Step 10: Troubleshoot Fail2ban
## Initially, Fail2ban failed to start because there were duplicate [sshd] sections in the configuration.
##  I checked the service logs:
sudo journalctl -u fail2ban --no-pager -n 30
## The logs identified the duplicate [sshd] configuration.
##  I removed the conflicting local configuration and tested Fail2ban again:
sudo rm /etc/fail2ban/jail.local
sudo fail2ban-client -t
##  The result was:
##  OK: configuration test is successful
##  I restarted Fail2ban:
sudo systemctl restart fail2ban
##  Then verified the service:
sudo systemctl status fail2ban
##  The service was successfully running.
##  Step 11: Verify the SSH Jail
##  I verified that the SSH jail was active:
sudo fail2ban-client status sshd
## The sshd jail was active and monitoring SSH authentication attempts.


