Each ESCU detection now comes with an attached risk_score value, this value is derived from a simple formula risk_score=(impact * confidence)/100. Where impact and confidence are 0-100 number values based on the detection author's judgment. Impact captures the effect the attack will have on an organization and confidence in how accurate the analytics is at catching said threat. This is also documented under each detections in [research.splunk.com](http://research.splunk.com/) see: https://research.splunk.com/endpoint/attempted_credential_dump_from_registry_via_reg_exe/#rba

Impact and Confidence today are somewhat arbitrary base on the detection author. The rule of thumb we use is:

### Scoring for impact and confidence 
* 0-25 **low** 
* 25-50 **medium** 
* 50-80 **high** 
* 80-100 **critical**

### Meaning of Impact values

* Low impact = has very little repercussions to the organization
* Medium impact = could cause damages to the organization
* High impact = requires investigation and analysis for the damages caused to the organization
* Extreme impact = likely requires forensic analysis and Incident Response team to be involved

### Meaning of Confidence values

* Low confidence = might have false positive not a very tight rule could contain other results aside from what is intended
* Medium confidence =  some false positives but unlikely
* High confidence = the rule will catch exactly what the author expect to find
* Extreme confidence = No way to get false positives, search is very tight to what exactly it looks for, no possibility for false positive

# Example Table
![image](https://user-images.githubusercontent.com/1476868/187281619-950d2f16-68d4-4488-9a8e-012af10f2d3d.png)
_credits to: Ryan Long_
 
### Using Observables and Roles to calculate risk score:
The risk and threat objects are created with the following logic:
- When the observable **`type`** is **`'User', 'Username', 'Email Address'`**:
    - The `risk_object_type` is `user` in savedsearches.conf
    - The `risk_object_field` is the `name` of the observable in the detection.yml

- When the observable **`type`** is **`'Device', 'Endpoint', 'Hostname', 'IP Address'`** :
    - The `risk_object_type` is `system` in savedsearches.conf
    - The `risk_object_field` is the `name` of the observable in the detection.yml

- When the observable **`type`** is **NOT** matching the above list:
    - The `risk_object_type` is `other` in savedsearches.conf
    - The `risk_object_field` is the `name` of the observable in the detection.yml

- When the observable **`role`** is  **`Attacker`**: 
    - The `threat_object_type` is the `type` of the observable in the detection.yml
    - The `threat_object_field` is the `name` of the observable in the detection.yml

### Observables :

- Role:(Choose one or many of the following)
```
`   "Other"
    "Unknown"
    "Actor:
    "Target"
    "Attacker
    "Victim"
    "Parent Process"
    "Child Process"
    "Known Bad"
    "Data Loss:
    "Observer"
```
- Types: (Choose one)
```
    "Unknown"
    "Hostname"
    "IP Address"
    "MAC Address"
    "User Name"
    "Email Address"
    "URL String"
    "File Name"
    "File Hash"
    "Process Name"
    "Ressource UID"
    "Endpoint"
    "User"
    "Email"
    "Uniform Resource Locator"
    "File"
    "Process"
    "Geo Location"
    "Container
    "Registry Key"
    "Registry Value"
    "Other"
```

