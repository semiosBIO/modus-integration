# Integration Requirements

In order to achieve a successful integration with Agworld, we need the following information from your lab.

## 1. Identification of the Modus Test Ids you'll use
### Detail
For every lab result you provide, a Modus Test Id needs to be provided with that result as specified in the Excel files located in the [modus documentation](https://github.com/AgGateway/Modus/tree/main/Method%20Lists/Modus%201). (e.g. "Soil Analysis Nomenclature.xlsx” for soil tests). To onboard with Agworld, you need to consider the tests your lab performs (the elements tested, the extraction methods, chemistry), and which Modus Test Ids they correspond to. Send us that list of Modus Test Ids so we can verify they match the latest version of the Modus standard.

### Purpose
To ensure that when you provide us a lab result, we know the exact element, extraction method, and chemistry used.

## 2. Lab Tests Offering (for users to choose)
### Detail
A CSV file listing all lab tests (packages/suites and individual tests) that are offered by your lab.
For each test, provide:
2.1) The test code. This will be the code provided to the lab, c.f: ExampleSampleSubmissionData_v3.csv. e.g. "S3"
2.2) The test name. This will be made selectable in Agworld. e.g. "Standard Package 3 - S3"
2.3) A more detailed description of that test. e.g. "Package includes: Sulfur, Zinc, Manganese, Iron, Copper, Boron"
2.4) The type of test that it is - e.g. "Soil" or "Plant" etc

### Purpose
This list will enable us to make those tests selectable within the Agworld interface by your customers.
The test ids/codes will populate the “test suites” column in [ExampleSampleSubmissionData_v3.csv](https://github.com/semiosBIO/modus-integration/blob/main/example_submission.csv)
(i.e. we’ll put the id/code in there based on the user's selection. This can have multiple values which will be semi-colon separated.)

There is a drop down list in Agworld that users can select from to pick the test suite(s) or individual test(s) that they want performed by you: TODO - add image
How this dropdown relates to this step - we need a list of information to build this drop down list.
Here's a (tiny) example:
TODO - add image

In the above example, the first three items are test suites, the last three are individual tests.

The purposes of the four columns are as follows:
Code is what you would like us to send to you in our CSV of sample data as the identifier of the test suite (or individual test) to perform, according to your business' own naming.
Display Name is the name of the test suite (or individual test) that you would like visible to the user in the drop down list as seen above.
Description is what is shown in the tooltip when the user hovers over the test name - which may describe (for example) the list of analyses performed as part of the test suite. For individual tests, it may only contain just the element name again.
Sample Type is the type of test that it is - e.g. "Soil" or "Plant" etc.

## 3. Example Lab Results XML File
### Details
An example lab results XML file with the format and data that you will be sending your lab results to us in (i.e. Modus format). c.f: [exampleMinimumModusOutput.xml](https://github.com/semiosBIO/modus-integration/blob/main/example_modus_result.xml)

### Purpose
This allows us to check that we will be able to automatically import the files you send to us.

## 4. Four email addresses
### Details
4.1) The "to" email address to send our sample submission information to (i.e. the ExampleSampleSubmissionData_v3.csv files),
so that when the physical samples arrive at the lab, you’ll have all the information required to process them.
4.2) The "from" email address that Agworld will receive the Modus XML lab results from (when you send Agworld the lab results).
4.3) The "to" email address to send emails to upon import failure of lab results into Agworld.
4.4) The "to" email address to send emails to upon import success of lab results into Agworld.

Note: We strongly recommend that these are group email addresses, as this will avoid problems if individual staff are unavailable due to leave or sickness.

### Purpose
To enable the sending/retrieving of samples/results, and error/success handling.

## Example files
Please make yourself familiar with the following files.

### 1. ExampleSampleSubmissionData_v3.csv
The file can be found here: [ExampleSampleSubmissionData_v3.csv](https://github.com/semiosBIO/modus-integration/blob/main/example_submission.csv)

Description: The samples submission file we send to you.

Details:
When sample collectors have finished their sample collecting, using our iPad’s Sampling App, the iPad will automatically send your lab an email containing an attachment like this CSV file.
It contains all the information you should require to be able to identify each sample, know what tests to run, who to bill and contact, and where to send the results (our automated email inbox).
The “sample barcode” column contains the barcode that will match the barcode on the individual bags. Or if barcodes are not used, just a unique id that will be labelled on the bag.
The “sample unique id” is the id that Agworld requires to identify the results for each sample when you email them to Agworld's lab results email address (our automated email inbox).
This "sample unique id" belongs in the <FMISSampleID> tag when sending Agworld the results.

### 2. exampleMinimumModusOutput.xml
The file can be found here: [exampleMinimumModusOutput.xml](https://github.com/semiosBIO/modus-integration/blob/main/example_modus_result.xml)

Description: The lab results file you send back to us (Maximum XML File Size: 2MB).

Details:
This is an example of a Modus xml file that you will need to send to Agworld.
The data inside it corresponds to the results for the first 2 samples we provided in [ExampleSampleSubmissionData_v3.csv](https://github.com/semiosBIO/modus-integration/blob/main/example_submission.csv).
There are many more fields that can be populated in the MODUS xml, although this is what we consider a minimum.
(i.e. we don’t require EventCodes, DepthRefs etc, although they can be provided).
Note that in Production, you cannot send XML files to Agworld that are larger than 2MB. Most labs send results for 1 field / sample job at a time.

## Read more
More detailed information on the integration process can be found in the main [README](https://github.com/semiosBIO/modus-integration/blob/main/README.md) of this repo.