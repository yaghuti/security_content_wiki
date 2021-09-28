# What's in an Analytic Story?

[Analytic Stories](https://github.com/splunk/security-content/blob/develop/docs/stories_categories.md) and their corresponding searches are composed of **.yml** files (manifests) and associated .conf files. The stories reside in [/stories](https://github.com/splunk/security-content/tree/develop/stories) and the searches live in [/detections](https://github.com/splunk/security-content/tree/develop/detections). 

Manifests contain a number of mandatory and optional fields. You can see the full field list for each piece of content [here](https://github.com/splunk/security-content/tree/develop/docs#spec-documentation).


#### Content Parts
* [stories/](https://github.com/splunk/security-content/tree/develop/stories/): All Analytic Stories 
* [detections/](https://github.com/splunk/security-content/tree/develop/detections/): Splunk Enterprise, Splunk UBA, and Splunk Phantom detections that power Analytic Stories
* [deployments/](https://github.com/splunk/security-content/tree/develop/deployments/): Deployment configurations for scheduling correlation searches in Enterprise Security
* [macros/](https://github.com/splunk/security-content/tree/develop/macros/): Macros that are used by the detections
* [lookups/](https://github.com/splunk/security-content/tree/develop/lookups/): Lookups that are used by the detections
* [playbooks/](https://github.com/splunk/security-content/tree/develop/playbooks/): Playbook configurations that are associated with analytic stories

#### Supporting Parts
* [dist/](https://github.com/splunk/security-content/tree/develop/dist/): Splunk content app-source files, including lookups, binaries, and default config files
* [bin/](bin/): All binaries required to produce and test content

# Docs
* [docs/](https://github.com/splunk/security-content/tree/develop/docs/): Documentation for all spec files
* [spec/](https://github.com/splunk/security-content/tree/develop/spec/): All spec files that describe the security content