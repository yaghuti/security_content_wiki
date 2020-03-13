# What's in an Analytic Story?

[Analytic Stories](https://github.com/splunk/security-content/blob/develop/docs/stories_categories.md) and their corresponding searches are composed of **.yml** files (manifests) and associated .conf files. The stories reside in [/stories](https://github.com/splunk/security-content/tree/develop/stories) and the searches live in [/detections](https://github.com/splunk/security-content/tree/develop/detections). 

Manifests contain a number of mandatory and optional fields. You can see the full field list for each piece of content [here](https://github.com/splunk/security-content/tree/develop/docs#spec-documentation).

# Security Content Layout
![](docs/static/structure.png)

#### Content Parts
* [stories/](stories/): All Analytic Stories 
* [detections/](detections/): Splunk Enterprise, Splunk UBA, and Splunk Phantom detections that power Analytic Stories
* [investigations/](investigations/): Splunk Enterprise and Splunk Phantom investigative searches and playbooks employed by Analytic Stories
* [responses/](responses/): Automated Splunk Enterprise and Splunk Phantom responses triggered by Analytic Stories
* [baselines/](baselines/): Splunk Phantom and Splunk Enterprise baseline searches needed to support detection searches in Analytic Stories

#### Supporting Parts
* [package/](package/): Splunk content app-source files, including lookups, binaries, and default config files
* [bin/](bin/): All binaries required to produce and test content

# Docs
* [docs/](docs/): Documentation for all spec files
* [spec/](spec/): All spec files that describe the security content
