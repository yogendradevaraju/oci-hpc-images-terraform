# oci-hpc-images-ansible
_Custom OCI HPC image customization using Ansible playbooks_

## Custom Image Build Workflow (Pure Ansible)

### 1. Configure Default Variables

Using ansible/vars/defaults.yml.example create a new version of the file: `defaults.yml` and fill in the variables.
```
cp ansible/vars/defaults.yml.example ansible/vars/defaults.yml
```

### 2. Create a Custom Image Vars File

Create a new YAML file for your custom image in the ansible/vars/ directory using one of the example files as a template and fill in the variables based on your requirement:
```
cp ansible/vars/example-image.yml ansible/vars/my-custom-image.yml
```
In the my-custom-image.yml file, you will need to edit the image_id for your region. OCIDs can be found here, pick the image OCID based on your region: https://docs.oracle.com/en-us/iaas/images/

### 3. Update builder_spec.yaml to Use the Custom Vars File

Edit the builder_spec.yaml file by updating the Ansible playbook command under the “run image build playbook” section with the path to your new custom-image vars file created in step 2.
```
ansible-playbook ./ansible/builder.yml -e "image_build_file=vars/my-custom-image.yml"
```

### 4. Commit and Push Your Changes

- Add, commit, and push your changes to the `master` branch of your repository.
**Note:** Ensure you are pushing to the correct remote repository, i.e., your personal/private GitHub repository.

### 5. Trigger the OCI DevOps Pipeline

Any new push to the master branch automatically triggers the OCI DevOps pipeline to start building your custom image.

### 6. Follow the progress in OCI Console
  – Go to:
  ```
  OCI Console → Developer Services → DevOps → Projects → oci-custom-image-pipeline-terraform → Latest build history
  ```
  – Here you can follow the status, logs, and results of your triggered pipeline builds.

