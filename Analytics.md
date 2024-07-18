# Prerequisites
1. Cluster Setup: Ensure you have a Kubernetes cluster with three master nodes and one Cluster Control Manager (CCM) node.
2. Download Required Files: Download Avaya Analytics and Avaya Common Services files from the Avaya Product Licensing and Delivery System (PLDS). Since This ensures that all necessary services and dependencies are properly licensed.
3. Prepare Deployment Spreadsheet: Complete the Avaya_Oceana_Application_Deployment_<ReleaseNumber>.xlsm file with the necessary configuration details.

# Installation Steps
## 1. Pre-Installation
- Download Avaya `Common Services License`: Ensure the CCM has public internet access or prepare for offline deployment.
- Complete `Deployment Spreadsheet`: Fill out the macro-enabled deployment spreadsheet with the appropriate configuration values.

## 2. Setting Up Cluster Control Manager (CCM)
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

## 3. Installing Cylance
- Install Cylance on CCM:
  ```
  rpm -ivh CylancePROTECTOpenDriver-3.0.1005-2344.el8.x86_64.rpm
  rpm -ivh CylancePROTECTDriver-3.0.1005-2344.el8.x86_64.rpm
  rpm -ivh CylancePROTECT.el8.rpm
  ```
- Copy Cylance RPM to Nodes:
  ```
  scp cust@10.30.7.51:/home/cust/CylancePROTECTOpenDriver-3.0.1005-2344.el8.x86_64.rpm /var/vcap/data
  scp cust@10.30.7.51:/home/cust/CylancePROTECTDriver-3.0.1005-2344.el8.x86_64.rpm /var/vcap/data
  scp cust@10.30.7.51:/home/cust/CylancePROTECT.el8.rpm /var/vcap/data
  ```
- Install Cylance on Nodes:
  ```
  cd /var/vcap/data
  rpm -ivh CylancePROTECTOpenDriver-3.0.1005-2344.el8.x86_64.rpm
  rpm -ivh CylancePROTECTDriver-3.0.1005-2344.el8.x86_64.rpm
  rpm -ivh CylancePROTECT.el8.rpm
  ```
## 4. Deploy Avaya Analytics
- Verify Disk Space:
  ```
  df -h
  ```
- Extract Downloads on CCM:
  ```
  tar -zxvf downloads.tgz -C /var/avaya/artifactCache
  ```

- Load Solution Images and Charts:
  ```
  agn load -d /var/avaya/artifactCache/downloads -h <Cluster Control Manager FQDN>
  ```
- Start Cluster Deployment:
  ```
  kubectl apply -f avaya-analytics-deployment.yaml
  ```

## 5. Verifying the Deployment
- Check Pods Status:
  ```
  kubectl get pods --all-namespaces
  ```
- Check Services Status:
  ```
  kubectl get services --all-namespaces
  ```
## 6. Post-Installation
- Verify Cylance Protection:
  ```
  systemctl status cylancesvc
  ```
- Disable Cylance Before Upgrade:
  ```
  systemctl stop cylancesvc.service
  systemctl disable cylancesvc.service
  ```
- Re-enable Cylance After Upgrade:
  ```
  systemctl enable cylancesvc.service
  systemctl start cylancesvc.service
  ```

## Using Helm chart
- Download Analytics Package: Obtain the Avaya Analytics package from the Avaya PLDS (Product Licensing and Delivery System).
- Helm Chart Deployment: Use Helm, a Kubernetes package manager, to deploy Avaya Analytics.
  - Add Helm Repository:
    ```
    helm repo add avaya-repo <repository_url>
    ```
  - Update Helm Repositories:
    ```
    helm repo update
    ```
  - Install Analytics via Helm:
    ```
    helm install avaya-analytics avaya-repo/avaya-analytics
    ```
    
