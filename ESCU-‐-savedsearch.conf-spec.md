
This page provides detailed documentation for the ESCU saved search configuration in `savedsearches.conf`.


### Stanza Header
- **`[ESCU - <Detection Name> - Rule]`**
  - The name of the saved search. Detection Name has to be less than 

### General Settings
- **`action.escu = 0`**
  - Disables or enables a specific ESCU action. `0` indicates disabled.
- **`action.escu.enabled = 1/0`**
  - Specifically enables an ESCU action, `1` for enabled.
- **`description`**
  - Provides a brief description and deprecation notice for the saved search.
- **`disabled = true/false`**
  - Indicates that the search is currently disabled.

### Mapping and Metadata
- **`action.escu.mappings = {"cis20": ["CIS 10"], "mitre_attack": ["T1110"], "nist": ["DE.AE"]}`**
  - Defines mapping to security frameworks like CIS20 and NIST.
- **`action.escu.data_models = ["Authentication"]`**
  - Specifies the data models required, if any.
- **`action.escu.analytic_story = ["Suspicious Okta Activity"]`**
  - Associates the search with broader analytic themes, such as "Suspicious Okta Activity".

### Implementation and Usage
- **`action.escu.eli5 = <string> `**
  - Offers a simplified explanation of the search's purpose.
- **`action.escu.how_to_implement = <string> `**
  - Provides implementation guidance, including suggested data sources.
- **`action.escu.known_false_positives = <string> `**
  - Discusses potential false positives and general mitigation strategies.

### Dates and Confidence
- **`action.escu.creation_date = <string>`**
  - The date when the search was created.
- **`action.escu.modification_date = <date>`**
  - The date when the search was last modified.
- **`action.escu.confidence = <string> `**
  - The confidence level in the search's effectiveness, marked as "low, medium,high".

### Search Identification
- **`action.escu.full_search_name`**
  - The full name of the search within the ESCU framework.
- **`action.escu.search_type`**
  - Specifies the type of search, here a detection search.

### Product and Technology
- **`action.escu.product = ["Splunk Enterprise", "Splunk Enterprise Security", "Splunk Cloud"]`**
  - Lists relevant Splunk products.
- **`action.escu.providing_technologies = ["Okta"]`**
  - Indicates the technologies providing data for the search.

### Risk and Correlation (Enterprise Security related configs) 
- **`action.risk = 1`**
  - Enables risk-based actions.
- **`action.risk.param._risk_message = <string>`**
  - Placeholder for a risk message, marked as "tbd".
- **`action.correlationsearch.enabled = 1`**
  - Enables correlation search features.

### Scheduling and Dispatch
- **`cron_schedule`**
  - Defines the cron schedule for the search.
- **`dispatch.earliest_time` and `dispatch.latest_time`**
  - Set the time range for the search.
- **`schedule_window = auto`**
  - Splunk manages scheduling windows automatically.

### Alert and Visibility
- **`alert.digest_mode = 1`**
  - Enables alert digest mode.
- **`is_visible = false`**
  - Controls the visibility of the search in the UI.

### Search Query
- **`search`**
  - Contains the actual Splunk search query.

## Additional Settings
- **`enableSched = 1`**
  - Enables scheduling for the search.
- **`allow_skew = 100%`**
  - Allows a 100% time skew in scheduling.
- **`counttype`**
  - Determines how results are counted, by the number of events.
- **`relation`**
  - Sets the relational condition for alert triggering.
- **`quantity`**
  - Sets the threshold quantity for the relational condition.
- **`realtime_schedule = 0`**
  - Disables real-time scheduling.

For further information or inquiries, feel free to contact `research@splunk.com`.
