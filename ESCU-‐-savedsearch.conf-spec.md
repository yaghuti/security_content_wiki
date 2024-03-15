
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