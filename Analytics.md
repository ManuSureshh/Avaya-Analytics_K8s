# Prerequisites
1. Cluster Setup: Ensure you have a Kubernetes cluster with three master nodes and one Cluster Control Manager (CCM) node.
2. Download Required Files: Download Avaya Analytics and Avaya Common Services files from the Avaya Product Licensing and Delivery System (PLDS).
3. Prepare Deployment Spreadsheet: Complete the Avaya_Oceana_Application_Deployment_<ReleaseNumber>.xlsm file with the necessary configuration details.

# Installation Steps
1. Pre-Installation
- Download Avaya `Common Services License`: Ensure the CCM has public internet access or prepare for offline deployment.
- Complete `Deployment Spreadsheet`: Fill out the macro-enabled deployment spreadsheet with the appropriate configuration values.

2. Setting Up Cluster Control Manager (CCM)
- Extract Downloads:
  ```
  tar -zxvf downloads.tgz -C /var/avaya/artifactCache
  ```

  ```
  df -h  # Check disk space
  ```

- Upload Solution Images:
  ```
  agn load -d /var/avaya/artifactCache/downloads -h <Cluster Control Manager FQDN>
  ```

- Enter the username and password when prompted.

3. Installing Cylance

