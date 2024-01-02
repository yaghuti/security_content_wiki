Each ESCU detection now comes with an attached risk_score value, this value is derived from a simple formula risk_score=(impact * confidence)/100. Where impact and confidence are 0-100 number values based on the detection author's judgment. Impact captures the effect the attack will have on an organization and confidence in how accurate the analytics is at catching said threat. This is also documented under each detections in [research.splunk.com](http://research.splunk.com/) see: https://research.splunk.com/endpoint/attempted_credential_dump_from_registry_via_reg_exe/#rba

### Definition:
**Impact**: This refers to the potential severity or consequences of the threat or activity that the detection rule is designed to identify. Impact measures how significant a threat is in terms of potential damage or risk to the organization's assets, data, or operations. For example, a rule detecting a potential data breach would have a high impact due to the severe consequences of data loss or exposure.

**Confidence**: This is a measure of how accurately a detection rule identifies genuine threats without generating false positives. A rule with high confidence can reliably distinguish between actual malicious activity and benign actions that might appear suspicious. Confidence is crucial for ensuring that security teams are alerted to real threats without being overwhelmed by false alarms.
Impact and Confidence today are somewhat arbitrary base on the detection author. 

The rule of thumb we use is:

### Scoring for impact and confidence 
* 0-25 **low** 
* 25-50 **medium** 
* 50-80 **high** 
* 80-100 **critical**

### Meaning of Impact values

* Low Impact: These are threats that have minimal consequences for the organization. They might represent minor policy violations or low-level security risks that don't significantly compromise data, systems, or operations. Actions triggered by low impact rules often don't require immediate response but may need monitoring or periodic review.

* Medium Impact: This category includes threats that could potentially cause moderate harm to the organization's assets, reputation, or operations. They might not be immediately critical but could escalate if not addressed. Medium impact rules typically warrant timely investigation to assess and mitigate potential risks.

* High Impact: High impact threats pose a significant risk to the organization. They could lead to substantial data loss, service disruption, financial loss, or other serious consequences. Detection rules with high impact require immediate attention and action to prevent or limit damage.

* Extremely High Impact: This highest level is reserved for the most severe threats, which pose an imminent and critical risk to the organization’s core operations, data security, or existence. These might include advanced persistent threats (APTs), major data breaches, or other catastrophic events. Extreme impact rules necessitate an urgent and comprehensive response, often involving multiple layers of the organization, including top management.

### Meaning of Confidence values

* Low confidence = might have false positive not a very tight rule could contain other results aside from what is intended
* Medium confidence =  some false positives but unlikely
* High confidence = the rule will catch exactly what the author expect to find
* Extreme High confidence = No way to get false positives, search is very tight to what exactly it looks for, no possibility for false positive

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

