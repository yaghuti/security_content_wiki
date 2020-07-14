## Customizing source types with Macros 
When customizing security-content to fit your organization and Splunk deployment, one of the key things to change is what names your different source types have. A great example of this is sysmon data. If collected using the latest [Splunk Add-On for Microsoft Sysmon](https://splunkbase.splunk.com/app/1914/), it will automatically be source typed with:

 `sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational`

This might not be the exact source type used in your organization and Splunk deployment. However, to set your own, simply modify the file: [security-content/macros/sysmon.yml](https://github.com/splunk/security-content/blob/develop/macros/sysmon.yml) and change the value of [definition](https://github.com/splunk/security-content/blob/develop/macros/sysmon.yml#L1). Other, great examples are [okta](https://github.com/splunk/security-content/blob/develop/macros/okta.yml), [wmi](https://github.com/splunk/security-content/blob/develop/macros/wmi.yml), and [streams http](https://github.com/splunk/security-content/blob/develop/macros/stream_http.yml).

## Customizing scheduling and alert actions with deployments

To customize how often any detection run and what alert action they should automatically perform, a deployment configuration must be created. By default, security-content includes a deployment called [Enterprise Security deployment configuration](https://github.com/splunk/security-content/blob/develop/deployments/10_enterprise_security_deployment_configuration.yml) which schedules all Analytic Stories to run hourly and search in the last 60 minutes. The default deployment also has an alert action configure that creates a notable event in Enterprise Security if anything is detected. This can be easily customized by individual Analytic Story, and Detection by setting a matching tag value. For example: 

```
name: Schedule Credential Dumping Daily
id: bc91a8cd-35e7-4bb2-6140-e756cc46f214
date: '2020-04-27'
description: Schedule Credential Dumping Daily with Email notification to the SOC
author: Jose Hernandez
scheduling:
  cron_schedule: '0 0 * * *'
  earliest_time: -1d@d
  latest_time: -10m@m
  schedule_window: auto
alert_action:
  email:
    message: Splunk Alert $name$ triggered %fields%
    subject: Splunk Alert $name$
    to: soc@splunk.com
tags:
  analytics_story: Credential Dumping
```

Schedule the [Credential Dumping Story](https://github.com/splunk/security-content/blob/develop/stories/credential_dumping.yml) to be executed daily and send results via email to soc@splunk.com. The deployment and Analytic story are linked by the matching tag both share `analytics_story: Credential Dumping`. By default, we also ship deployments for short ([runs hourly, searches back 60 minutes](https://github.com/splunk/security-content/blob/develop/deployments/20_baseline_cache_hourly_updates.yml)) and long ([runs daily, searches back 90 days](https://github.com/splunk/security-content/blob/develop/deployments/30_long_running_baseline_searches.yml) running baseline searches as well. It is important to note that security-content uses the file name alphanumeric order to resolve any conflicts in tags. In other words, if you wanted the Credential Dumping deployment example from above to not overwrite the default configuration file `10_enterprise_security_deployment_configuration.yml`, you would name the deployment file `11_credential_dumping_hourly.yml` or similar. 

Follow [these]() steps to generate your own Splunk App with your changes!