
Splunk Security Content detections has a field called `type` these types will drive workflow in the future on the product, below are the current proposed types:

See https://car.mitre.org/Glossary for inspiration.

| Type        | Description | Example      |   
| ----------- | ----------- |--------------|
| TTP | A TTP analytic is designed to detect a certain adversary tactic, technique or procedure. | [Attempted Credential Dump From Registry via Reg exe](https://github.com/splunk/security_content/blob/develop/detections/endpoint/attempted_credential_dump_from_registry_via_reg_exe.yml) |
| Baseline | A posture analytic is designed to help in the maintenance of the analytic or create a baseline of data for detections to leverage. | [Baseline Of Cloud Infrastructure API Calls Per User](https://github.com/splunk/security_content/blob/develop/baselines/baseline_of_api_calls_per_user_arn.ymlhttps://github.com/splunk/security_content/blob/develop/detections/cloud/baseline_of_cloud_infrastructure_api_calls_per_user.yml) |
| Anomaly | An anomaly analytic triggers on behavior that is not normally observed. Anomalous may not be explicitly malicious but may be suspect. For example, detection of executables that have never been run before or a process using the network which does not normally use the network. Like Situational Awareness analytics, anomaly analytics don’t necessarily indicate an attack. | [Abnormally High Number Of Cloud Infrastructure API Calls](https://github.com/splunk/security_content/blob/develop/detections/cloud/abnormally_high_number_of_cloud_infrastructure_api_calls.yml) | 
| Hunting | A detection that increases the risk of an asset or entity, although tends to be too noisy to generate a notable event by itself. It leverages aggregated risk from various other detections to produce a notable. Also known as hunting queries.  | [Common Ransomware Extensions ](https://github.com/splunk/security_content/blob/develop/detections/endpoint/common_ransomware_extensions.yml) |
| Correlation | An analytic that correlates various detection results to correlate a high level threat and its primary purpose is to generate a notable. | Spreading Ransomware Infection | 
| Investigation | An analytic that is used to investigate an entity or asset, usually executed after another analytic type finds results as a next step in the workflow. They are executed after a result is found as  | [AWS Investigate User Activities By ARN](https://github.com/splunk/security_content/blob/develop/detections/cloud/aws_investigate_user_activities_by_arn.yml) |


