# aws-ubuntu-workstation
This terraform script deploys an Ubuntu Workstation with minimal additional software
installed.

## AWS CloudShell Setup
Open new CloudShell environment.
Install Terraform from - https://releases.hashicorp.com/terraform/

```
wget https://releases.hashicorp.com/terraform/1.8.2/terraform_1.8.2_linux_amd64.zip
unzip terraform*.zip
mkdir ~/bin
mv terraform ~/bin/
rm terraform*.zip
```

Configure the User Profile in the CloudShell.

```
aws configure --profile myterraform
```
Paste "aws ubuntu-workstation-user" Access key ID from Bitwarden and then the Secret Access Key.
Enter ```eu-central-1``` for the Default region name and ```json``` for the Default output format.
Test it by running the following command:
```
aws sts get-caller-identity --profile myterraform
```

Make a copy of the PEM file on CloudShell in ~/ from Bitwardem "aws_ubuntu_workstation.pem"

```
vim aws_ubuntu_workstation.pem
```

Useful ```vim``` commands:
- Press "i" key to enter insert mode.
- Press ```ESC``` to exit insert mode.
- Save file and exit ```:wq```.
- Exit without saving file ```:q!```

Change PEM file permissions

```
chmod 400 ~/aws_ubuntu_workstation.pem
```

```ls -als``` to ensure file permissions have changed to read only

```
-r--------. 1 cloudshell-user cloudshell-user 1675 May  6 06:54 aws_ubuntu_workstation.pem
```

## DEPLOYMENT
Git clone the repository and provision the cloud workstation by running the following
commands, one at a time:

```
git clone  https://github.com/jdalexan32/aws-ubuntu-workstation.git
```
and then

```
cd aws-ubuntu-workstation
```

Next modify the `terraform.tfvars` contents with the settings that are appropriate
for your AWS Account.

Now run these commands:

```
terraform init
terraform apply
```

Obviously, this assumes that you have both `git` and `terraform` installed.

## USAGE
The cloud workstation can be accessed via either SSH or RDP. The necessary information
will be an output when the script is complete.


## ADDITIONAL NOTES
The `workstation1.sh` is passed as user data to the instance and runs the first time
the system boots. The script generates a log entry for almost every command as it runs
and sends it to `/tmp/first-boot.log`

Since the script installs updates and some software, it may take a while. I like to watch
the first boot script by watching its log when connected via SSH. Just run:

```
tail -f /tmp/first-boot.log
```

Otherwise, if you are connected via SSH, you will get a system message when the boot
script has completed:

```
NOTICE: First Boot Setup Has Completed
```

After that point it is ready for an Remote Desktop (RDP) Connection. Use an RDP client,
such as Microsoft Terminal Services Client (MSTSC) to RDP to the IP address that is output
by the Terraform script.

**DON'T FORGET TO CHANGE THE RDP PASSWORD AFTER LOGON**
