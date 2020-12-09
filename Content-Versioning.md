# How releases are versioned

Each Splunk Security Content release follows a 3 number structure: **\<major\>.\<minor\>.\<patch\>** for example `3.9.1`. Below is a explanation on what each number signifies and when do they change. 

* **\<major\>** - this number pertains to the specification/schema version our content is adhering to, today we are in spec 3.0. This only changes when we make a schema change or update. 
* **\<minor\>** - this number pertains to the update we are on, specifically every time a new piece of content (detection, story, response, etc.) is introduced this number is increased.  
* **\<patch\>** - this number pertains to fixes for content, specifically every time we resolve a bug with a current piece of content but do not introduce any new functionality this number increases. 

We did not come up with this concept and are just implementing schematic versioning see https://semver.org/.
Note that release announcement are only sent out on minor changes and above and not usually for patches unless it is a critical issue that must be communicated. 