IMPORTANT ❗ ❗ ❗ Please remember to destroy all the resources after each work session. You can recreate infrastructure by creating new PR and merging it to master.
  
![img.png](doc/figures/destroy.png)


1. Authors:

   ***group nr 3***

   ***link to forked repo***
   
2. Fork https://github.com/bdg-tbd/tbd-2023z-phase1 and follow all steps in README.md.

3. Select your project and set budget alerts on 5%, 25%, 50%, 80% of 50$ (in cloud console -> billing -> budget & alerts -> create buget; unclick discounts and promotions&others while creating budget).

  ![img.png](doc/figures/discounts.png)

4. From avaialble Github Actions select and run destroy on main branch.

5. Create new git branch and add two resources in ```/modules/data-pipeline/main.tf```:
    1. resource "google_storage_bucket" "tbd-data-bucket" -> the bucket to store data. Set the following properties:
        * project  // look for variable in variables.tf
        * name  // look for variable in variables.tf
        * location // look for variable in variables.tf
        * uniform_bucket_level_access = false #tfsec:ignore:google-storage-enable-ubla
        * force_destroy               = true
        * public_access_prevention    = "enforced"
        * if checkcov returns error, add other properties if needed
       
    2. resource "google_storage_bucket_iam_member" "tbd-data-bucket-iam-editor" -> assign role storage.objectUser to data service account. Set the following properties:
        * bucket // refere to bucket name from tbd-data-bucket
        * role   // follow the instruction above
        * member = "serviceAccount:${var.data_service_account}"

    ***insert the link to the modified file and terraform snippet here***

    Create PR from this branch to **YOUR** master and merge it to make new release. 
    
    ***place the screenshot from GA after succesfull application of release with this changes***

    

6. Analyze terraform code. Play with terraform plan, terraform graph to investigate different modules.
    ![image](https://github.com/karpinski-j/tbd-2023z-phase1/assets/83401763/07b87418-3bb3-4f4f-9c10-c9d16e0b0648)

    Dataproc is a fully managed Hadoop and Spark service that simplifies data processing. Automation allows for the quick creation and management of clusters, enabling you to focus on your data and applications, and save money through the option to turn clusters on and off.     
    Additionally, Dataproc makes it easier to use Hadoop and Spark, reduces costs, is scalable, reduces complexity, and enhances performance.
   
7. Reach YARN UI
   
   ***place the port and the screenshot of YARN UI here***
   
8. Draw an architecture diagram (e.g. in draw.io) that includes:
    1. VPC topology with service assignment to subnets
    2. Description of the components of service accounts
    3. List of buckets for disposal
    4. Description of network communication (ports, why it is necessary to specify the host for the driver) of Apache Spark running from Vertex AI Workbech
  
    ***place your diagram here***

9. Add costs by entering the expected consumption into Infracost

   ***place the expected consumption you entered here***

   ***place the screenshot from infracost output here***

10. Some resources are not supported by infracost yet. Estimate manually total costs of infrastructure based on pricing costs for region used in the project. Include costs of cloud composer, dataproc and AI vertex workbanch and them to infracost estimation.

    ***place your estimation and references here***

    ***what are the options for cost optimization?***
    
12. Create a BigQuery dataset and an external table
    Code: 
    ```
    bq mk my_dataset
    bq mk --table --external_table_definition=@ORC=gs://bucket_orc/orc-file-11-format.orc dataset.table_1
    bq show dataset.table_1 
    ```
    ![image](https://github.com/karpinski-j/tbd-2023z-phase1/assets/83401763/8a032fe7-6d27-4292-8efb-34484955641a)
    ![image](https://github.com/karpinski-j/tbd-2023z-phase1/assets/83401763/5a8583f2-46bd-47b8-b7e1-8a5c1c5b7af3)

    Table scheme is already provided in orc file.
    ORC also stores metadata about the file, such as the schema, at the end of the file.
  
13. Start an interactive session from Vertex AI workbench (steps 7-9 in README):

    ***place the screenshot of notebook here***
   
14. Find and correct the error in spark-job.py

    ***describe the cause and how to find the error***

15. Additional tasks using Terraform:

    1. Add support for arbitrary machine types and worker nodes for a Dataproc cluster and JupyterLab instance

    ***place the link to the modified file and inserted terraform code***
    
    3. Add support for preemptible/spot instances in a Dataproc cluster

    ***place the link to the modified file and inserted terraform code***
    
    3. Perform additional hardening of Jupyterlab environment, i.e. disable sudo access and enable secure boot
    
    ***place the link to the modified file and inserted terraform code***

    4. (Optional) Get access to Apache Spark WebUI

    ***place the link to the modified file and inserted terraform code***
