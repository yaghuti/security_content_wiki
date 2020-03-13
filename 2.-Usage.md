
The Splunk Security Content Exchange can be used via:

#### [Splunk App](https://github.com/splunk/security-content/releases)
Grab the latest release of DA-ESS-ContentUpdate and install it on a Splunk Enterprise server (search head).

#### [API](https://docs.splunkresearch.com/?version=latest)
```
curl -s https://content.splunkresearch.com | jq
{
  "hello": "welcome to Splunks Research security content api",
  "available_endpoints": [
    "/stories",
    "/detections",
    "/investigations",
    "/baselines",
    "/responses",
    "/package"
  ]
}
```