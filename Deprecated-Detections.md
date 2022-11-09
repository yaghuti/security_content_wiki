# What happens to objects under deprecated

Detections under the [deprecated](https://github.com/splunk/security_content/tree/develop/detections/deprecated) folder are treated a bit differently, below is a list of things that happen to them specifically:
 
- `doc_gen.py` will no longer include deprecated detections on [Splunk Docs](https://docs.splunk.com/Documentation/ESSOC/3.23.0/detections/Detections). 
- The correlation search label is updated to `ESCU - Deprecated -<search_name> - Rule`
- research.splunk.com shows a deprecation WARNING 
![image](https://user-images.githubusercontent.com/1476868/200946466-0155d9bc-224f-463c-938f-7fa68a402d48.png)

- The following note is added to the beginning of the description of the deprecated detection:

```
 WARNING, this detection has been marked deprecated by the Splunk Threat Research team, which means that it will no longer be maintained or supported. If you have any questions feel free to email us at: research@splunk.com.*
```

# When do we deprecate a detection

- The attack or analytic is no longer relevant
- STRT builds a better approach to the analytic
- Uses a data source or TA no longer supported
- If we get enough bug reports and it does not make sense to maintain or meet our bar for quality







