# What's in an Analytic Story?

[Analytic Stories](https://github.com/splunk/security-content/blob/develop/docs/stories_categories.md) and their corresponding searches are composed of **.yml** files (manifests) and associated .conf files. The stories reside in [/stories](https://github.com/splunk/security-content/tree/develop/stories) and the searches live in [/detections](https://github.com/splunk/security-content/tree/develop/detections). 

#### Content Parts
* [stories/](https://github.com/splunk/security-content/tree/develop/stories/): All Analytic Stories 
* [detections/](https://github.com/splunk/security-content/tree/develop/detections/): Splunk Enterprise, Splunk UBA, and Splunk Phantom detections that power Analytic Stories
* [deployments/](https://github.com/splunk/security-content/tree/develop/deployments/): Deployment configurations for scheduling correlation searches in Enterprise Security
* [macros/](https://github.com/splunk/security-content/tree/develop/macros/): Macros that are used by the detections
* [lookups/](https://github.com/splunk/security-content/tree/develop/lookups/): Lookups that are used by the detections
* [playbooks/](https://github.com/splunk/security-content/tree/develop/playbooks/): Playbook configurations that are associated with analytic stories

#### Supporting Parts
* [CI/CD](https://github.com/splunk/security_content/tree/develop/.github/workflows): These git actions file help with validation, building, inspecting and testing of our repository

