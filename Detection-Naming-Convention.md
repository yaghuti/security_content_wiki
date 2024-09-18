# Summary 

Everytime a Splunk Security Content analytic is created it should follow the naming convention below. This convention provides us consistent naming as well as organization for our different security content components. 

# Format

`<platform>_<technique_name>_<short_description>`

### Where

* `<platform>` = Cloud, Endpoint, Network, Application, Splunk etc
* `<technique_name>` = Full name of the technique: OS Credential Dumping, Valid Accounts, Process Injection
* `<short_description>` = A short description of the detection, ideally 1 to 2 words. See `names should be`

# Example

[Executables Or Script Creation In Suspicious Path](https://research.splunk.com/endpoint/executables_or_script_creation_in_suspicious_path/)

# Names should be:
* Limited to 64 characters
* Avoid verbs/words like: abnormal, suspicious, malicious, and detect
