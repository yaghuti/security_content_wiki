
This page provides detailed documentation for the ESCU saved search configuration in `savedsearches.conf`.


#### Stanza Header
- **`[ESCU - <Detection Name> - Rule]`**
  - The name of the saved search. Detection Name has to be less than 

#### ESCU/Saved Search Settings
- **`action.escu = 0`**
  - Disables or enables a specific ESCU action. `0` indicates disabled.
- **`action.escu.enabled = 1/0`**
  - Specifically enables an ESCU action, `1` for enabled.
- **`description`**
  - Provides a brief description and deprecation notice for the saved search.
- **`disabled = true/false`**
  - Indicates that the search is currently disabled.

#### Mapping and Metadata
- **`action.escu.mappings = {"cis20": ["CIS 10"], "mitre_attack": ["T1110"], "nist": ["DE.AE"]}`**
  - Defines mapping to security frameworks like CIS20 and NIST.
- **`action.escu.data_models = ["Authentication"]`**
  - Specifies the data models required, if any.
- **`action.escu.analytic_story = ["Suspicious Okta Activity"]`**
  - Associates the search with broader analytic themes, such as "Suspicious Okta Activity".

#### Implementation and Usage
- **`action.escu.eli5 = <string> `**
  - Offers a simplified explanation of the search's purpose.
- **`action.escu.how_to_implement = <string> `**
  - Provides implementation guidance, including suggested data sources.
- **`action.escu.known_false_positives = <string> `**
  - Discusses potential false positives and general mitigation strategies.

#### Dates and Confidence
- **`action.escu.creation_date = <string>`**
  - The date when the search was created.
- **`action.escu.modification_date = <date>`**
  - The date when the search was last modified.
- **`action.escu.confidence = <string> `**
  - The confidence level in the search's effectiveness, marked as "low, medium,high".

#### Search Identification
- **`action.escu.full_search_name`**
  - The full name of the search within the ESCU framework.
- **`action.escu.search_type`**
  - Specifies the type of search, here a detection search.

#### Product and Technology
- **`action.escu.product = ["Splunk Enterprise", "Splunk Enterprise Security", "Splunk Cloud"]`**
  - Lists relevant Splunk products.
- **`action.escu.providing_technologies = ["Okta"]`**
  - Indicates the technologies providing data for the search.

#### Scheduling and Dispatch
- **`cron_schedule`**
  - Defines the cron schedule for the search.
- **`dispatch.earliest_time` and `dispatch.latest_time`**
  - Set the time range for the search.
- **`schedule_window = auto`**
  - Splunk manages scheduling windows automatically.

#### Alert and Visibility
- **`alert.digest_mode = 1`**
  - Enables alert digest mode.
- **`is_visible = false`**
  - Controls the visibility of the search in the UI.

#### Search Query
- **`search`**
  - Contains the actual Splunk search query.

### Additional Settings
- **`enableSched = 1`**
  - Enables scheduling for the search.
- **`relation`**
  - Sets the relational condition for alert triggering.
- **`quantity`**
  - Sets the threshold quantity for the relational condition.
- **`realtime_schedule = 0`**
  - Disables real-time scheduling.

#### Risk and Correlation (Enterprise Security related configs) 
- **`action.risk = 1`**
  - Enables risk-based actions based on the search results.
- **`cron_schedule`**
  - Defines the cron schedule for the search to run, set to at the top of every hour.
- **`dispatch.earliest_time` and `dispatch.latest_time`**
  - Configures the time range for the search, from 70 minutes to 10 minutes before the current time.
- **`action.correlationsearch.enabled = 1`**
  - Enables correlation search features, linking the search to broader analytical contexts.
- **`schedule_window = auto`**
  - Allows Splunk to automatically manage scheduling windows to optimize search performance.
- **`action.notable = 1`**
  - Indicates that events found by this search are to be marked as notable.
- **`alert.digest_mode = 1`**
  - Enables alert digest mode, aggregating alerts for efficiency.

#### Risk Settings
- **`action.risk.param._risk_message = <string>`**
  - Defines the message to be displayed for a risk event, incorporating dynamic variables for user and source IP address.
- **`action.risk.param._risk`**
  - Details the risk scoring parameters, including object fields for user and source IP, along with the risk score.
- **`action.risk.param._risk_score = 0`**
  - Sets the base risk score.
- **`action.risk.param.verbose = 0`**
  - Controls verbosity of the risk action’s output, with `0` disabling verbose output.

#### Correlation Search Settings
- **`action.correlationsearch.label = ESCU - <Detection Name> - Rule`**
  - Provides a descriptive label for the correlation search.
- **`action.correlationsearch.annotations = {"analytic_story": ["Suspicious Okta Activity"], "cis20": ["CIS 10"], "confidence": 60, "impact": 80, "mitre_attack": ["T1586", "T1586.003", "T1078", "T1078.004", "T1621"], "nist": ["DE.CM"]}`**
  - Adds metadata annotations to the correlation search, linking to analytic stories, compliance standards, and attack techniques.

