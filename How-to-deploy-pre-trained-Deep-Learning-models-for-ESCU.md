## Steps to deploy a pre-trained deep learning model for ESCU

NOTE: The following instructions are specifically for deploying pre-trained deep learning models using Splunk App for Data Science and Deep Learning (DSDL).  The deployed models are then used with "apply" command in ESCU detections.

The steps are outlined as follows -
1. Set up Splunk App for Data Science and Deep Learning (DSDL)
2. Download the model artifacts
3. Deploy model artifacts into Splunk app DSDL

### Set up Splunk App for Data Science and Deep Learning (DSDL)
1. Install the DSDL app (https://splunkbase.splunk.com/app/4607/) on Splunk instance and follow the steps in the Overview > User Guide.
2. Additional information and FAQs are available here https://splunkbase.splunk.com/app/4607/#/details.

### Download the model artifacts
1. Model artifacts follow the pattern pretrained_<model_name>_dsdl.tar.gz. Retrieve the model name as follows -
   * Look for _dsdl suffixed .yml files under the [lookups](https://github.com/splunk/security_content/tree/develop/lookups) directory. 
   * Verify the detection name in the description of the yml file.
   * Look for the token that matches pretrained_[a-zA-Z_]+_dsdl.yml i.e., _the substring between pretrained_ and __dsdl substrings._
   * This token is the model name.
    (The model name will be denoted as <model_name> in this document.)
2. Download the pre-trained model file .tar.gz from https://seal.splunkresearch.com/pretrained_<model_name>_dsdl.tar.gz

 Example: The pre-trained model file for [Detect DGA domains using pre-trained model in DSDL](https://github.com/splunk/security_content/blob/develop/detections/experimental/network/detect_dga_domains_using_pretrained_model_in_dsdl.yml) can be downloaded from this link : [https://seal.splunkresearch.com/pretrained_dga_model_dsdl.tar.gz](https://seal.splunkresearch.com/pretrained_dga_model_dsdl.tar.gz)

2. Download pretrained_<model_name>_dsdl.ipynb from [here](https://github.com/splunk/security_content/tree/develop/notebooks)

### Deploy the model artifacts

1. Login into the Splunk instance and launch the Splunk App for Data Science and Deep Learning (DSDL).
2. Select Containers from the drop-down menu and it should list all the containers.
3. Select Container Image as Golden image 3.9 and Cluster target as per env setup and start the pretrained_<model_name>_dsdl container.
4. Wait for the container to start up and urls to populate for the container.
5. Login into the Jupyter lab of pretrained_<model_name>_dsdl container by clicking on the url, ex: http://{container_url}:port_num/lab? 
    * Use the password provided in the Overview > User Guide of DSDL app
6. The below steps are performed within the Jupyter Lab of the container.
    * Upload the pretrained_<model_name>_dsdl.tar.gz file into app/model/data path using the upload option in the Jupyter notebook.
    * Open a terminal on Jupyterlab and execute the following commands

         ```
          tar -xf app/model/data/pretrained_<model_name>_dsdl.tar.gz -C app/model/data
         ```			
      This will extract the artifact pretrained_<model_name>_dsdl.tar.gz under app/model/data				
    * Upload pretrained_<model_name>_dsdl.ipynb notebook into notebooks folder using the upload option in Jupyter lab.
    * Save the notebook using the save option in Jupyter notebook. 
    * Upload pretrained_<model_name>_dsdl.json into notebooks/data folder.
 7. The pre-trained model pretrained_<model_name>_dsdl is now deployed within DSDL.



### Example: Steps for deploying model for: "Detect DGA domains using pre-trained model in DSDL"

1. Make sure the Splunk app for Data Science and Deep Learning (DSDL) is installed and set up.
2. To retrieve the model name, open [lookups](https://github.com/splunk/security_content/tree/develop/lookups) directory and look for the _dsdl.yml file. (Currently, this the only [pretrained dsdl model file](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_pretrained_dga_model_dsdl.yml) available in the lookups directory)
3. The model name is dga_model which matches the pattern pretrained_[a-zA-Z_]+__dsdl.yml. Notice that <model_name> in pretrained_<model_name>_dsdl.tar.gz is replaced with dga_model.
4. Model artifacts can be downloaded from this link : [https://seal.splunkresearch.com/pretrained_dga_model_dsdl.tar.gz](https://seal.splunkresearch.com/pretrained_dga_model_dsdl.tar.gz).
5. Download the pretrained_dga_model_dsdl.ipynb Jupyter notebook from https://github.com/splunk/security_content/notebooks
Download the artifacts .tar.gz file from the link - https://seal.splunkresearch.com/pretrained_dga_model_dsdl.tar.gz
* Download the pretrained_dga_model_dsdl.ipynb Jupyter notebook from here: [notebooks](https://github.com/splunk/security_content/notebooks)
* Launch the Splunk app DSDL and click the Containers. This pretrained_dga_model_dsdl container should be listed.
* Select Container Image as Golden image 3.9 and Cluster target as per env setup and start the pretrained_dga_model_dsdl container.
* The urls populate when the container is launched. Login to the Jupyter lab url. 
* Below steps need to be followed inside Jupyter lab 
  * Upload the pretrained_dga_model_dsdl.tar.gz file into app/model/data path using the upload option in the jupyter notebook.
  * Extract the artifact pretrained_dga_model_dsdl.tar.gz using tar -xf app/model/data/pretrained_dga_model_dsdl.tar.gz -C app/model/data
  * Upload pretrained_dga_model_dsdl.pynb into Jupyter lab notebooks folder using the upload option in Jupyter lab
  * Save the notebook using the save option in jupyter notebook.
  * Upload pretrained_dga_model_dsdl.json into notebooks/data folder.

