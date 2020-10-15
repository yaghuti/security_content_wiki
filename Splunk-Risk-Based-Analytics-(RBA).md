# What is RBA
A method of alerting on anomalous behavior using Splunk Enterprise Security Risk Based Alerting (RBA). 

# Using
In order to use RBA in any [detection](https://github.com/splunk/security-content/tree/develop/detections) under the Splunk Security repository you must:

1. Tag the detection with a `risk_object_type` 
2. Tag the detection with a `risk_object`
3. Tag the detection with a `risk_score`

See below for details and examples on how to use each of these tags. 

### Risk Object Tag

The field name to use as the risk_object in the display of the risk scores in Splunk Enterprise Security. The `risk_object` value is selected based of the associated Splunk search output in the detection and maps to the type of entity of interest in the search output.    

`risk_object`

* is **optional**
* type: `string`
* default `""`

### Risk Object Values

This parameters only restriction it must be a string.

### Risk Object Example 

```json
tags:
  analytics_story:
  - AWS Suspicious Provisioning Activities
  mitre_attack_id:
  - T1535
  cis20:
  - CIS 1
  nist:
  - ID.AM
  security_domain: endpoint
  asset_type: AWS Instance
  risk_score: 25
  risk_object_type: user
  risk_object: userName
```

### Risk Object Type Tag

This parameter describes the type of object defined in the `risk_object` tag. 

`risk_object_type`

* is **optional**
* type: `string`
* default `""`

### Risk Object Type Values
 
* Type `system` allows for asset correlation
* Type `user` allows for identity correlation
* Type `<string>` or any other string as input will suffice but may not 

Note again just as in the ``risk_object`` case the only restriction on the value input for this field is it must be a string. 

### Risk Object Type Example 

```
tags:
  analytics_story:
  - AWS Suspicious Provisioning Activities
  mitre_attack_id:
  - T1535
  cis20:
  - CIS 1
  nist:
  - ID.AM
  security_domain: endpoint
  asset_type: AWS Instance
  risk_score: 25
  risk_object_type: user
  risk_object: userName
```

### Risk Object Score Tag

The risk score corresponding to the entity described in `risk_object`.  The score will be applied to risk modifiers used in the upstream RBA analytics and UI dashboards of Splunk Security Products.

`risk_score`

* is **optional**
* type: `Integer`
* default `1`

### Risk Object Score Example 

```
tags:
  analytics_story:
  - AWS Suspicious Provisioning Activities
  mitre_attack_id:
  - T1535
  cis20:
  - CIS 1
  nist:
  - ID.AM
  security_domain: endpoint
  asset_type: AWS Instance
  risk_score: 25
  risk_object_type: user
  risk_object: userName
```