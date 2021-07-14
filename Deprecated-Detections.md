Detections under the [deprecated](https://github.com/splunk/security_content/tree/develop/detections/deprecated) folder are treated a bit differently, below is a list of things that happen to them specifically:
 
- `doc_gen.py` will not longer include deprecated detections on [Splunk Docs](https://docs.splunk.com/Documentation/ESSOC/3.23.0/detections/Detections). 
- The correlation search label is updated to `ESCU - Deprecated -<search_name> - Rule`
- The following note is added to the beginning of the description of the deprecated detection:

```
 WARNING, this detection has been marked deprecated by the Splunk Threat Research team, this means that it will no longer be maintained or supported. If you have any questions feel free to email us at: research@splunk.com.*
```