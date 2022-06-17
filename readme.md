Instructions at [Wiki](https://github.com/ocdevel/diy-cloud-gaming/wiki)

Do-it-yourself Cloud Gaming
Stadia, Luna, GeForce now? Limited libraries. Plus you have your own on Steam, GOG, etc. Monthly lock-in prices, this solution lets you pay by the hour of play. Read more justification here

This terraform module provisions a gaming PC in AWS. You can then use that for cloud-gaming, both traditional or VR (Quest 2). Notes: Original blog post.

On localhost (Mac, PC, whatever)
Clone this repo, setup AWS. See Notes: Non-tech people
Install Terraform.
Edit terraform.tfvars with any edits you want. The documentation for all these variables is in variables.tf, read through that!
terraform init; terraform apply. Review the modifications, yes. This will take a long time because it's waiting for infrastructure > EC2 instance > boot up > password available for decrypt.
TODO fix this: first set terraform.tfvars state="stop"; run terraform apply --auto-approve; then set state="init", apply again
It will output your login credentials to your new Windows PC. Use DCV Viewer to log in. Enter the connection info output from Terraform in terminal (IP, Administrator, Password). Note: Nice DCV woes

On Windows Server (through RDP)
Open Server Manager on Windows

Disable IE enahanced security. Notes: IE Enhanced Security
Local Server → Properties → IE Enhanced Security: Administrators=off
Enable "Wireless Lan". Needed for streaming stuff
Add roles and features
Click through until you get to Features
Checkbox Wireless LAN Service feature
Click Next to move to the end to install the feature
Reboot the EC2 instance in order to complete the installation of the new feature.
After the reboot, revisit Server Manager → Roles and Features. I think you just need to visit that page and click close, it doesn't finalize installation until you go back there.
Download everything; eg Chrome, Steam, etc. Don

Game streaming!
Now the setup work is done, time to get streaming. Read Streaming Setup

When you're done
See the output from Terraform terminal, it gives a command to stop your instance. That way you're not paying for the instance when you're not using it (free when it's shut-down). Or you can stop the instance inside Windows Server (Start > Shut Down). Don't terminate the instance, you'll lose it! You won't be charged for shut-down instances in AWS, so just boot it up later when you wanna play.

Without Terraform
If you don't have a computer to do this from, you can do it all through mobile / whatever. I'll write instructions later, but I recommend looking through the Terraform files and the original blog post to mind-translate the code steps to manual console.aws.com steps. It'll be much harder in console, Terraform strings things together that's not explicit in the code files (eg, Internet Gateway, Route Tables, etc); so if you can use Terraform, please do.

Gotchas
If your AWS account is new (eg, you set it up for this), see Notes: AWS Gotchas. Actually, you may wanna read that anyway.