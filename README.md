# Azure-Linux-Virtual-Machine
Create Azure Linux VM using Terraform

Pre-requisite Note: Create SSH Keys for Azure Linux VM

# Create Folder
cd terraform-manifests/
mkdir ssh-keys

# Create SSH Key
cd ssh-keys
ssh-keygen \
    -m PEM \
    -t rsa \
    -b 4096 \
    -C "azureuser@myserver" \
    -f terraform-azure.pem 
Important Note: If you give passphrase during generation, during everytime you login to VM, you also need to provide passphrase.

# List Files
ls -lrt ssh-keys/

# Files Generated after above command 
Public Key: terraform-azure.pem.pub -> Rename as terraform-azure.pub
Private Key: terraform-azure.pem

# Permissions for Pem file
chmod 400 terraform-azure.pem

Step-08: Execute Terraform Commands

# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
Step-09: Verify Resources

# Verify Resources - Virtual Network
1. Azure Resource Group
2. Azure Virtual Network
3. Azure Subnets (Web, App, DB, Bastion)
4. Azure Network Security Groups (Web, App, DB, Bastion)
5. View the topology
6. Verify Terraform Outputs in Terraform CLI

# Verify Resources - Web Linux VM 
1. Verify Public IP created for Web Linux VM
2. Verify Network Interface created for Web Linux VM
3. Verify Web Linux VM
4. Verify Network Security Groups associated with VM (web Subnet NSG and NIC NSG)
5. View Topology at Web Linux VM -> Networking
6. Connect to Web Linux VM
ssh -i ssh-keys/terraform-azure.pem azureuser@<Web-LinuxVM-PublicIP>
sudo su - 
cd /var/log
tail -100f cloud-init-output.log
cd /var/www/html
ls -lrt
cd /var/www/html/app1
ls -lrt
exit
exit

7. Access Sample Application
http://<PUBLIC-IP>/
http://<PUBLIC-IP>/app1/index.html
http://<PUBLIC-IP>/app1/hostname.html
http://<PUBLIC-IP>/app1/status.html
http://<PUBLIC-IP>/app1/metadata.html
Step-10: Comment NSG associated with VM

# Comment code in c7-04-web-linuxvm-network-security-group.tf
NSG associated with Web Linux VM NIC is commented

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify NSG associated with VM
1. Verify Network Security Groups associated with VM (web Subnet NSG only)
2. Access Application
http://<PUBLIC-IP>/app1/metadata.html
Step-11: Delete Resources

# Delete Resources
terraform destroy 
[or]
terraform apply -destroy -auto-approve

# Clean-Up Files
rm -rf .terraform* 
rm -rf terraform.tfstate*
Step-12: Additional Cautionary Note

If we don't disassociate the NSG associated with VM first before destroying resources and directly destroy all resources we might face the below error.
At that point we need to manually delete the resource group directly in Azure Portal Console
This is due to Terraform Azure Provider not able to handle dependencies effectively
# Destroy Resources
terraform destroy -auto-approve

# Manually Delete the RG
Login to Azure Mgmt Console 
Go to Resource Groups -> hr-dev-rg -> Delete

# Clean-Up Files
rm -rf .terraform* 
rm -rf terraform.tfstate*
