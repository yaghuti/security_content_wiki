
 #### Content Parts
* [stories/](https://github.com/splunk/security-content/tree/develop/stories/): All Analytic Stories shipped in ESCU have their 
* [detections/](https://github.com/splunk/security-content/tree/develop/detections/): Splunk Enterprise, Splunk UBA, and Splunk Phantom detections that power Analytic Stories
* [deployments/](https://github.com/splunk/security-content/tree/develop/deployments/): Deployment configurations for scheduling correlation searches in Enterprise Security
* [macros/](https://github.com/splunk/security-content/tree/develop/macros/): Macros that are used by the detections
* [lookups/](https://github.com/splunk/security-content/tree/develop/lookups/): Lookups that are used by the detections
* [playbooks/](https://github.com/splunk/security-content/tree/develop/playbooks/): Playbook configurations that are associated with analytic stories
* [dashboards/](https://github.com/splunk/security-content/tree/develop/dashboards/): Contains xml configuration for the dashboards shipped from this 

 #### Supporting Parts
* [CI/CD](https://github.com/splunk/security_content/tree/develop/.github/workflows): These git actions file help with validation, building, inspecting and testing of our repository

 #### Release Versioning

Each Splunk Security Content [release](https://github.com/splunk/security-content/releases) follows a 3 number structure: **\<major\>.\<minor\>.\<patch\>** for example `3.9.1`. The following is an explanation of what each number signifies and when the numbers change.

* **\<major\>** - This number pertains to the specification/schema version our content is adhering to. Today we are in spec 3.0. This number only changes when we make a schema change or update. 
* **\<minor\>** - This number pertains to the update we are on. This number increases every time we introduce a new piece of content. Examples of content include, but are not limited to, the following: detections, stories, responses, and so on.  
* **\<patch\>** - This number pertains to fixes for content. This number increases every time we resolve a bug with a current piece of content but do not introduce any new functionality.

We did not come up with this concept and are just implementing semantic versioning per https://semver.org/. Note that release announcements are only sent out for major and minor changes, but not usually for patches unless they contain critical issues that require communication.

#### Content Versioning of yaml files
 * **version** - This number is an integer and is bumped every time a yaml file is changed
* **date** - This date string is updated every time the content is modified
