# Immutable Infrastructure Workflow
This repo higlights how to use Ansible & Red Hat Insights as part of an automated build process for immutable system images -- creating an automated build using the following:

**Travis CI**  
This will watch for commits to the GitHub repo where our system configuration is stored

**Packer**  
This will handle creating the AMI based on the packer.json spec we've created

**Ansible**  
This will use our existing system configuration roles and ensure the image is configured as desired

**AWS EC2**
Chosen for simplicity's sake, as the tools will support created other types of cloud images

**Red Hat Insights**  
A tool specifically for Red Hat Enterprise Linux that performs automated discovery of important issues

The basic workflow here is:
1. Commit Ansible playbooks or other content to a branch starting with 'config' on GitHub
2. Travis will watch for branches as specified and attempt to perform a build using Packer
3. Packer will use the Ansible playbooks to configure the system
4. If the build passes, we will merge the into the master branch and create an AMI marked for production use

In this example, I use variables for RHSM registration which are stored in a file `rhsm_vars.yml`.

To create this file, simply create a file rhsm_vars.yml in your working directory and populate it with the following:

```yaml
rhsm_activation_key:
rhsm_org:
rhsm_username: your_rh_username
rhsm_password: your_rh_password
rhsm_pool_id: your_rhsm_pool
```
If you're unsure about the values here, review the [Red Hat Subscription Management Guide](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html-single/rhsm/index)

## Next Steps
After this workflow is running there are lots of things you can do to ensure your environment uses only successful builds here including:
* Policy enforcement via CloudForms
* Smart Inventory discovery via Ansible Tower
