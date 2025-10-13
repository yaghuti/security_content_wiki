# splunk/security_content/wiki

## 1.-Home

### Welcome to the Splunk Security Research Team's Security Content Project

This project gives you access to our repository of Analytic Stories that are security guides which provide background on TTPs, mapped to the MITRE framework, the Lockheed Martin Kill Chain, and CIS controls. They include Splunk searches, machine-learning algorithms, and Splunk Phantom playbooks (where available)—all designed to work together to detect, investigate, and respond to threats.
This content is available via Splunk Enterprise Security, [Splunkbase](https://splunkbase.splunk.com/app/3449/) and can be explored on [research.splunk.com](research.splunk.com)  The security-content project was designed to bring all Splunk detection, and the community together to improve our collective defenses.

## 2.-Installation-and-Usage

### Splunk ES Content Update Application

* [Github](https://github.com/splunk/security_content/releases) - Grab the latest release of DA-ESS-ContentUpdate and install it on a Splunk Enterprise instance.

* [Splunkbase](https://classic.splunkbase.splunk.com/app/3449/) - Grab the latest release of DA-ESS-ContentUpdate from Splunkbase and install it on a Splunk Enterprise instance.

* [Enterprise Security](https://www.splunk.com/en_us/products/enterprise-security.html)- These detections are already available in Splunk Enterprise Security via an automatic [application update process](https://docs.splunk.com/Documentation/ES/latest/Admin/Usecasecontentlibrary#Update_the_Analytic_Stories) built into the product

* [Website](http://research.splunk.com/) - You can also access this content on [https://www.research.splunk.com](http://research.splunk.com/) which is updated daily with the latest content that is available in the ESCU application.

### Getting Started

Follow these steps to get started with Splunk Security Content.

1. Clone this repository using `git clone https://github.com/splunk/security_content.git`
2. Navigate to the repository directory using `cd security_content`
3. Install contentctl using `pip install contentctl` to install the latest version of contentctl, this is a pre-requisite to validate, build and test the content like the Splunk Threat Research team

**Note:** We have sister projects that enable us to build the industry's best security content. These projects are the Splunk Attack Range, an attack simulation lab built around Splunk, and Contentctl, the tool that enables us to build, test, and package our content for distribution.

* [Splunk Attack Range](https://github.com/splunk/attack_range): An attack simulation lab built around Splunk.
* [Contentctl](https://github.com/splunk/contentctl): The tool that enables us to build, test, and package our content for distribution.
* [Attack data](https://github.com/splunk/attack_data): The is a collection of attack data that is used to test our content.

### Quick Start

1. Setup the environment

    ```bash
    git clone https://github.com/splunk/security_content.git
    cd security_content
    python3.11 -m venv .venv
    source .venv/bin/activate
    pip install contentctl

2. Create a new detection.yml and answer the questions

    ```bash
    contentctl new
    ```

3. **NOTE** - Make sure you update the detection.yml with the required fields and values.

4. Validate your content

    ```bash
    contentctl validate
    ```

5. Build an ESCU app

    ```bash
    contentctl build --enrichments
    ```

6. Test the content - Our testing framework is based on [contentctl](https://github.com/splunk/contentctl) and is extensive and flexible. Refer to the [contentctl test documentation](https://github.com/splunk/contentctl/blob/093d75b2cd18ee84aa80b6a9ab38fe8a830f8ce0/docs/ContentTestingGuide.md) to learn more about the testing framework.

## 3.-Content-Structure-and-Versioning

### Content Parts

* [stories/](https://github.com/splunk/security-content/tree/develop/stories/): All Analytic Stories shipped in ESCU have their
* [detections/](https://github.com/splunk/security-content/tree/develop/detections/): Splunk Enterprise, Splunk UBA, and Splunk Phantom detections that power Analytic Stories
* [deployments/](https://github.com/splunk/security-content/tree/develop/deployments/): Deployment configurations for scheduling correlation searches in Enterprise Security
* [macros/](https://github.com/splunk/security-content/tree/develop/macros/): Macros that are used by the detections
* [lookups/](https://github.com/splunk/security-content/tree/develop/lookups/): Lookups that are used by the detections
* [playbooks/](https://github.com/splunk/security-content/tree/develop/playbooks/): Playbook configurations that are associated with analytic stories
* [dashboards/](https://github.com/splunk/security-content/tree/develop/dashboards/): Contains xml configuration for the dashboards shipped with the app

### Release Versioning

Each Splunk Security Content [release](https://github.com/splunk/security-content/releases) follows a 3 number structure: **\<major\>.\<minor\>.\<patch\>** for example `3.9.1`. The following is an explanation of what each number signifies and when the numbers change.

* **\<major\>** - This number pertains to the specification/schema version our content is adhering to. Today we are in spec 3.0. This number only changes when we make a schema change or update.
* **\<minor\>** - This number pertains to the update we are on. This number increases every time we introduce a new piece of content. Examples of content include, but are not limited to, the following: detections, stories, responses, and so on.  
* **\<patch\>** - This number pertains to fixes for content. This number increases every time we resolve a bug with a current piece of content but do not introduce any new functionality.

We did not come up with this concept and are just implementing semantic versioning per <https://semver.org/>. Note that release announcements are only sent out for major and minor changes, but not usually for patches unless they contain critical issues that require communication.

### Content Versioning of yaml files

* **version** - This number is an integer and is bumped every time a yaml file is changed
* **date** - This date string is updated every time the content is modified

## 3.1-Security-Content-Code

The code in security content is using the following concepts:

* Clean Architecture
* Software Engineering Design Patterns
* Test Driven Development

Concepts from the following books about clean architecture are followed to structure the code:

* [Clean Architecture: A Craftsman’s Guide to Software Structure and Design](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)
* [Implementing the Clean Architecture](https://leanpub.com/implementing-the-clean-architecture)

### Clean Architecture

![CleanArchitecture.jpg](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

The Clean Architecture provides the following advantages:

* Independent of Frameworks
* Testability
* Independent of Database
* Independent of any external agency

An important concept of Clean Architecture is the dependency rule. This rule says that source code dependencies can only point inwards. Nothing in an inner circle can know anything at all about something in an outer circle. In particular, the name of something declared in an outer circle must not be mentioned by the code in the an inner circle. That includes, functions, classes. variables, or any other named software entity. This leads to that changes in code of an outer circle has no impact on the code of an inner circle.

[More information](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

Based on this guidelines, the security content project is structured as following under bin/contentctl_project:

* contentctl_core: Contains the two inner circles consisting of Domain and Application. Domain consists of Entities which are objects like detections, stories and so on. The application layer consists of Interface Adapter, Interface Builder, Factories and Use Cases.
* contentctl_infrastructure: Contains the third inner circle and contains the Implementation of Adapter, Builder and Database specific writer and reader classes.

### Software Engineering Design Patterns

The following software design patterns are used:

* [Builder](https://sourcemaking.com/design_patterns/builder)
* [Factory](https://sourcemaking.com/design_patterns/factory_method)
* [Adapter](https://sourcemaking.com/design_patterns/adapter)

#### Builder

##### Problem - Builder

An application needs to create the elements of a complex aggregate. The specification for the aggregate exists on secondary storage and one of many representations needs to be built in primary storage.

##### Intent - Builder

* Separate the construction of a complex object from its representation so that the same construction process can create different representations.
* Parse a complex representation, create one of several targets.

##### Builder Implementation in Security Content

In security content the software design pattern builder is used to build the different security content object such as detections.
In this software design pattern a director is used to execute the different steps needed to build a detection.
Example from bin/contentctl/contentctl_infrastructure/builder/security_content_director.py

````python
    def constructDetection(self, builder: DetectionBuilder, path: str, deployments: list, playbooks: list, baselines: list, tests: list, attack_enrichment: dict, macros: list, lookups: list) -> None:
        builder.reset()
        builder.setObject(os.path.join(os.path.dirname(__file__), path))
        builder.addDeployment(deployments)
        builder.addRBA()
        builder.addNesFields()
        builder.addAnnotations()
        builder.addMappings()
        builder.addBaseline(baselines)
        builder.addPlaybook(playbooks)
        builder.addUnitTest(tests)
        builder.addMitreAttackEnrichment(attack_enrichment)
        builder.addMacros(macros)
        builder.addLookups(lookups)
````

[More information](https://sourcemaking.com/design_patterns/builder)

#### Factory

##### Problem - Factory

A framework needs to standardize the architectural model for a range of applications, but allow for individual applications to define their own domain objects and provide for their instantiation.

##### Intent - Factory

* Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

* Defining a "virtual" constructor.
* The new operator considered harmful.

##### Factory Implementation in Security Content

In security content the software design pattern factory is used to iterate through the different security content objects. Subsequently, the factory is executing the director for every security content object, which then executes the corresponding builder.
Example from bin/contentctl/contentctl_core/application/factory/factory.py

````python
     def createSecurityContent(self, type: SecurityContentType) -> list:
          objects = []
          if type == SecurityContentType.deployments:
               files = Utils.get_all_yml_files_from_directory(os.path.join(self.input_dto.input_path, str(type.name), 'ESCU'))
          elif type == SecurityContentType.unit_tests:
               files = Utils.get_all_yml_files_from_directory(os.path.join(self.input_dto.input_path, 'tests'))
          else:
               files = Utils.get_all_yml_files_from_directory(os.path.join(self.input_dto.input_path, str(type.name)))
          
          for file in files:
               if not 'ssa__' in file:
                    if type == SecurityContentType.lookups:
                         self.input_dto.director.constructLookup(self.input_dto.basic_builder, file)
                         self.output_dto.lookups.append(self.input_dto.basic_builder.getObject())
                    
                    elif type == SecurityContentType.macros:
                         self.input_dto.director.constructMacro(self.input_dto.basic_builder, file)
                         self.output_dto.macros.append(self.input_dto.basic_builder.getObject())
                    
                    elif type == SecurityContentType.deployments:
                         self.input_dto.director.constructDeployment(self.input_dto.basic_builder, file)
                         self.output_dto.deployments.append(self.input_dto.basic_builder.getObject())
                    
                    elif type == SecurityContentType.playbooks:
                         self.input_dto.director.constructPlaybook(self.input_dto.playbook_builder, file, self.output_dto.detections)
                         self.output_dto.playbooks.append(self.input_dto.playbook_builder.getObject())                    
                    
                    elif type == SecurityContentType.baselines:
                         self.input_dto.director.constructBaseline(self.input_dto.baseline_builder, file, self.output_dto.deployments)
                         baseline = self.input_dto.baseline_builder.getObject()
                         self.output_dto.baselines.append(baseline)
                    
                    elif type == SecurityContentType.investigations:
                         self.input_dto.director.constructInvestigation(self.input_dto.investigation_builder, file)
                         investigation = self.input_dto.investigation_builder.getObject()
                         self.output_dto.investigations.append(investigation)

                    elif type == SecurityContentType.stories:
                         self.input_dto.director.constructStory(self.input_dto.story_builder, file, 
                              self.output_dto.detections, self.output_dto.baselines, self.output_dto.investigations)
                         story = self.input_dto.story_builder.getObject()
                         self.output_dto.stories.append(story)
               
                    elif type == SecurityContentType.detections:
                         self.input_dto.director.constructDetection(self.input_dto.detection_builder, file, 
                              self.output_dto.deployments, self.output_dto.playbooks, self.output_dto.baselines,
                              self.output_dto.tests, self.input_dto.attack_enrichment, self.output_dto.macros,
                              self.output_dto.lookups)
                         detection = self.input_dto.detection_builder.getObject()
                         self.output_dto.detections.append(detection)
               
                    elif type == SecurityContentType.unit_tests:
                         self.input_dto.director.constructTest(self.input_dto.basic_builder, file)
                         test = self.input_dto.basic_builder.getObject()
                         self.output_dto.tests.append(test)
````

[More information](https://sourcemaking.com/design_patterns/factory_method)

#### Adapter

##### Problem - Adapter

An "off the shelf" component offers compelling functionality that you would like to reuse, but its "view of the world" is not compatible with the philosophy and architecture of the system currently being developed.

##### Intent - Adapter

* Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

* Wrap an existing class with a new interface.
* Impedance match an old component to a new system

##### Adapter Implementation in Security Content

The software design pattern Adapter is used to write the data objects to disk. Based on the different use cases in security content, there exists different Adapter based on the output format such as ObjToConfAdapter, ObjToJsonAdapter, ObjToYmlAdapter, ObjToMdAdapter, ...
Example from bin/contentctl/contentctl_infrastructure/adapter/obj_to_conf_adapter.py

````python
import os
import glob
import shutil

from bin.contentctl_project.contentctl_core.application.adapter.adapter import Adapter
from bin.contentctl_project.contentctl_infrastructure.adapter.conf_writer import ConfWriter
from bin.contentctl_project.contentctl_core.domain.entities.enums.enums import SecurityContentType


class ObjToConfAdapter(Adapter):

    def writeHeaders(self, output_folder: str) -> None:
        ConfWriter.writeConfFileHeader(os.path.join(output_folder, 'default/analyticstories.conf'))
        ConfWriter.writeConfFileHeader(os.path.join(output_folder, 'default/savedsearches.conf'))
        ConfWriter.writeConfFileHeader(os.path.join(output_folder, 'default/collections.conf'))
        ConfWriter.writeConfFileHeader(os.path.join(output_folder, 'default/es_investigations.conf'))
        ConfWriter.writeConfFileHeader(os.path.join(output_folder, 'default/macros.conf'))
        ConfWriter.writeConfFileHeader(os.path.join(output_folder, 'default/transforms.conf'))
        ConfWriter.writeConfFileHeader(os.path.join(output_folder, 'default/workflow_actions.conf'))


    def writeObjects(self, objects: list, output_path: str, type: SecurityContentType = None) -> None:
        if type == SecurityContentType.detections:
            ConfWriter.writeConfFile('savedsearches_detections.j2', 
            os.path.join(output_path, 'default/savedsearches.conf'), 
            objects)

            ConfWriter.writeConfFile('analyticstories_detections.j2',
                os.path.join(output_path, 'default/analyticstories.conf'), 
                objects)

            ConfWriter.writeConfFile('macros_detections.j2',
                os.path.join(output_path, 'default/macros.conf'), 
                objects)
        
        elif type == SecurityContentType.stories:
            ConfWriter.writeConfFile('analyticstories_stories.j2',
                os.path.join(output_path, 'default/analyticstories.conf'), 
                objects)

        elif type == SecurityContentType.baselines:
            ConfWriter.writeConfFile('savedsearches_baselines.j2', 
                os.path.join(output_path, 'default/savedsearches.conf'), 
                objects)

        elif type == SecurityContentType.investigations:
            ConfWriter.writeConfFile('savedsearches_investigations.j2', 
                os.path.join(output_path, 'default/savedsearches.conf'), 
                objects)
            
            ConfWriter.writeConfFile('analyticstories_investigations.j2', 
                os.path.join(output_path, 'default/analyticstories.conf'), 
                objects)

            workbench_panels = []
            for investigation in objects:
                if investigation.inputs:
                    response_file_name_xml = investigation.lowercase_name + "___response_task.xml"
                    workbench_panels.append(investigation)
                    investigation.search = investigation.search.replace(">","&gt;")
                    investigation.search = investigation.search.replace("<","&lt;")
                    ConfWriter.writeConfFileHeader(os.path.join(output_path, 
                        'default/data/ui/panels/', str("workbench_panel_" + response_file_name_xml)))
                    ConfWriter.writeConfFile('panel.j2', 
                        os.path.join(output_path, 
                        'default/data/ui/panels/', str("workbench_panel_" + response_file_name_xml)),
                        [investigation.search])

            ConfWriter.writeConfFile('es_investigations_investigations.j2', 
                os.path.join(output_path, 'default/es_investigations.conf'), 
                workbench_panels)

            ConfWriter.writeConfFile('workflow_actions.j2', 
                os.path.join(output_path, 'default/workflow_actions.conf'), 
                workbench_panels)   

        elif type == SecurityContentType.lookups:
            ConfWriter.writeConfFile('collections.j2', 
                os.path.join(output_path, 'default/collections.conf'), 
                objects)

            ConfWriter.writeConfFile('transforms.j2', 
                os.path.join(output_path, 'default/transforms.conf'), 
                objects)

            files = glob.iglob(os.path.join(os.path.dirname(__file__), '../../../..' , 'lookups', '*.csv'))
            for file in files:
                if os.path.isfile(file):
                    shutil.copy(file, os.path.join(output_path, 'lookups'))

        elif type == SecurityContentType.macros:
            ConfWriter.writeConfFile('macros.j2', 
                os.path.join(output_path, 'default/macros.conf'), 
                objects)
````

[More information](https://sourcemaking.com/design_patterns/adapter)

### Test Driven Development

Test Driven Development (TDD) is software development approach in which test cases are developed to specify and validate what the code will do. In simple terms, test cases for each functionality are created and tested first and if the test fails then the new code is written in order to pass the test and making code simple and bug-free. Test-Driven Development starts with designing and developing tests for every small functionality of an application.

For test driven development in security content, pytest is used. Pytest will test every python file which starts with test_*. Inside the python file, it will run every method which starts with test_*. Every class in security content should have a corresponding test file. Pytest can be use in the following way:

````python
export PYTHONPATH="/path/to/security_content/"
pytest -s bin/contentctl_project 
````

#### Implementation in Security Content

Every class in security content contains a corresponding test file, e.g.:
bin/contentctl/contentctl_infrastructure/builder/cve_enrichment.py

````python
from pycvesearch import CVESearch

CVESSEARCH_API_URL = 'https://cve.circl.lu'

class CveEnrichment():

    @classmethod
    def enrich_cve(self, cve_id: str) -> dict:
        cve = CVESearch(CVESSEARCH_API_URL)
        result = cve.id(cve_id)
        cve_enriched = dict()
        cve_enriched['id'] = cve_id
        cve_enriched['cvss'] = result['cvss']
        cve_enriched['summary'] = result['summary']
        return cve_enriched
````

bin/contentctl/contentctl_infrastructure/tests/builder/test_cve_enrichment.py

````python
from bin.contentctl_project.contentctl_infrastructure.builder.cve_enrichment import CveEnrichment

def test_cve_enrichment():
    cve_enrichment = CveEnrichment.enrich_cve('CVE-2021-34527')
    assert cve_enrichment['id'] == 'CVE-2021-34527'
    assert cve_enrichment['cvss'] == 9.0
    assert cve_enrichment['summary'] == 'Windows Print Spooler Remote Code Execution Vulnerability'
````

[More information](https://www.guru99.com/test-driven-development.html)

#### Security Content Code Execution Flow

The execution of contentctl is the following:

1. Main executable contentctl.py
2. Executes specific use case e.g. bin/contentctl/contentctl_core/application/use_case/generate.py
3. Read all security content objects using factory e.g. bin/contentctl/contentctl_core/application/factory/factory.py

   * Factory is using director and builder to read and enrich security content objects.

4. Write security content object into files using adapter e.g. bin/contentctl/contentctl_infrastructure/adapter/obj_to_conf_adapter.py

#### Common Tasks

##### Add additional validators for a specific security content obj such as detection

* Simple validators are added into the security content object e.g. bin/contentctl/contentctl_core/domain/entities/detection.py. This validator validates if the detection name is smaller then 75 characters:

````python
    @validator('name')
    def name_max_length(cls, v):
        if len(v) > 75:
            raise ValueError('name is longer then 75 chars: ' + v)
        return v
````

Complex validation which needs multiple security content objects such as detection + story is added into the use case validate under bin/contentctl/contentctl_core/application/use_case/validate.py

````python
...
    def validate_detection_exist_for_test(self, tests : list, detections: list):
        for test in tests:
            found_detection = False
            for detection in detections:
                if test.tests[0].file in detection.file_path:
                     found_detection = True

            if not found_detection:
                ValueError("detection doesn't exist for test file: " + test.name)
...
````

##### Add another enrichment step to a security content object such as detection

First, add the new enrichment step into the interface class of the builder under:
bin/contentctl/contentctl_core/application/builder/detection_builder.py

````python
    @abc.abstractmethod
    def addCve(self) -> None:
        pass
````

Second, add the implementation of this new function under:
bin/contentctl/contentctl_infrastructure/builder/security_content_detection_builder.py

````python
...
    def addCve(self) -> None:
        self.security_content_obj.cve_enrichment = []
        for cve in self.security_content_obj.tags.cve:
            self.security_content_obj.cve_enrichment.append(CveEnrichment.enrich_cve(cve))
...
````

Third, add the new enrichment step to the director under:
bin/contentctl/contentctl_infrastructure/builder/security_content_director.py:

````python
...
    def constructDetection(self, builder: DetectionBuilder, path: str, deployments: list, playbooks: list, baselines: list, tests: list, attack_enrichment: dict, macros: list, lookups: list) -> None:
        builder.reset()
        builder.setObject(os.path.join(os.path.dirname(__file__), path))
        builder.addDeployment(deployments)
        builder.addRBA()
        builder.addNesFields()
        builder.addAnnotations()
        builder.addMappings()
        builder.addBaseline(baselines)
        builder.addPlaybook(playbooks)
        builder.addUnitTest(tests)
        builder.addMitreAttackEnrichment(attack_enrichment)
        builder.addMacros(macros)
        builder.addLookups(lookups)
        builder.addCve()
...
````

## 4.-Developing-ESCU-Content

### Writing Content

#### Pre-Requisites

Before you begin, follow the steps to install **dependencies**

1. Install `contentctl`. This requires Python 3.11 or Python 3.12. We recommend using [pipx](https://pipx.pypa.io/stable/) to install and manage it (`pipx install contentctl`).

#### Steps for Writing Content

1. Select the content [piece](https://github.com/splunk/security-content/wiki/Content-Structure) you want to write.
2. Copy an example and edit it to suit your needs. At a minimum, you must write [a detection search](https://github.com/splunk/security-content/tree/develop/detections).
3. Make a pull request.

#### Testing New Content

1. Run `contentctl validate` locally. This will ensure your new content or changes are all still valid.
2. If you'd like to run detection testing locally, you can run `contentctl test --no_enable-integration-testing  mode:changes --mode.target-branch develop` which will run unit testing for each detection that differs from the develop branch.

The pull request will trigger a GitHub Actions run, a continuous-integration app that integrates with a VCS and automatically runs a series of steps every time that it detects a change to your repository. A GitHub Actions run consists of a series of steps, usually build, test, and deployment. If your tests pass, you're good to go! A repository maintainer will make sure the PR makes it into the next release. Which will be deployed in the [ESCU app](https://splunkbase.splunk.com/app/3449/). If the GitHub Actions check fails, refer to [troubleshooting](https://github.com/splunk/security-content/wiki/Troubleshooting) first, some problems are easily described by CI. If not do not worry, our team will work with you in the PR to make sure your content passes validation and its part of our next release!

For a more detailed explanation on how to contribute to the project, please see ["Contributing"](https://github.com/splunk/security-content/wiki/Contributing-to-the-Project)

### Recommendations

* 🚨 NOTE: If you are just getting started with managing your Splunk detection as code, we recommend that you keep the YAML structure of the detections as close as possible to the original structure of the detections. This will make it easier to manage your detections and will also make it easier to contribute back to the community by creating a pull request to the Splunk Security Content project.

* In order to build an content app that specific for your organization, we strongly recommend that you start with keeping only the detections that are related to your organization and remove other yamls that are not related to your organization. This includes selecting detections, stories, macros, lookups that are used by the detection ymls.

* If your detections are using macros and lookups, please make sure that you have the same macros and lookups in those directories.. This will ensure that the content app is self-contained and does not rely on external files.

## 4.1-Contributing-to-the-Project

This document is the single source of truth on how to contribute to this codebase. Please feel free to browse the open issues and file new ones. All feedback is welcome!

----

### Topics

* [Prerequisites](#prerequisites)
* [Contributor License Agreement](#contributor-license-agreement)
* [Code of Conduct](#code-of-conduct)
* [Setup Development Environment](#setup-development-environment)
* [Contribution Workflow](#contribution-workflow)
* [Feature Requests and Bug Reports](#feature-requests-and-bug-reports)
* [Fixing Issues](#fixing-issues)
* [Pull Requests](#pull-requests)
* [Code Review](#code-review)
* [Documentation](#documentation)
* [Maintainers](#maintainers)

----

### Prerequisites

When contributing to this repository, please first discuss the change you wish to make via a GitHub issue, Slack message, email, or via other channels with the owners of this repository.

#### Contributor License Agreement

We accept all pull requests from the Github community

If you wish to be a contributing member of our community, please see the agreement [for individuals](https://www.splunk.com/goto/individualcontributions) or [for organizations](https://www.splunk.com/goto/contributions).

#### Code of Conduct

Please make sure to read and observe our [Code of Conduct](https://github.com/splunk/security-content/wiki/Code-of-Conduct). Please follow it in all of your interactions involving the project.

#### Setup Development Environment

see [Developing Content](https://github.com/splunk/security-content/wiki/Developing-Content)

### Contribution Workflow

Help is always welcome! For example, documentation can always use improvement. There's always code that can be clarified, functionality that can be extended, and tests to be added to guarantee behavior. If you see something you think should be fixed, don't be afraid to own it.

#### Feature Requests and Bug Reports

Have ideas on improvements? See something that needs work? While the community encourages everyone to contribute code, it is also appreciated when someone reports an issue. Please report any issues or bugs you find through [GitHub's issue tracker](https://github.com/splunk/security-content/issues).

If you are reporting a bug, please include:

* Your operating system name and version
* Any details about your local setup that might be helpful in troubleshooting (ex. Python interpreter version, Splunk version, etc.)
* Detailed steps to reproduce the bug

We'd also like to hear about your propositions and suggestions. Feel free to submit them as issues and:

* Explain in detail how they should work
* Note that keeping the scope as narrow as possible will make the suggestion easier to implement

#### Fixing Issues

Look through our [issue tracker](https://github.com/splunk/security-content/issues) to find problems to fix! Feel free to comment and tag corresponding stakeholders or full-time maintainers of this project with any questions or concerns.

#### Pull Requests

What is a "pull request"? It informs the project's core developers about the changes you want to review and merge. Once you submit a pull request, it enters a stage of code review where you and others can discuss its potential modifications and maybe even add more commits to it later on.

If you want to learn more, please consult this [tutorial on how pull requests work](https://help.github.com/articles/using-pull-requests/) in the [GitHub Help Center](https://help.github.com/).

Here's an overview of how you can make a pull request against this project:

1. Fill out the [Splunk Contribution Agreement](https://www.splunk.com/goto/contributions).
2. Fork the [security_content GitHub repository](https://github.com/splunk/security-content)
3. Clone your fork using git and create a branch off of develop

    ```bash
    $ git@github.com:YOUR_GITHUB_USERNAME/security_content.git
    cd security-content

    # This project uses 'develop' for all development activity, so create your branch off that
    $ git checkout -b your-bugfix-branch-name develop
    ```

4. Make your changes, commit, and push (once your tests have passed)

    ```bash
    cd security-content
    $ git commit -m "<insert helpful commit message>"
    $ git push 
    ```

5. Submit a pull request through the GitHub website, using the changes from your forked codebase

#### Code Review

There are two aspects of code review: giving and receiving.

To make it easier for your PR to receive reviews, keep in mind that the reviewers will need you to:

* Follow the project coding conventions
* Write good commit messages
* Break large changes into a logical series of smaller patches which individually make easily understandable changes, and in aggregate solve a broader issue

Reviewers, the people providing the review, are highly encouraged to revisit the [Code of Conduct](contributing/code-of-conduct.md) and must go above and beyond to promote a collaborative, respectful community.

When reviewing PRs from others, [The Gentle Art of Patch Review](http://sage.thesharps.us/2014/09/01/the-gentle-art-of-patch-review/) suggests an iterative series of focuses designed to lead new contributors to positive collaboration, such as:

* Is the idea behind the contribution sound?
* Is the contribution architected correctly?
* Is the contribution polished?

For this project, we require at least one approval. A build from our continuous integration system must also be successful off of your branch. Please note that any new changes made with your existing pull request during review will automatically unapproved and re-trigger another build/round of tests.

#### Documentation

We can always use improvements to our documentation! Anyone can contribute to these docs--whether you’re new to the project, you’ve been around a long time, or if you just can’t stand seeing typos.

Here's what's needed?

1. More complementary documentation. Have you something unclear?
2. More examples or generic templates that others can use.
3. Blog posts, articles, and such are all very appreciated.

You can also edit documentation files directly in the GitHub web interface, without creating a local copy. This can be convenient for small typos or grammar fixes.

### Maintainers

If you need help, feel free to tag one of the active maintainers of this project in a post or comment. We'll do our best to reach out to you as quickly as we can.

```text
# Active maintainers marked with (*)

(*) Bhavin Patel
(*) Jose Hernandez
(*) Lou Stella
(*) Patrick Bareib
(*) Teoderick Contreras
(*) Eric Mcginnis

```

## 4.2-Customize-to-Your-Environment

### Customizing source types with Macros

When customizing security-content to fit your organization and Splunk deployment, one of the key things to change is what names your different source types have. A great example of this is sysmon data. If collected using the latest [Splunk Add-On for Microsoft Sysmon](https://splunkbase.splunk.com/app/1914/), it will automatically be source typed with:

 `sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational`

This might not be the exact source type used in your organization and Splunk deployment. However, to set your own, simply modify the file: [security-content/macros/sysmon.yml](https://github.com/splunk/security-content/blob/develop/macros/sysmon.yml) and change the value of [definition](https://github.com/splunk/security-content/blob/develop/macros/sysmon.yml#L1). Other, great examples are [okta](https://github.com/splunk/security-content/blob/develop/macros/okta.yml), [wmi](https://github.com/splunk/security-content/blob/develop/macros/wmi.yml), and [streams http](https://github.com/splunk/security-content/blob/develop/macros/stream_http.yml).

### Customizing scheduling and alert actions with deployments

To customize how often any detection run and what alert action they should automatically perform, a deployment configuration must be created. By default, security-content includes a deployment called [deployment configurations](https://github.com/splunk/security-content/blob/develop/deployments/) which schedules all Analytic Stories to run hourly and search in the last 60 minutes. The default deployment also has an alert action configure that creates a notable event in Enterprise Security if anything is detected. This can be easily customized by individual Analytic Story, and Detection by setting a matching tag value. For example:

```yml
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
    to: <soc@splunk.com>
tags:
  analytics_story: Credential Dumping
```

Schedule the [Credential Dumping Story](https://github.com/splunk/security-content/blob/develop/stories/credential_dumping.yml) to be executed daily and send results via email to <soc@splunk.com>. The deployment and Analytic story are linked by the matching tag both share `analytics_story: Credential Dumping`. By default, we also ship deployments for short ([runs hourly, searches back 60 minutes](https://github.com/splunk/security-content/blob/develop/deployments/20_baseline_cache_hourly_updates.yml)) and long ([runs daily, searches back 90 days](https://github.com/splunk/security-content/blob/develop/deployments/30_long_running_baseline_searches.yml) running baseline searches as well. It is important to note that security-content uses the file name alphanumeric order to resolve any conflicts in tags. In other words, if you wanted the Credential Dumping deployment example from above to not overwrite the default configuration file `10_enterprise_security_deployment_configuration.yml`, you would name the deployment file `11_credential_dumping_hourly.yml` or similar.

Follow [these](https://github.com/splunk/security-content/wiki/Generating-a-Splunk-App-with-your-Content) steps to generate your own Splunk App with your changes!

## 5.1-Detection-Naming-Convention

### Summary

Every time a Splunk Security Content analytic is created it should follow the naming convention below. This convention provides us consistent naming as well as organization for our different security content components.

### Format

`<platform> <short_summary>`

#### Where

* `<platform>`: Should represent the platform the detection is targeting AWS, GCP, Linux, Windows, MacOS, Splunk, etc.
* `<short_summary>`: A short and precise summary of the detection, ideally referencing the tooling or technique being detected. See `names should be` for limitations.

### Example

* Windows Registry Dump Via Reg
* Windows Network Scan Via Nmap
* Windows Potential Credential Dumping Activity
* Linux Auditd New User Added
* AWS Multi-Factor Authentication Disabled

### Names should

* Be limited to 64 characters.
* Avoid starting with the word "detect".
* Be as clear and precise as possible in highlighting what the rule is trying to detect.

## 5.2-Detection-Types-and-Status

Splunk Security Content detections has a field called `type` these types will drive workflow in the future on the product, below are the current proposed types:

See <https://car.mitre.org/Glossary> for inspiration.

### Type

| Type        | Description | Example      |
| ----------- | ----------- |--------------|
| TTP | A TTP analytic is designed to detect a certain adversary tactic, technique or procedure. | [Attempted Credential Dump From Registry via Reg exe](https://github.com/splunk/security_content/blob/develop/detections/endpoint/attempted_credential_dump_from_registry_via_reg_exe.yml) |
| Baseline | A posture analytic is designed to help in the maintenance of the analytic or create a baseline of data for detections to leverage. | [Baseline Of Cloud Instances Launched](https://github.com/splunk/security_content/blob/develop/baselines/baseline_of_cloud_instances_launched.yml) |
| Anomaly | An anomaly analytic triggers on behavior that is not normally observed. Anomalous may not be explicitly malicious but may be suspect. For example, detection of executables that have never been run before or a process using the network which does not normally use the network. Like Situational Awareness analytics, anomaly analytics don’t necessarily indicate an attack. | [Abnormally High Number Of Cloud Infrastructure API Calls](https://github.com/splunk/security_content/blob/develop/detections/cloud/abnormally_high_number_of_cloud_infrastructure_api_calls.yml) |
| Hunting | A detection that increases the risk of an asset or entity, although tends to be too noisy to generate a notable event by itself. It leverages aggregated risk from various other detections to produce a notable. Also known as hunting queries.  | [Common Ransomware Extensions](https://github.com/splunk/security_content/blob/develop/detections/endpoint/common_ransomware_extensions.yml) |
| Correlation | An analytic that correlates various detection results to correlate a high level threat and its primary purpose is to generate a notable. | [Windows Post Exploitation Risk Behavior](https://github.com/splunk/security_content/blob/develop/detections/endpoint/windows_post_exploitation_risk_behavior.yml) |
| Investigation | These analytics are searches that leverage tokens and are used in the [prebuilt panels](https://github.com/splunk/security_content/tree/v4.30.0/dist/DA-ESS-ContentUpdate/default/data/ui/panels) shipped by ESCU for Investigative Workbench in ES | [AWS Investigate Security Hub alerts by dest](https://github.com/splunk/security_content/blob/develop/investigations/aws_investigate_security_hub_alerts_by_dest.yml) |

### Detection Configurations

Below is a table showing how each type is configured out of the box in ESCU.

| Analytic Type | Generates Notable | Increases Risk (RBA) | Triggers Playbook | Tied to a Dashboard | Runs on CRON Schedule | Enabled OOB |  
| ------------- | ----------------- | -------------------- | ----------------- | ------------------- | --------------------- | ----------- |
| Hunting | No | No | No | No | No | No |
| TTP | Yes | Yes | Yes | No | Yes | No |
| Baseline | No | No | No | No | Yes | No |
| Anomaly | No | Yes | No | No | Yes | No |
| Correlation | Yes | No | No | No | Yes | No |

### Status

Status | Explanation

-- | --
Production | These are fully-tested detections in Splunk Enterprise Security environment with latest Splunk TAs installed against the associated attack data
Experimental | These detections DO NOT have an associated attack data because we were either not able to simulate the attack or that the attack data contains sensitive information that we were not able to publish to our attack data repository
Deprecated | These detections are deprecated and no longer supported or maintained by Splunk. Usually, the description of a deprecated detections have a note regarding why the said detection is deprecated and if there is a replacement detection available

## 5.3-ESCU-savedsearch.conf-spec

### YAML SPECS

YAML specs define the structure and required fields for various YAML configuration files used in the project. These specifications ensure consistency and validation across different types of YAML files, such as macros, lookups, and analytic stories. Each spec outlines the expected data types, descriptions, and whether the fields are mandatory, providing a clear schema for developers to follow.

* [detection](https://github.com/splunk/security_content/blob/develop/docs/yaml-spec/detection_spec.yml)
* [stories](https://github.com/splunk/security_content/blob/develop/docs/yaml-spec/stories_spec.yml)
* [macros](https://github.com/splunk/security_content/blob/develop/docs/yaml-spec/macros_spec.yml)
* [lookups](https://github.com/splunk/security_content/blob/develop/docs/yaml-spec/lookups_spec.yml)

### `savedsearches.conf` spec

#### Stanza Header

* **`[ESCU - <Detection Name> - Rule]`**
  * The name of the saved search. Detection Name has to be less than 67 characters (Excluding ESCU - , - Rule)

#### ESCU/Saved Search Settings

* **`action.escu = 0`**
  * Disables or enables a specific ESCU action. `0` indicates disabled.
* **`action.escu.enabled = 1/0`**
  * Specifically enables an ESCU action, `1` for enabled.
* **`description`**
  * Provides a brief description and deprecation notice for the saved search.
* **`disabled = true/false`**
  * Indicates that the search is currently disabled.

#### Mapping and Metadata

* **`action.escu.mappings = {"cis20": ["CIS 10"], "mitre_attack": ["T1110"], "nist": ["DE.AE"]}`**
  * Defines mapping to security frameworks like CIS20 and NIST.
* **`action.escu.data_models = ["Authentication"]`**
  * Specifies the data models required, if any.
* **`action.escu.analytic_story = ["Suspicious Okta Activity"]`**
  * Associates the search with broader analytic themes, such as "Suspicious Okta Activity".

#### Implementation and Usage

* **`action.escu.eli5 = <string>`**
  * Offers a simplified explanation of the search's purpose.
* **`action.escu.how_to_implement = <string>`**
  * Provides implementation guidance, including suggested data sources.
* **`action.escu.known_false_positives = <string>`**
  * Discusses potential false positives and general mitigation strategies.

#### Dates and Confidence

* **`action.escu.creation_date = <string>`**
  * The date when the search was created.
* **`action.escu.modification_date = <date>`**
  * The date when the search was last modified.
* **`action.escu.confidence = <string>`**
  * The confidence level in the search's effectiveness, marked as "low, medium,high".

#### Search Identification

* **`action.escu.full_search_name =  ESCU - <Detection Name> - Rule`**
  * The full name of the search within the ESCU framework.
* **`action.escu.search_type = detection`**
  * Specifies the type of search: detection (For all ESCU searches)

#### Product and Technology

* **`action.escu.product = ["Splunk Enterprise", "Splunk Enterprise Security", "Splunk Cloud"]`**
  * Lists relevant Splunk products.
* **`action.escu.providing_technologies = ["Okta"]`**
  * Indicates the technologies providing data for the search.

#### Scheduling and Dispatch

* **`cron_schedule`**
  * Defines the cron schedule for the search.
* **`dispatch.earliest_time` and `dispatch.latest_time`**
  * Set the time range for the search.
* **`schedule_window = auto`**
  * Splunk manages scheduling windows automatically.

#### Alert and Visibility

* **`alert.digest_mode = 1`**
  * Enables alert digest mode.
* **`is_visible = false`**
  * Controls the visibility of the search in the UI.

#### Search Query

* **`search`**
  * Contains the actual Splunk search query.

#### Additional Settings

* **`enableSched = 1`**
  * Enables scheduling for the search.
* **`relation  = greater than`**
  * Sets the relational condition for alert triggering.
* **`quantity`**
  * Sets the threshold quantity for the relational condition.
* **`realtime_schedule = 0`**
  * Disables real-time scheduling.

#### Risk and Correlation (Enterprise Security related configs)

* **`action.risk = 1`**
  * Enables risk-based actions based on the search results.
* **`cron_schedule`**
  * Defines the cron schedule for the search to run, set to at the top of every hour.
* **`dispatch.earliest_time` and `dispatch.latest_time`**
  * Configures the time range for the search, from 70 minutes to 10 minutes before the current time.
* **`action.correlationsearch.enabled = 1`**
  * Enables correlation search features, linking the search to broader analytical contexts.
* **`schedule_window = auto`**
  * Allows Splunk to automatically manage scheduling windows to optimize search performance.
* **`action.notable = 1`**
  * Indicates that events found by this search are to be marked as notable.
* **`alert.digest_mode = 1`**
  * Enables alert digest mode, aggregating alerts for efficiency.

#### Risk Settings

* **`action.risk.param._risk_message = <string>`**
  * Defines the message to be displayed for a risk event, incorporating dynamic variables for user and source IP address.
* **`action.risk.param._risk = [{"risk_object_field": "user", "risk_object_type": "other", "risk_score": 81}, {"threat_object_field": "src", "threat_object_type": "ip_address"}]`**
  * Details the risk scoring parameters, including object fields for user and source IP, along with the risk score.
* **`action.risk.param._risk_score = 0`**
  * Sets the base risk score. (For older versions of ES)
* **`action.risk.param.verbose = 0`**
  * Controls verbosity of the risk action’s output, with `0` disabling verbose output.

#### Correlation Search Settings

* **`action.correlationsearch.label = ESCU - <Detection Name> - Rule`**
  * Provides a descriptive label for the correlation search.
* **`action.correlationsearch.annotations = {"analytic_story": ["Suspicious Okta Activity"], "cis20": ["CIS 10"], "confidence": 60, "impact": 80, "mitre_attack": ["T1586", "T1586.003", "T1078", "T1078.004", "T1621"], "nist": ["DE.CM"]}`**
  * Adds metadata annotations to the correlation search, linking to analytic stories, compliance standards, and attack techniques.

#### Notable Event Configuration

* **`action.notable.param.nes_fields`**
  * Specifies the fields to be included in the notable event summary.
* **`action.notable.param.rule_description`**
  * Provides a detailed description of the analytic rationale behind the search, identifying failed MFA challenge attempts in Okta authentication processes.
* **`action.notable.param.rule_title`**
  * Sets a concise title for the notable event generated by this search.
* **`action.notable.param.security_domain`**
  * Associates the search with a security domain, in this case, identity,
* **`action.notable.param.severity`**
  * Defines the severity of the notable event, marked as high.

### Sample Config Configuration

```conf
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

## 5.4-Deprecated-Detections

### What happens to objects under deprecated

Detections under the [deprecated](https://github.com/splunk/security_content/tree/develop/detections/deprecated) folder are treated a bit differently, below is a list of things that happen to them specifically:

* `doc_gen.py` will no longer include deprecated detections on [Splunk Docs](https://docs.splunk.com/Documentation/ESSOC/3.23.0/detections/Detections).
* The correlation search label is updated to `ESCU - Deprecated -<search_name> - Rule`
* research.splunk.com shows a deprecation WARNING
![image](https://user-images.githubusercontent.com/1476868/200946466-0155d9bc-224f-463c-938f-7fa68a402d48.png)
* The following note is added to the beginning of the description of the deprecated detection:

    ```text
     WARNING, this detection has been marked deprecated by the Splunk Threat Research team, which means that it will no longer be maintained or supported. If you have any questions feel free to email us at: research@splunk.com.*
    ```

* Note the detections are **still** included on the ESCU package

### When do we deprecate a detection

* The attack or analytic is no longer relevant.
* STRT builds a better approach to the analytic.
* Uses a data source or TA no longer supported.
* When a data source becomes CIM compliant, we deprecate the _raw source type searches and convert them into a data model based search.
* If we get enough bug reports and it does not make sense to maintain or meet our bar for quality

## 6.1-How-are-risk-score-calculated-for-RBA

Each ESCU detection now comes with an attached risk_score value, this value is derived from a simple formula risk_score=(impact * confidence)/100. Where impact and confidence are 0-100 number values based on the detection author's judgment. Impact captures the effect the attack will have on an organization and confidence in how accurate the analytics is at catching said threat. This is also documented under each detections in [research.splunk.com](http://research.splunk.com/) see: <https://research.splunk.com/endpoint/attempted_credential_dump_from_registry_via_reg_exe/#rba>

### Definition

**Impact**: This refers to the potential severity or consequences of the threat or activity that the detection rule is designed to identify. Impact measures how significant a threat is in terms of potential damage or risk to the organization's assets, data, or operations. For example, a rule detecting a potential data breach would have a high impact due to the severe consequences of data loss or exposure.

**Confidence**: This is a measure of how accurately a detection rule identifies genuine threats without generating false positives. A rule with high confidence can reliably distinguish between actual malicious activity and benign actions that might appear suspicious. Confidence is crucial for ensuring that security teams are alerted to real threats without being overwhelmed by false alarms.
Impact and Confidence today are somewhat arbitrary base on the detection author.

The rule of thumb we use is:

### Scoring for impact and confidence

* 0-24 **low**
* 25-49 **medium**
* 50-79 **high**
* 80-100 **critical** / **extremely high**

### Meaning of Impact values

* **Low Impact**: These are threats that have minimal consequences for the organization. They might represent minor policy violations or low-level security risks that don't significantly compromise data, systems, or operations. Actions triggered by low impact rules often don't require immediate response but may need monitoring or periodic review.

* **Medium Impact**: This category includes threats that could potentially cause moderate harm to the organization's assets, reputation, or operations. They might not be immediately critical but could escalate if not addressed. Medium impact rules typically warrant timely investigation to assess and mitigate potential risks.

* **High Impact**: High impact threats pose a significant risk to the organization. They could lead to substantial data loss, service disruption, financial loss, or other serious consequences. Detection rules with high impact require immediate attention and action to prevent or limit damage.

* **Critical Impact**: This highest level is reserved for the most severe threats, which pose an imminent and critical risk to the organization’s core operations, data security, or existence. These might include advanced persistent threats (APTs), major data breaches, or other catastrophic events. Critical impact rules necessitate an urgent and comprehensive response, often involving multiple layers of the organization, including top management.

### Meaning of Confidence values

* **Low Confidence**: Detection rules with low confidence are likely to generate a higher number of false positives. These rules might be based on less reliable indicators of compromise or vague behavioral patterns. They require more validation and are typically used for initial flagging rather than immediate action.

* **Medium Confidence**: Medium confidence rules strike a balance between accuracy and sensitivity. They are more reliable than low confidence rules but still have a moderate risk of false positives. These rules might require correlation with additional data or context to ascertain the legitimacy of the alert.

* **High Confidence**: High confidence rules are expected to accurately detect threats with minimal false positives. They are based on well-corroborated indicators or behaviors strongly associated with malicious activity. Alerts generated by these rules are often taken seriously and may trigger immediate investigation.

* **Extremely High Confidence**: Rules at this level have the highest degree of reliability and are based on definitive, proven indicators of compromise or attack patterns. Critical confidence implies that an alert almost certainly indicates a real threat. Responses to these alerts are prioritized, and immediate action is often required to address the detected issue.

### Example Table

![image](https://user-images.githubusercontent.com/1476868/187281619-950d2f16-68d4-4488-9a8e-012af10f2d3d.png)
_credits to: Ryan Long

## 6.2-How-to-add-ML-model-files-to-ESCU

### Writing MLTK Content

**NOTE:** This is specifically for shipping pre trained models. The model files can be created either by leveraging [`fit`](https://docs.splunk.com/Documentation/MLApp/latest/User/Customsearchcommands) command in MLTK or by other custom ML tools

### Files needed in a PR

1. In detections/ [<detection_name>.yml](https://github.com/splunk/security_content/blob/bdb8f427f866bd2c8e55bc3721ca95cefd28dd4c/detections/endpoint/potentially_malicious_code_on_commandline.yml)
   * The SPL in this file uses the MLTK apply command

2. In lookups/ directory
   * The actual model file ending with [<model_name>.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_unusual_commandline_detection.mlmodel)
   * A [<lookup_name>.yml file](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_unusual_commandline_detection.yml) that references the model file:

3. In tests/ [<test_name.test>.yml](https://github.com/splunk/security_content/blob/develop/tests/endpoint/potentially_malicious_code_on_commandline.test.yml) with a reference to a test data set in [attack_data](https://github.com/splunk/attack_data) repository

### Building detection and test yml template files

 `python contentctl.py -p . new_content -t detection`

## 6.3-How-to-deploy-pre-trained-Deep-Learning-models-for-ESCU

### Steps to deploy a pre-trained deep learning model for ESCU

NOTE: The following instructions are specifically for deploying pre-trained deep learning models using Splunk App for Data Science and Deep Learning (DSDL)5.0 in ESCU.  The deployed models are then used with "apply" command in ESCU detections.

The steps are outlined as follows -

1. Set up Splunk App for Data Science and Deep Learning (DSDL) 5.0
2. Download the model artifacts
3. Deploy model artifacts into Splunk app DSDL

### Set up Splunk App for Data Science and Deep Learning (DSDL)

1. Install the DSDL app (<https://splunkbase.splunk.com/app/4607/>) 5.0 on Splunk instance and follow the steps in the Overview > User Guide.
2. Additional information and FAQs are available here <https://splunkbase.splunk.com/app/4607/#/details>.
3. Ensure the containers use Golden Image CPU 5.0.0

### Download the model artifacts

1. Download the pre-trained model file .tar.gz from the link provided here.

    | Detection        | Model |
    | ----------- | ----------- |
    | Detect DGA domains using pre-trained model in DSDL | [pre-trained model](https://seal.splunkresearch.com/pretrained_dga_model_dsdl.tar.gz) |
    | Detect DNS Tunneling using TXT record responses | [pre-trained model](https://seal.splunkresearch.com/detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.tar.gz) |
    | Detect suspicious process names | [pre-trained model](https://seal.splunkresearch.com/detect_suspicious_processnames_using_pretrained_model_in_dsdl.tar.gz) |
    | Detect DNS Data Exfiltration | [pre-trained model](https://seal.splunkresearch.com/detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.tar.gz) |

2. Download notebook .ipynb for the detection

    | Detection        | Notebook |
    | ----------- | ----------- |
    | Detect DGA domains using pre-trained model in DSDL | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/pretrained_dga_model_dsdl.ipynb) |
    | Detect DNS Tunneling using TXT record responses | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.ipynb) |
    | Detect suspicious process names | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_processnames_using_pretrained_model_in_dsdl.ipynb) |
    | Detect DNS Data Exfiltration | [notebook](https://github.com/splunk/security_content/blob/develop/notebooks/detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.ipynb) |

3. Download model configuration file .json for the detection

    | Detection        | .json file |
    | ----------- | ----------- |
    | Detect DGA domains using pre-trained model in DSDL | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/pretrained_dga_model_dsdl.json) |
    | Detect DNS Tunneling using TXT record responses | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.json) |
    | Detect suspicious process names | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/detect_suspicious_processnames_using_pretrained_model_in_dsdl.json) |
    | Detect DNS Data Exfiltration | [.json](https://github.com/splunk/security_content/blob/develop/notebooks/detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.json) |

4. Follow the below steps only if you are deploying the model to use outside of ESCU. For ex: in a simple SPL search using |apply command.
   Place these .mlmodel files into app context /lookup directory. For ex: etc/apps/mltk-container/lookups
  
    | Detection        | .mlmodel file |
    | ----------- | ----------- |
    | Detect DGA domains using pre-trained model in DSDL | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_pretrained_dga_model_dsdl.mlmodel) |
    | Detect DNS Tunneling using TXT record responses | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_detect_suspicious_dns_txt_records_using_pretrained_model_in_dsdl.mlmodel) |
    | Detect suspicious process names | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_detect_suspicious_processnames_using_pretrained_model_in_dsdl.mlmodel) |
    | Detect DNS Data Exfiltration | [.mlmodel](https://github.com/splunk/security_content/blob/develop/lookups/__mlspl_detect_dns_data_exfiltration_using_pretrained_model_in_dsdl.mlmodel) |

### Deploy the model artifacts

1. Login into the Splunk instance and launch the Splunk App for Data Science and Deep Learning (DSDL).
2. Select Containers from the drop-down menu and it should list all the containers.
3. Select Container Image as Golden image CPU 5.0.0 and Cluster target as per env setup and start the dev container.
4. Wait for the container to start up and urls to populate for the container.
5. Login into the Jupyter lab of dev container by clicking on the url, ex: http://{container_url}:port_num/lab?
    * Use the password provided in the Overview > User Guide of DSDL app
6. The below steps are performed within the Jupyter Lab of the container.
    * Upload the pre-trained model .tar.gz file into app/model/data path using the upload option in the Jupyter notebook.
    * Open a terminal on Jupyterlab and execute the following commands

         ```bash
          tar -xf app/model/data/<pretrained_model_file_name>.tar.gz -C app/model/data/<pretrained_model_file_name>/
         ```

      This will extract the artifact `<pretrained_model_file_name>.tar.gz` into `app/model/data/<pretrained_model_file_name>/`
    * Upload .ipynb notebook into notebooks folder using the upload option in Jupyter lab.
    * Also, upload .json model configuration into notebooks/data folder.
    * Save the notebook using the save option in Jupyter notebook.

7. Start the container specific to the detection.

## 7-Code-of-Conduct

### Our Code of Conduct

#### Our Pledge

In the interest of fostering an open and welcoming environment, we as
contributors and maintainers pledge to making participation in our project and
our community a harassment-free experience for everyone, regardless of age, body
size, disability, ethnicity, gender identity and expression, level of experience,
nationality, personal appearance, race, religion, or sexual identity and
orientation.

#### Our Standards

Examples of behavior that contributes to creating a positive environment
include:

* Using welcoming and inclusive language
* Being respectful of differing viewpoints and experiences
* Gracefully accepting constructive criticism
* Focusing on what is best for the community
* Showing empathy towards other community members

Examples of unacceptable behavior by participants include:

* The use of sexualized language or imagery and unwelcome sexual attention or
advances
* Trolling, insulting/derogatory comments, and personal or political attacks
* Public or private harassment
* Publishing others' private information, such as a physical or electronic address, without explicit permission
* Other conduct which could reasonably be considered inappropriate in a professional setting

#### Our Responsibilities

Project maintainers are responsible for clarifying the standards of acceptable
behavior and are expected to take appropriate and fair corrective action in
response to any instances of unacceptable behavior.

Project maintainers have the right and responsibility to remove, edit, or
reject comments, commits, code, wiki edits, issues, and other contributions
that are not aligned to this Code of Conduct, or to ban temporarily or
permanently any contributor for other behaviors that they deem inappropriate,
threatening, offensive, or harmful.

#### Scope

This Code of Conduct applies both within project spaces and in public spaces
when an individual is representing the project or its community. Examples of
representing a project or community include using an official project e-mail
address, posting via an official social media account, or acting as an appointed
representative at an online or offline event. Representation of a project may be
further defined and clarified by project maintainers.

#### Enforcement

Instances of abusive, harassing, or otherwise unacceptable behavior may be
reported by contacting the project team at <research@splunk.com>. All
complaints will be reviewed and investigated and will result in a response that
is deemed necessary and appropriate to the circumstances. The project team is
obligated to maintain confidentiality with regard to the reporter of an incident.
Further details of specific enforcement policies may be posted separately.

Project maintainers who do not follow or enforce the Code of Conduct in good
faith may face temporary or permanent repercussions as determined by other
members of the project's leadership.

#### Attribution

This Code of Conduct is adapted from the [Contributor Covenant](https://www.contributor-covenant.org/), version 1.4,
available at <http://contributor-covenant.org/version/1/4>
