The code in security content is using the following concepts:
- Clean Architecture
- Software Engineering Design Patterns
- Test Driven Development

Concepts from the following books about clean architecture are followed to structure the code:
- [Clean Architecture: A Craftsman’s Guide to Software Structure and Design](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)
- [Implementing the Clean Architecture](https://leanpub.com/implementing-the-clean-architecture)

# Clean Architecture
![](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

The Clean Architecture provides the following advantages:
- Independent of Frameworks
- Testability
- Independent of Database
- Independent of any external agency

An important concept of Clean Architecture is the dependency rule. This rule says that source code dependencies can only point inwards. Nothing in an inner circle can know anything at all about something in an outer circle. In particular, the name of something declared in an outer circle must not be mentioned by the code in the an inner circle. That includes, functions, classes. variables, or any other named software entity. This leads to that changes in code of an outer circle has no impact on the code of an inner circle.

[More information](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

Based on this guidelines, the security content project is structured as following under bin/contentctl_project:
* contentctl_core: Contains the two inner circles consisting of Domain and Application. Domain consists of Entities which are objects like detections, stories and so on. The application layer consists of Interface Adapter, Interface Builder, Factories and Use Cases.
* contentctl_infrastructure: Contains the third inner circle and contains the Implementation of Adapter, Builder and Database specific writer and reader classes.



# Software Engineering Design Patterns
The following software design patterns are used:
- [Builder](https://sourcemaking.com/design_patterns/builder)
- [Factory](https://sourcemaking.com/design_patterns/factory_method)
- [Adapter](https://sourcemaking.com/design_patterns/adapter)

## Builder

### Problem
An application needs to create the elements of a complex aggregate. The specification for the aggregate exists on secondary storage and one of many representations needs to be built in primary storage.

### Intent
- Separate the construction of a complex object from its representation so that the same construction process can create different representations.
- Parse a complex representation, create one of several targets.

### Implementation in Security Content
In security content the software design pattern builder is used to build the different security content object such as detections.
In this software design pattern a director is used to execute the different steps needed to build a detection. 
Example from bin/contentctl/contentctl_infrastructure/builder/security_content_director.py
````
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

## Factory

### Problem
A framework needs to standardize the architectural model for a range of applications, but allow for individual applications to define their own domain objects and provide for their instantiation.

### Intent
- Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.
- Defining a "virtual" constructor.
- The new operator considered harmful.

### Implementation in Security Content
In security content the software design pattern factory is used to iterate through the different security content objects. Subsequently, the factory is executing the director for every security content object, which then executes the corresponding builder.
Example from bin/contentctl/contentctl_core/application/factory/factory.py
````
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

## Adapter

### Problem
An "off the shelf" component offers compelling functionality that you would like to reuse, but its "view of the world" is not compatible with the philosophy and architecture of the system currently being developed.

### Intent
- Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
- Wrap an existing class with a new interface.
- Impedance match an old component to a new system

### Implementation in Security Content
The software design pattern Adapter is used to write the data objects to disk. Based on the different use cases in security content, there exists different Adapter based on the output format such as ObjToConfAdapter, ObjToJsonAdapter, ObjToYmlAdapter, ObjToMdAdapter, ...
Example from bin/contentctl/contentctl_infrastructure/adapter/obj_to_conf_adapter.py
````
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

# Test Driven Development
Test Driven Development (TDD) is software development approach in which test cases are developed to specify and validate what the code will do. In simple terms, test cases for each functionality are created and tested first and if the test fails then the new code is written in order to pass the test and making code simple and bug-free. Test-Driven Development starts with designing and developing tests for every small functionality of an application. 

For test driven development in security content, pytest is used. Pytest will test every python file which starts with test_*. Inside the python file, it will run every method which starts with test_*. Every class in security content should have a corresponding test file. Pytest can be use in the following way:
````
export PYTHONPATH="/path/to/security_content/"
pytest -s bin/contentctl_project 
````

### Implementation in Security Content
Every class in security content contains a corresponding test file, e.g.:
bin/contentctl/contentctl_infrastructure/builder/cve_enrichment.py
````
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
````
from bin.contentctl_project.contentctl_infrastructure.builder.cve_enrichment import CveEnrichment

def test_cve_enrichment():
    cve_enrichment = CveEnrichment.enrich_cve('CVE-2021-34527')
    assert cve_enrichment['id'] == 'CVE-2021-34527'
    assert cve_enrichment['cvss'] == 9.0
    assert cve_enrichment['summary'] == 'Windows Print Spooler Remote Code Execution Vulnerability'
````

[More information](https://www.guru99.com/test-driven-development.html)