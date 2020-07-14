# What does generate.py do?
To build a package containing any customization done to content like changing how [often detection run](https://github.com/splunk/security-content/wiki/Customize-to-Your-Environment#customizing-scheduling-and-alert-actions-with-deployments), or what the right source type for [sysmon](https://github.com/splunk/security-content/wiki/Customize-to-Your-Environment#customizing-source-types-with-macros) in your environment use the generate.py script. The script [generate.py](https://github.com/splunk/security-content/blob/develop/bin/generate.py) generates all the dynamic components that make content in a Splunk app. These specifically include:

* [savedsearches.conf](https://github.com/splunk/security-content/blob/develop/package/default/savedsearches.conf) Where all detections, response searches, and baselines are written into with their deployment configuration set.
* [macros.conf](https://github.com/splunk/security-content/blob/develop/package/default/macros.conf) Where all macros are written into, also it generates empty macros for every detection with the suffix [_filter](https://github.com/splunk/security-content/blob/develop/package/default/macros.conf#L33) to allow easy filtering of false positives.  
* [analytics_stories.conf](https://github.com/splunk/security-content/blob/develop/package/default/analytic_stories.conf) This is a bit of an odd one since this file is only used by the [Splunk Enterprise Security](https://www.splunk.com/en_us/software/enterprise-security.html) and [ESCU App](https://splunkbase.splunk.com/app/3449/) today to list the Use Cases AKA Analytic Stories. If you have Splunk Enterprise Security it will include your own stories under the Use Case Library [feature](https://docs.splunk.com/Documentation/ES/6.2.0/Admin/Usecasecontentlibrary).
* All [lookups/](https://github.com/splunk/security-content/tree/develop/lookups) define in transform.conf or collections.conf with their respective csv.
* [Splunk Enterprise Security](https://www.splunk.com/en_us/software/enterprise-security.html) workflow actions via [es_investigations.conf](https://github.com/splunk/security-content/blob/develop/package/default/es_investigations.conf) and [workflow_actions.conf](https://github.com/splunk/security-content/blob/develop/package/default/workflow_actions.conf) files. 

To run generate.py you must have the requirements of the project installed as described in step 2 of the [Installation Guide](https://github.com/splunk/security-content/wiki/Installation-and-Usage#github-workflow), but in case you missed it here is the command:

`pip install virtualenv && virtualenv venv && source venv/bin/activate && pip install -r requirements.txt`

Then run generate to build a Splunk App in a folder.

`python bin/generate.py --path . --output package --verbose`

Just like before, the path above assumes that you are under the security-content repository. At the end of its execution, you should expect an output similar to:

```
69 stories have been successfully written to package/default/analytic_stories.conf
206 detections have been successfully written to package/default/savedsearches.conf
71 response tasks have been successfully written to package/default/savedsearches.conf
46 baselines have been successfully written to package/default/savedsearches.conf
65 macros have been successfully written to package/default/macros.conf
workbench panels were generated
security content generation completed..
```

Note that we use the folder [package/](https://github.com/splunk/security-content/tree/develop/package) to store the static pieces of our Splunk App like the different dashboards, views, and other components as well. From here you might consider modifying the different static pieces of the app like name, author, version under the [app.manifest](https://github.com/splunk/security-content/blob/develop/package/app.manifest#L7), and [default/app.conf](https://github.com/splunk/security-content/blob/develop/package/default/app.conf) files. Next, let's talk about packaging our application for deployment and committing any changes made to our fork of security-content. 