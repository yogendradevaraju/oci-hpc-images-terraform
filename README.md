# oci-hpc-images-ansible
_Custom OCI HPC image customization using Ansible playbooks_

## Custom Image Build Workflow (Pure Ansible)

### 1. Configure Default Variables

Using ansible/vars/defaults.yml.example create a new version of the file: `defaults.yml` and fill in the variables.

### 2. Create a Custom Image Vars File

Create a new YAML file for your custom image in the ansible/vars/ directory using one of the example files as a template and fill in the variables based on your requirement:
```
cp ansible/vars/example-image.yml ansible/vars/my-custom-image.yml
```
In the my-custom-image.yml file, you will need to edit the image_id for your region. OCIDs can be found here, pick the image OCID based on your region: https://docs.oracle.com/en-us/iaas/images/

### 3. Update builder_spec.yaml to Use the Custom Vars File

Edit the builder_spec.yaml file and update the Ansible playbook command under the “run image build playbook” section with the path to your new vars file. Replace my-custom-image.yml with your file name:
```
ansible-playbook ./ansible/builder.yml -e "image_build_file=vars/my-custom-image.yml"
```

### 4. Commit and Push Your Changes

Add, commit, and push your changes to the master branch.

### 5. Trigger the OCI DevOps Pipeline

Any new push to the master branch automatically triggers the OCI DevOps pipeline to start building your custom image.

