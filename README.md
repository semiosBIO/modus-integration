<!--
   Copyright Almanac (Semios, AgWorld)
   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
-->
# Lab Integration with Agworld
Documentation for the modus lab integration in Agworld

## Why Integrate with Agworld
Agworld has brought a world-first platform to the fore, making precision agronomy a simple set of steps to follow on 1 platform, rather than a complicated set of decisions and coordinations required between:
- Growers
- Consultants
- Third-party sample collectors
- Labs
- Precision Agronomists
- Applicators

Labs that integrate with Agworld are joining a fast growing platform of connected farmers and agronomists who want the easy ability to perform precision tasks. A successful lab integration will:
1. Gain the lab access to Agworld's growing platform of users wanting to perform precision.
2. Allow them to receive lab requests in a consistent, automated, and paperless process that is scalable and easily repeatable.
3. Through adopting the open [Modus standard](https://github.com/AgGateway/Modus), tasks such as adding/changing test packages, extraction & measurement methods, chemistry, units etc becomes a simple task that won't affect any integration with FMIS', other labs, or universities.

## How to Integrate with Agworld
### High Level Lab Workflow
The main steps in the Agworld workflow that are relevant to labs are:

1. An Agworld agronomist or third-party collector will use our Sample Collection iPad App to collect samples in their client's fields, either with barcoded bags, or a handwritten sample id system. (The iPad facilitates barcode/id scans using its camera, or manual id input as necessary).
2. Upon collection completion, the iPad will send an email to the lab notifying them of an incoming lab samples submission with a CSV attachment containing detailed information about them. This will include sample barcodes/ids, the lab test suites/packages to run, the date sampled, billing, collector and grower information, and an email address to send lab results to.
3. The lab will process the samples in the following way:
- The lab will receive the physical samples, and will perform a match up with the CSV attachment and the barcodes or sequence numbers on the bags.
- The samples will be processed.
- The results will be emailed to Agworld in Modus XML format.
4. Agworld will receive the results, match the results based on the "sample unique id" provided in the samples submission CSV, and begin performing precision analysis on the results.

### Input data that the Lab Receives from Agworld (Sample Information)
When Agworld sample collectors have finished their sample collecting using our iPad’s Sampling App, the iPad will automatically send the lab an email containing an attachment similar to this [CSV file](/example_submission.csv). It contains all the information the lab should require to be able to identify each sample, know what tests to run, who to bill and contact, and where to send the results (our automated email inbox).
The “sample barcode” column will contain either a barcode or a sequence number (depending on what method the sample collectors have selected) that will match value on the individual bags. 
The “sample unique id” is the id that Agworld requires to identify the results for each sample when you email them to our automated email inbox.

### Output data that the Lab Emails to Agworld (Lab results)
#### Modus
Agworld implements the Open [Modus standard](https://github.com/AgGateway/Modus). It has gained a lot of support in recent years and is a great way of having a common approach and format for communications between labs, universities and farm management information systems (FMIS).
[Method Lists](https://github.com/AgGateway/Modus/tree/main/Method%20Lists/Modus%201)   (describes all of the unique tests, extraction methods, elements and units that are available)
[XML Schema](https://github.com/AgGateway/Modus/blob/main/Schema/Modus%201/modus_result.xsd)   (describes the XML format definition)

Since Agworld is integrating with dozens of different labs, Modus allows us to receive results from labs in a standard format. Should any lab add new test suites/packages, extraction methods, units, and elements to their offering, we have a standard way of interpreting the new results without code change, thus being more responsive to lab changes. It also helps integrations with other third party consumers of this information, such as PCT Agcloud which facilitates the generation of interpolated surfaces / layers.

For labs, it means adopting a standard that is going to be compatible with more FMIS’ in future, and can expect a quick turn-around time when changing test suites, extraction methods, chemistry, units, etc.

#### Example Output
The links above provide XML Schema Definitions (XSDs) which clearly state the acceptable structure and parameters for Modus XML Lab Results. However, here is an [example Modus XML file](/example_modus_result.xml) that would correspond to the results for the first 2 samples we provided in the [example CSV file](/example_submission.csv) that the iPad Sampling App sends. There are many more fields that can be populated in the Modus XML, although this is what we consider a minimum. That is, we don’t require elements like EventCodes, DepthRefs and the like, although they can be provided. Note that in Production, you cannot send XML files to Agworld that are larger than 2MB. Most labs choose to send results for 1 field / sample job at a time.

### What Agworld needs for a Complete Integration
1. The ability of the lab to send results in Modus XML format to our automated lab inbox. The address of this inbox varies by region and the latest email address is provided in each submission CSV sent to the lab.
2. The lab to consider the sample tests it performs, and identify a list of ModusTestIds that correspond to these tests, as specified in the Excel files located here: [Method Lists](https://github.com/AgGateway/Modus/tree/main/Method%20Lists/Modus%201) (e.g. "Soil Analysis Nomenclature.xlsx” for soil tests).
3. A list of all lab tests (packages/suites and individual tests) that are offered by the lab to its customers. Specifically, these 4 pieces of information for each test:
- The test name. This will be made selectable in the Agworld website. e.g. "Standard Package 3 - S3"
- The test id/code. This will be the code provided to the lab. e.g. "S3"
- A description of that test that will be shown in mouseover on the Agworld website - e.g. “Package includes: Sulfur, Zinc, Manganese, Iron, Copper, Boron"
- The type of test that it is - e.g. "Soil" or "Plant" etc (Agworld currently only supports Soil tests)
This list will enable us to make those tests selectable within the Agworld interface by your customers. This will then populate the “test suite” column in the example CSV file. i.e. we will fill in the test id/code there.
4. Four email addresses:
- The "to" email address that Agworld will send the sample submission information to (i.e. the CSV files), so that when the physical samples arrive at the lab, the lab will have all the information required to process them.
- The "from" email address that Agworld will receive the Modus XML lab results from (when the lab sends Agworld the lab results). This can also be a domain name to allow all emails at that domain to post results to Agworld.
- The "to" email address Agworld will send email reciepts to upon import failure of lab results into Agworld.
- The "to" email address Agworld will send email reciepts to upon import success of lab results into Agworld.
5. An example lab results XML file with the format and data that you would be sending to us (i.e. Modus format). This allows us to validate that we will be able to import the files you send to us.

More details can be found in [REQUIREMENTS](https://github.com/semiosBIO/modus-integration/blob/main/REQUIREMENTS.md).
