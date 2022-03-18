# Writing MLTK Content

## Files needed in a PR
1. In detections/ [<detection_name>.yml](https://github.com/splunk/security_content/blob/bdb8f427f866bd2c8e55bc3721ca95cefd28dd4c/detections/endpoint/potentially_malicious_code_on_commandline.yml)
- The SPL in this file uses the MLTK apply command

2. In lookups/ directory 
- The actual model file ending with [<model_name>.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_unusual_commandline_detection.mlmodel)
- A [<lookup_name>.yml file](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_unusual_commandline_detection.yml) that references the model file: 

3. In tests/ [<test_name.test>.yml](https://github.com/splunk/security_content/blob/develop/tests/endpoint/potentially_malicious_code_on_commandline.test.yml) with a reference to a test data set in [attack_data](https://github.com/splunk/attack_data) repository"


**NOTE:** This is specifically for shipping pre trained models. The model files can be created either by leveraging [`fit`](https://docs.splunk.com/Documentation/MLApp/latest/User/Customsearchcommands) command in MLTK or by other custom ML tools
