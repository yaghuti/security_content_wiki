# SSA Testing Detections

Hey did you know we strive to test all the detections we develop under the security-content reposity. By testing we mean we actively make sure that the following pieces are checked in CI:

1. [validates](https://github.com/splunk/security-content/wiki/Troubleshooting#our-automated-tests) all detection files 
2. downloads attack data used to test described under the provided [attack data](https://github.com/splunk/security-content/tree/develop/tests)
3. builds and activates dsp pipeline with the detections
4. sends attack data through the pipeline
5. evaluates results using pass condition in test files

## Manual Execution

As a developer you can run tests locally to find issues in your detections. Manual execution provides also debugging capabilities. `validate_ssa.py` can show you a sample of your input data, shows you the testing pipeline after the SSA macros are removed, and it can show you a sample of your pipeline's output.

Here there are the three steps that should follow.

*step 1*: Prepare your environment
```
virtualenv -p python3.7 venv && source venv/bin/activate
python -m pip install -r requirements.txt
```

*step 2*: Read the help
```
python bin/validate_ssa.py -h
```

```
usage: python bin/validate_ssa.py [-h] [--skip-errors] [--debug]
                       test_files [test_files ...]
```

*step 3*: Execute all, or some tests

```
python bin/validate_ssa.py --skip-errors --debug test_files tests/*/*.yml
```

_Note: Currently the testing can only run inside Splunk's network due to dependencies on Humvee._

## Automated
This runs as part of a [gitlabs CI pipeline](https://github.com/splunk/security-content/blob/add_gitlab_ci-all-tests/.gitlab-ci.yml) on PR via CI. 

```
variables:
  GIT_SUBMODULE_STRATEGY: recursive

stages:
  - ssa-validate

validate:
  stage: ssa-validate
  image: chrsep/alpine-java-python
  before_script:
    - pip3 install -r requirements.txt
  script:
    - python3 bin/validate.py -p . -v
    - python3 bin/validate_ssa.py --skip-error tests/*/*
```

## Logical Flow Diagram 🚰

![](https://github.com/splunk/security-content/raw/add_gitlab_ci-all-tests/docs/static/ssa-testing.png)

## Specifics

### Tests
Test files have 3 major components that allow us to consider a detection tested. These are:

* Detection name
* Pass Condition
* Attack Data file

You can see an example of one here:

```
name: More than usual number of LOLBAS applications in short time period - SSA Unit Test
detections:
  - name: More than usual number of LOLBAS applications in short time period
    file: endpoint/unusual_lolbas_in_short_period_of_time___ssa.yml
    pass_condition: '| stats count | where count > 0'
description: Test more than usual lolbas being executed in a short period of time
attack_data:
  - file_name: T1059.all.labeled.lolbas-test.json
    data: https://ssa-test-dataset.s3-us-west-2.amazonaws.com/T1059.all.labeled.lolbas-test.json
```

** Note we only support one attack data file per detection even though the field is an array.


### Detections
Detection files are described by the spec 3.0 schema. They contain a search (SPL2 if typed SSA or SPL if typed ESCU) and meta data around it in the form of tags. The major components are:

* search 
* type
* name

```
name: First time seen command line argument - SSA
id: fc0edc95-ff2b-48b0-9f6f-63da3789fd23
version: 1
date: '2020-6-25'
description: "This search looks for command-line arguments that use a `/c` parameter
  to execute a command that has not previously been seen. 
This is an implementation on SPL2 of the rule `First time seen command line argument` by @bpatel."
how_to_implement: "You must be populating the endpoint data model for SSA and specifically the process_name and the process fields"
author: Ignacio Bermudez Corrales, Splunk
type: SSA
search: '| from read_ssa_enriched_events()
| eval timestamp=parse_long(ucast(map_get(input_event, "_time"), "string", null))
| eval dest_user_id=ucast(map_get(input_event, "dest_user_id"), "string", null),
       dest_device_id=ucast(map_get(input_event, "dest_device_id"), "string", null),
       process_name=ucast(map_get(input_event, "process_name"), "string", null),
       cmd_line=lower(ucast(map_get(input_event, "process"), "string", null))
| where process_name="cmd.exe" AND
        match_regex(ucast(cmd_line, "string", ""), /.* \/[cC] .*/)=true
| first_time_event cache_partitions=5 input_columns="cmd_line"
| where first_time_cmd_line
| eval start_time = timestamp,
       end_time = timestamp,
       entities = mvappend(dest_device_id, dest_user_id),
       body = "TBD"
| into write_ssa_detected_events();'
eli5: "The subsearch returns all events where `cmd.exe` was used with a `/c` parameter
  in the command-line arguments to execute other commands/programs. It appends the
  historical data to those results in the lookup file. Next, it recalculates the `firstTime`
  and `lastTime` field for command-line execution and outputs this data to the lookup
  file to update the local cache. It returns only those events that have first been
  seen in the past one hour. This is combined with the main search to return the time,
  user, destination, process, parent process, and value of the command-line argument."
known_false_positives: "Legitimate programs can also use command-line arguments to
  execute. Please verify the command-line arguments to check what command/program
  is being executed. We recommend customizing the `first_time_seen_cmd_line_filter` macro to exclude legitimate parent_process_name"
tags:
  cis20:
    - CIS 3
    - CIS 8
  kill_chain_phases:
    - Command and Control
    - Actions on Objectives
  mitre_attack:
    - Execution
    - Scripting
    - Persistence
    - Command-Line Interface
  mitre_technique_id:
    - T1059
    - T1117
    - T1202
  nist:
    - PR.PT
    - DE.CM
    - PR.IP
  risk_severity: low
  security_domain: endpoint

```

### Usage

```
usage: validate_ssa.py [-h] [--skip-errors] [--debug]
                       test_files [test_files ...]

positional arguments:
  test_files     test files to be checked

optional arguments:
  -h, --help     show this help message and exit
  --skip-errors
  --debug
 ```

