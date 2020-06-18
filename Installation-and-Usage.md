The Splunk Security Content can be used via:

#### [Splunk App](https://github.com/splunk/security-content/releases)
Grab the latest release of DA-ESS-ContentUpdate and install it on a Splunk Enterprise instance.

#### [API](https://docs.splunkresearch.com/?version=latest)
```
curl -s https://content.splunkresearch.com | jq
{
  "hello": "welcome to Splunks Research security content api"
}
```

#### GitHub Workflow
Splunk Security Content can be used from GitHub by executing the following steps:
- Clone the Security Content GitHub project.
`````
git clone git@github.com:splunk/security-content.git
`````
- Change the deployment configuration under deployments/ to fit to your Splunk environment.
- Create virtualenv and install requirements.
`````
pip install virtualenv && virtualenv venv && source venv/bin/activate && pip install -r requirements.txt
`````
- Run bin/generate.py with the following command.
`````
python bin/generate.py --path . --output package -v
`````
- Copy the package folder to your Splunk instance.