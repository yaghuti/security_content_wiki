# What's in an Analytic Story?

[Analytic Stories](https://github.com/splunk/security-content/blob/develop/docs/stories_categories.md) and their corresponding searches are composed of **.yml** files (manifests) and associated .conf files. The stories reside in [/stories](https://github.com/splunk/security-content/tree/develop/stories) and the searches live in [/detections](https://github.com/splunk/security-content/tree/develop/detections). 

Manifests contain a number of mandatory and optional fields. You can see the full field list for each piece of content [here](https://github.com/splunk/security-content/tree/develop/docs#spec-documentation).


#### Content Parts
* [stories/](https://github.com/splunk/security-content/tree/develop/stories/): All Analytic Stories 
* [detections/](https://github.com/splunk/security-content/tree/develop/detections/): Splunk Enterprise, Splunk UBA, and Splunk Phantom detections that power Analytic Stories
* [deployments/](https://github.com/splunk/security-content/tree/develop/deployments/): Deployment configurations for scheduling correlation searches in Enterprise Security
* [responses/](https://github.com/splunk/security-content/tree/develop/responses/): Automated Splunk Enterprise and Splunk Phantom responses triggered by Analytic Stories
* [response_tasks/](https://github.com/splunk/security-content/tree/develop/response_tasks/): These contains various types of tasks that an analyst would do after a detection has triggered. 
* [baselines/](https://github.com/splunk/security-content/tree/develop/baselines/): Splunk Phantom and Splunk Enterprise baseline searches needed to support detection searches in Analytic Stories

#### Supporting Parts
* [package/](https://github.com/splunk/security-content/tree/develop/package/): Splunk content app-source files, including lookups, binaries, and default config files
* [bin/](bin/): All binaries required to produce and test content

# Docs
* [docs/](https://github.com/splunk/security-content/tree/develop/docs/): Documentation for all spec files
* [spec/](https://github.com/splunk/security-content/tree/develop/spec/): All spec files that describe the security content