## Steps to deploy a pre-trained deep learning model for ESCU

NOTE: The following instructions are specifically for deploying pre-trained deep learning models using Splunk App for Data Science and Deep Learning (DSDL).  The deployed models are then used with "apply" command in ESCU detections.

The steps are outlined as follows -
1. Set up Splunk App for Data Science and Deep Learning (DSDL)
2. Download the model artifacts
3. Deploy model artifacts into Splunk app DSD

## Set up Splunk App for Data Science and Deep Learning (DSDL)
1. Install the DSDL app (https://splunkbase.splunk.com/app/4607/) on Splunk instance and follow the steps in the Overview > User Guide.
2. Additional information and FAQs are available here https://splunkbase.splunk.com/app/4607/#/details.

## Download the model artifacts
1. Open the S3 bucket and download model file .tar.gz of interest from https://splunk-seal.s3.us-west-2.amazonaws.com/pretrained_<detection_name>_model_dsdl.tar.gz
2. Download pretrained_<detection_name>_model_dsdl.ipynb from https://github.com/splunk/security_content/notebooks

## Deploy the model artifacts

1. Login into the Splunk instance and launch the DSDL app.
2. Select Containers from the drop-down menu and it should list all the containers.
3. Select Container Image as Golden image 3.9 and Cluster target as per env setup and start the pretrained_<detection_name>_model_dsdl container.
4. Wait for the container to start up and urls to populate for the container.
5. Login into the Jupyter lab of pretrained_<detection_name>_model_dsdl container by clicking on the url, ex: http://{container_url}:port_num/lab? 
    * Use the password provided in the Overview > User Guide of DSDL app
6. The below steps are performed within the Jupyter Lab of the container.
    * Upload the pretrained_<detection_name>_model_dsdl.tar.gz file into app/model/data path using the upload option in the Jupyter notebook.
    * Open a terminal on Jupyterlab and execute the following commands

    * Untar the artifact pretrained_<detection_name>_model_dsdl.tar.gz using the command below

         ```
          tar -xf app/model/data/pretrained_<detection_name>_model_dsdl.tar.gz -C app/model/data
         ```			
   					
    * Upload pretrained_<detection_name>_model_dsdl.ipynb notebook into notebooks folder using the upload option in Jupyter lab and save the notebook using the save option in Jupyter notebook. 
    
    * Upload dga_model_dltk.json into notebooks/data folder.
 7. The pre-trained model pretrained_<detection_name>_model_dsdl is now deployed within DSDL.
