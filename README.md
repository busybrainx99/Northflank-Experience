# Deploying a Full-Stack Application on Northflank  

This README documents my experience using Northflank for the first time to deploy a full-stack application. It provides a high-level overview of the platform's capabilities, my deployment process, and the results I achieved. This is not a step-by-step tutorial but rather a summary of key features and my impressions.
---

## Step 1: Sign Up and Explore the Overview Page  

I signed up and navigated to the overview page. The platform stood out with its intuitive onboarding process, comprehensive documentation, and in-app mini-guides. These guides, accessible via the question mark icon in key sections, make it easy for first-time users to get started.

![Overview_page_large](https://github.com/user-attachments/assets/db37943a-0f58-41b2-96cf-89adff6f4457)

---

## Step 2: Link GitHub  

To use my GitHub repository containing a full-stack project, I linked my GitHub account to Northflank. The platform supports GitHub, GitLab, and Bitbucket. After linking, I could see all repositories I had authorized for Northflank access.  

#### Git Provider Options ![dfa06a8d2299bd79371b7663c15a2661](https://github.com/user-attachments/assets/84bdee04-c50b-403f-ab19-47bb6da33dde) 

#### Linked GitHub Repositories ![Github_link_page_l](https://github.com/user-attachments/assets/53d09664-5069-4fa4-9a3a-c06fffbd4cb9)


---

## Step 3: Choose a Cluster  

Northflank provides the ability to connect your own cloud account, either by spinning up a new cluster or importing an existing one, directly from its UI. Alternatively, you can leverage Northflank's built-in Kubernetes clusters, which are available in multiple regions, including Europe, Asia, and America. These clusters are optimized for fast and seamless deployment without the need for additional configuration.  

![Choose_Cluster_Provider_Page_L](https://github.com/user-attachments/assets/ebecdf6d-acda-4fdd-90ea-23396ece6347)

---

## Step 4: Add a Domain  

To make the service accessible via a custom domain, I first added and verified my domain. Northflank simplifies this process by providing the DNS records you need to configure on your registrar. After verifying, I added a subdomain for my service and linked it to enable HTTPS encryption.  

#### Domain Verification ![Domain_Setup_L](https://github.com/user-attachments/assets/3623a96f-48d8-4ed7-9d4a-764cfc59d8c8)

#### Subdomain Setup ![subdomain_setup_L](https://github.com/user-attachments/assets/5ad31029-57d5-480a-8011-0daaf18e1259)

---

## Step 5: Create a Project  

I created a project using Northflank’s built-in Kubernetes cluster abstraction. The platform offers regional options across Europe, Asia, and America, which makes deployment geographically flexible.  

(https://github.com/user-attachments/assets/c9d31243-3ab4-4faf-811f-8b0eb5fc6396)

---

## Step 6: Add Services  

### 6a. Create a Service  
Northflank deploys applications as **services**. Each service runs your code, provides an endpoint, and determines whether it is publicly or internally accessible.  

![Create_Service_L](https://github.com/user-attachments/assets/55754183-0c9d-435e-91a2-e60dc72bafb3)




### 6b. Configure Environment Variables  
Environment variables can be linked to other services like the backend. I used the **Dockerfile** in my repository for deployment. Northflank automatically detected the Dockerfile and built the service seamlessly.  

![Frontend_Dockerfile_L](https://github.com/user-attachments/assets/6bc1fc33-5da4-4cc9-a150-24aa23482b8d)


> **Note:** After deploying your services, replace local endpoints in your environment variables with the Northflank-generated endpoints. For instance, update API calls from > `http://localhost:8000` to the service's public or internal URL provided by Northflank. This ensures proper connectivity between services.

> Additionally, when you set up a database and associate it with a secret group, you do not need to manually configure database endpoints in your backend service environment variables. Northflank automatically appends the database environment variables (e.g., `DB_NAME`, `DB_URL`, `DB_PASSWORD`) to your backend service, using the aliases you define in the secret group. This allows you to map the database's environment variable keys to match your application's expected variable names seamlessly.

### 6c. Networking Configuration  
Northflank automatically detected the required port, assigned an endpoint, and allowed me to configure the domain for public access.  

![Networking_Section_L](https://github.com/user-attachments/assets/82e0a5ea-7d94-4024-844c-b681cacbd44b)



### 6d. Resource Allocation  
I allocated resources such as CPU, memory, and instances for the service. The paid tier allows enabling auto-scaling to handle increased workloads.  

![Resource_Section_L](https://github.com/user-attachments/assets/4ae630e6-eecc-4af8-a9ab-325ee8d073b3)


### 6e. Health Checks  
Northflank supports liveness, readiness, and startup probes for health checks. These were easy to configure within the UI.  

![Health_Checks_L](https://github.com/user-attachments/assets/44536c8d-f143-433e-bae3-dbd6f6e7cfee)


### 6f. Deploy Multiple Services  
I deployed both the frontend and backend as separate services. The backend service was configured for internal access only, while the frontend was made publicly accessible. Environment variables were used to link the two.  

#### Frontend Service
![Screenshot 2024-12-13 at 13 50 23](https://github.com/user-attachments/assets/663bed23-3c38-4626-a705-718c67ef469b)

#### Backend Service 
![Screenshot 2024-12-13 at 13 50 00](https://github.com/user-attachments/assets/20803183-69ad-49ba-941c-1855f5aeaae3)


### 6g. Service Management  
The Northflank UI provides access to container logs, monitoring, and health checks without the need for CLI commands.  

#### Metrics
![98c3b1f97734ff4c87554d865edeffce](https://github.com/user-attachments/assets/7e09a95a-8724-4201-b2d3-c6c3f2627f6a)

#### Container Shell Access
![container_shell_L](https://github.com/user-attachments/assets/e95bbbf8-eca5-4546-ae5f-0b1acc49d695)

---

## Step 7: Add a Database Add-On  

### 7a. Setup PostgreSQL Database  
I added a PostgreSQL database for the backend service. Northflank auto-provisioned environment variables such as `DB_NAME`, `DB_PASSWORD`, and `DB_URL` for seamless integration.  
![DB_Setup_L](https://github.com/user-attachments/assets/9f2dd10e-274c-4626-a8e6-d3ff1ce94056)




### 7b. Use Secret Groups  
I created a **secret group** to centralize environment variables. Northflank allowed me to create aliases for these variables to match the keys in my backend service code.  

#### Secret Group Configuration ![Secret_group_L](https://github.com/user-attachments/assets/ccd2a412-9ba8-40a5-a78d-1b073269ea3f)

#### Alias Setup Within the Secret Group ![DB_Secret_Link_To_Secret_L](https://github.com/user-attachments/assets/8612fcd4-d3a2-4c10-8bd3-b58600895f59)

---

## Step 8: Final Setup Overview  

After completing the setup, my application, database, backend, and frontend were all running smoothly.  

![Project_Overview_L](https://github.com/user-attachments/assets/c5e0b78a-2cf4-4d87-ba63-ac24ee2b14d3)



---

## Step 9: Test the Application  

I accessed the frontend service through the linked domain. Despite a small typo in the domain name, the app served correctly.  

![Website_Overview_L](https://github.com/user-attachments/assets/6f35e41b-4f39-4c93-9bd0-5bda3d7be694)

---

## Step 10: GitHub Integration and Release Flow  

I made a correction in my GitHub repository and committed the changes. Northflank automatically detected the commit and triggered a new build using its **release flow** feature, updating the deployed service.  

#### GitHub Commit 
![Github_Commit_L](https://github.com/user-attachments/assets/702767b4-3d94-4ef2-a2b5-03407e851d2e)  


#### Release Flow 
![NorthFlank_Release_Flow_L](https://github.com/user-attachments/assets/ba47f8c9-5bab-4197-8f1f-55aab2692052) 


#### Updated App 
![Deployed_App_Correction_L](https://github.com/user-attachments/assets/603527fe-d8a2-488e-ae36-424dc69e327a) 

---

## Step 11: TLS Confirmation  

Finally, I verified that TLS encryption was enabled for the domain in the Northflank UI and confirmed its recognition in the browser.  

#### TLS in Northflank UI 
![Encryption_Proof_UI_L](https://github.com/user-attachments/assets/92e2966f-67d6-4011-b01c-6315f99ff5a0)

#### TLS in Browser  
![Encryption_Proof_Browser_L](https://github.com/user-attachments/assets/609de6ff-1dec-42cf-b9b2-d0604649f019)


---
