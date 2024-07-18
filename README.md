# Avaya Analytics™ overview
- Avaya Analytics™ is a reporting platform that provides the ability to view and analyze Avaya 
Oceana® data through historical interaction dashboards.

## Avaya Analytics Topology
![k8s](https://github.com/ManuSureshh/Avaya-Analytics_K8s/assets/155379347/a17a3779-1cfd-47b0-ae10-5f473bfb3558)

## Prerequisites for Deploying Avaya Analytics on Kubernetes
**1. Client Computer Specifications:**
- Operating System: Windows 10 Professional or Enterprise (v1903 or later) or macOS Catalina (10.15.4 or later).
- Hardware Requirements:
  - 2 GB of available free memory.
  - 1 CPU with 2.0 GHz or higher.
  - 60 GB of disk space for regular offline installation or 100 GB for manual transfer.

**2. Administrative Privileges:**
- Local account administrator privileges on Windows or Mac.

**3. Software Requirements:**
- Docker Desktop: Install Docker Desktop for Windows or Mac (version 2.2.0.3 or later).
- Cluster Control Manager (CCM): Ensure that CCM has internet access for online deployment.

**4. Credentials:**
- Avaya SSO credentials for software deployment.

**5. Cluster Setup:**
Ensure you have a Kubernetes cluster with `three master nodes` and one `Cluster Control Manager` (CCM) node.

**6. Download Required Files:**
- Download Avaya Analytics and Avaya Common Services files from the Avaya Product Licensing and Delivery System (PLDS). 
- Since necessary `services and dependencies are properly licensed`, we need to download the License File from PLDS.
- Prepare `Deployment Spreadsheet`: Complete the Avaya_Oceana_Application_Deployment_<ReleaseNumber>.xlsm file with the necessary configuration details.


## 7. Setting Up Cluster Control Manager (CCM)
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