#### Notable Event Configuration
- **`action.notable.param.nes_fields`**
  - Specifies the fields to be included in the notable event summary.
- **`action.notable.param.rule_description`**
  - Provides a detailed description of the analytic rationale behind the search, identifying failed MFA challenge attempts in Okta authentication processes.
- **`action.notable.param.rule_title`**
  - Sets a concise title for the notable event generated by this search.
- **`action.notable.param.security_domain`**
  - Associates the search with a security domain, in this case, identity security.
- **`action.notable.param.severity`**
  - Defines the severity of the notable event, marked as high.

## Sample Config Configuration:

```
[ESCU - Okta Authentication Failed During MFA Challenge - DM - Rule]
action.escu = 0
action.escu.enabled = 1
description = The following analytic identifies an authentication attempt event against an Okta tenant that fails during the Multi-Factor Authentication (MFA) challenge. This detection is written against the Authentication datamodel and we look for a specific failed events where the authentication signature is `user.authentication.auth_via_mfa`. This behavior may represent an adversary trying to authenticate with compromised credentials for an account that has multi-factor authentication enabled.
action.escu.mappings = {"cis20": ["CIS 10"], "mitre_attack": ["T1586", "T1586.003", "T1078", "T1078.004", "T1621"], "nist": ["DE.CM"]}
action.escu.data_models = ["Authentication"]
action.escu.eli5 = The following analytic identifies an authentication attempt event against an Okta tenant that fails during the Multi-Factor Authentication (MFA) challenge. This detection is written against the Authentication datamodel and we look for a specific failed events where the authentication signature is `user.authentication.auth_via_mfa`. This behavior may represent an adversary trying to authenticate with compromised credentials for an account that has multi-factor authentication enabled.
action.escu.how_to_implement = UPDATE_HOW_TO_IMPLEMENT
action.escu.known_false_positives = UPDATE_KNOWN_FALSE_POSITIVES
action.escu.creation_date = 2024-03-11
action.escu.modification_date = 2024-03-11
action.escu.confidence = high
action.escu.full_search_name = ESCU - Okta Authentication Failed During MFA Challenge - DM - Rule
action.escu.search_type = detection
action.escu.product = ["Splunk Enterprise", "Splunk Enterprise Security", "Splunk Cloud"]
action.escu.providing_technologies = ["Okta"]
action.escu.analytic_story = ["Suspicious Okta Activity"]
action.risk = 1
action.risk.param._risk_message = A user [$user$] has failed to authenticate via MFA from IP Address - [$src$]"
action.risk.param._risk = [{"risk_object_field": "user", "risk_object_type": "other", "risk_score": 48}, {"threat_object_field": "src", "threat_object_type": "ip_address"}]
action.risk.param._risk_score = 0
action.risk.param.verbose = 0
cron_schedule = 0 * * * *
dispatch.earliest_time = -70m@m
dispatch.latest_time = -10m@m
action.correlationsearch.enabled = 1
action.correlationsearch.label = ESCU - Okta Authentication Failed During MFA Challenge - DM - Rule
action.correlationsearch.annotations = {"analytic_story": ["Suspicious Okta Activity"], "cis20": ["CIS 10"], "confidence": 60, "impact": 80, "mitre_attack": ["T1586", "T1586.003", "T1078", "T1078.004", "T1621"], "nist": ["DE.CM"]}
schedule_window = auto
action.notable = 1
action.notable.param.nes_fields = user,dest
action.notable.param.rule_description = The following analytic identifies an authentication attempt event against an Okta tenant that fails during the Multi-Factor Authentication (MFA) challenge. This detection is written against the Authentication datamodel and we look for a specific failed events where the authentication signature is `user.authentication.auth_via_mfa`. This behavior may represent an adversary trying to authenticate with compromised credentials for an account that has multi-factor authentication enabled.
action.notable.param.rule_title = Okta Authentication Failed During MFA Challenge - DM
action.notable.param.security_domain = identity
action.notable.param.severity = high
alert.digest_mode = 1
disabled = true
enableSched = 1
allow_skew = 100%
counttype = number of events
relation = greater than
quantity = 0
realtime_schedule = 0
is_visible = false
search = | tstats `security_content_summariesonly` count min(_time) as firstTime max(_time) as lastTime  values(Authentication.app) as app values(Authentication.reason) as reason values(Authentication.signature) as signature  values(Authentication.method) as method  from datamodel=Authentication where  Authentication.signature=user.authentication.auth_via_mfa Authentication.action = failure by _time Authentication.src Authentication.user Authentication.dest Authentication.action | `drop_dm_object_name("Authentication")` | `security_content_ctime(firstTime)` | `security_content_ctime(lastTime)`| iplocation src | `okta_authentication_failed_during_mfa_challenge___dm_filter`
```