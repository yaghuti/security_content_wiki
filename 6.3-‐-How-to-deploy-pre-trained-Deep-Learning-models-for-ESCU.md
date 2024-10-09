## Steps to deploy a pre-trained deep learning model for ESCU

NOTE: The following instructions are specifically for deploying pre-trained deep learning models using Splunk App for Data Science and Deep Learning (DSDL)5.0 in ESCU.  The deployed models are then used with "apply" command in ESCU detections.

The steps are outlined as follows -
1. Set up Splunk App for Data Science and Deep Learning (DSDL) 5.0
2. Download the model artifacts
3. Deploy model artifacts into Splunk app DSDL


### Set up Splunk App for Data Science and Deep Learning (DSDL)
1. Install the DSDL app (https://splunkbase.splunk.com/app/4607/) 5.0 on Splunk instance and follow the steps in the Overview > User Guide.
2. Additional information and FAQs are available here https://splunkbase.splunk.com/app/4607/#/details.
3. Ensure the containers use Golden Image CPU 5.0.0

### Download the model artifacts
1. Download the pre-trained model file .tar.gz from the link provided here.

| Detection        | Model |
| ----------- | ----------- |
| Detect DGA domains using pre-trained model in DSDL | [pre-trained model](https://seal.splunkresearch.com/pretrained_dga_model_dsdl.tar.gz) |
| Detect DNS Tunneling using TXT record responses | [pre-trained model](https://seal.splunkresearch.com/detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.tar.gz) |
| Detect suspicious process names | [pre-trained model](https://seal.splunkresearch.com/detect_suspicious_processnames_using_pretrained_model_in_dsdl.tar.gz) |
| Detect DNS Data Exfiltration | [pre-trained model](https://seal.splunkresearch.com/detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.tar.gz) |

2. Download notebook .ipynb for the detection

| Detection        | Notebook |
| ----------- | ----------- |
| Detect DGA domains using pre-trained model in DSDL | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/pretrained_dga_model_dsdl.ipynb) |
| Detect DNS Tunneling using TXT record responses | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.ipynb) |
| Detect suspicious process names | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_processnames_using_pretrained_model_in_dsdl.ipynb) |
| Detect DNS Data Exfiltration | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.ipynb) |


3. Download model configuration file .json for the detection 

| Detection        | .json file |
| ----------- | ----------- |
| Detect DGA domains using pre-trained model in DSDL | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/pretrained_dga_model_dsdl.json) |
| Detect DNS Tunneling using TXT record responses | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.json) |
| Detect suspicious process names | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_processnames_using_pretrained_model_in_dsdl.json) |
| Detect DNS Data Exfiltration | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.json) |

4. Follow the below steps only if you are deploying the model to use outside of ESCU. For ex: in a simple SPL search using |apply command. 
   Place these .mlmodel files into app context /lookup directory. For ex: etc/apps/mltk-container/lookups
  

| Detection        | .mlmodel file |
| ----------- | ----------- |
| Detect DGA domains using pre-trained model in DSDL | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_pretrained_dga_model_dsdl.mlmodel) |
| Detect DNS Tunneling using TXT record responses | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.mlmodel) |
| Detect suspicious process names | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_detect_suspicious_processnames_using_pretrained_model_in_dsdl.mlmodel) |
| Detect DNS Data Exfiltration | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.mlmodel) |

### Deploy the model artifacts

1. Login into the Splunk instance and launch the Splunk App for Data Science and Deep Learning (DSDL).
2. Select Containers from the drop-down menu and it should list all the containers.
3. Select Container Image as Golden image CPU 5.0.0 and Cluster target as per env setup and start the dev container.
4. Wait for the container to start up and urls to populate for the container.
5. Login into the Jupyter lab of dev container by clicking on the url, ex: http://{container_url}:port_num/lab? 
    * Use the password provided in the Overview > User Guide of DSDL app
6. The below steps are performed within the Jupyter Lab of the container.
    * Upload the pre-trained model .tar.gz file into app/model/data path using the upload option in the Jupyter notebook.
    * Open a terminal on Jupyterlab and execute the following commands

         ```
          tar -xf app/model/data/<pretrained_model_file_name>.tar.gz -C app/model/data/<pretrained_model_file_name>/
         ```			
      This will extract the artifact `<pretrained_model_file_name>.tar.gz` into `app/model/data/<pretrained_model_file_name>/`			
    * Upload <detection>.ipynb notebook into notebooks folder using the upload option in Jupyter lab.
    * Also, upload <detection>.json model configuration into notebooks/data folder.
    * Save the notebook using the save option in Jupyter notebook. 
 7. Start the container specific to the detection. 

