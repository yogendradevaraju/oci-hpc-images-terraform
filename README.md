# oci-hpc-images-ansible
_Custom OCI HPC image customization using Ansible playbooks_

## Custom Image Build Workflow (Pure Ansible)

### 1. Configure Default Variables

Using ansible/vars/defaults.yml.example create a new version of the file: `defaults.yml` and fill in the variables.
```
cp ansible/vars/defaults.yml.example ansible/vars/defaults.yml
```

### 2. Create a Custom Image Vars File

Create a new YAML file that defines the configuration for the custom image you want to build. Use the provided example file (ansible/vars/example-image.yml) as a template. Name your new file according to the image you're customizing (for example, Canonical_Ubuntu_22.04.yml).
```
cp ansible/vars/example-image.yml ansible/vars/Canonical_Ubuntu_22.04.yml
```
Edit your new file (ansible/vars/Canonical_Ubuntu_22.04.yml) and update the variables as needed, also you will need to edit the image_id for your region. OCIDs can be found here, pick the image OCID based on your region: https://docs.oracle.com/en-us/iaas/images/

### 3. Update builder_spec.yaml to Use the Custom Vars File
Next, you need to tell the build pipeline to use your new custom image vars file:
  - Open the builder_spec.yaml file located in the root directory of your repository.
  - Locate the following lines that invoke the Ansible playbook (typically lines 71 and 72, under the “run image build playbook” section):
```
ansible-playbook ./ansible/builder.yml -e "image_build_file=vars/my-custom-image.yml"
```
  - Change the file name in the command to match your new custom image vars file:
  For example, if you created Canonical_Ubuntu_22.04.yml, update the command in the build_spec file as follows:
```
ansible-playbook ./ansible/builder.yml -e "image_build_file=vars/Canonical_Ubuntu_22.04.yml"
```

### 4. Commit and Push Your Changes

- Add, commit, and push your changes to the `master` branch of your repository.
***Note:*** Ensure you are pushing to the correct remote repository, i.e., your personal/private GitHub repository.

### 5. Trigger the OCI DevOps Pipeline

Any new push to the master branch automatically triggers the OCI DevOps pipeline to start building your custom image.

### 6. Follow the progress in OCI Console
  - Go to:
  ```
  OCI Console → Developer Services → DevOps → Projects → oci-custom-image-pipeline-terraform → Latest build history
  ```
  - Here you can follow the status, logs, and results of your triggered pipeline builds.

