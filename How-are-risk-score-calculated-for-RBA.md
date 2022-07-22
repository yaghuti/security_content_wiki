Each ESCU detection now comes with an attached risk_score value, this value is derive from a simple formula risk_score=(impact * confidence)/100. Where impact and confidence are 0-100 number values based on the detection authors judgement. Impact captures the effect the attack will have in an organization and confidence in how accurate the analytic is at catching said threat. This is also documented under each detections in [research.splunk.com](http://research.splunk.com/) see: https://research.splunk.com/endpoint/attempted_credential_dump_from_registry_via_reg_exe/#rba

Impact and Confidence today are somewhat arbitrary base on the detection author. The rule of thumb we use is:

### Scoring 
* 0-25 **low** impact/confidence
* 25-50 **medium** impact/confidence
* 50-80 **high** impact/confidence 
* 80-100 **critical** impacting or certain

### Meaning of Impact values

* Low impact = has very little repercussions to the organization
* Medium impact = could cause damages to the org
* High impact = requires investigation and analysis for the damages caused to the org
* Extreme impact = likely requires forensic analysis and in I sent response to be involved

### Meaning of Confidence values

* Low confidence = might have false positive not a very tight rule could contain other results aside from what is intended
* Medium confidence =  some false positives but unlikely
* High confidence = the rule will catch exactly what the author expect to find
* Extreme confidence = No way to get false positives, search is very tight to what exactly it looks for, no possibility for false positive 