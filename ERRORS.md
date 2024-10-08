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
# Errors on result import

When a lab sends results to Agworld, there are some checks that take place to
determine whether the results are valid. If the results are invalid in some
way, an error will be generated and an error email will be sent back to the
lab.

These are the common errors, what they mean, and how they can be resolved.

## Unexpected Overwrite

Error text:
> Error importing the following sample unique ids because these sample results
> have already been imported, yet <OverwriteResult> is NOT set to "true".
> (Ensure you haven't repeated an 'FMISSampleID more than once) Sample unique
> ids were: {sample_ids}

This error occurs when there a FMISSampleID in the results XML matches a sample
in the Agworld system but that sample already has results attached to it. This
could occur if a lab submits results for the same sample twice, or if the
Agworld user submits results for the sample via the website before the lab
submits them.

This error can be resolved in the following way:
1. Double check that the results email is being sent to the correct email
   address in the correct region. Agworld reuses sample IDs across regions and
   so the lab could be accidentally trying to submit results for a Canadian
   sample to the US system or vice versa.
2. Either:
- If the existing results for the samples in Agworld are correct, remove those
  samples from the results XML being submitted to Agworld and resubmit.
- If the existing results for the samples in Agworld are not correct or need to
  be updated, use the <OverwriteResult> tag to force Agworld to accept the
  newer results. This must be added as a sibling node to the <FMISSampleID>
  tag.

## No XML attached

Error text:
> Expecting XML email attachements for import, however none were attached

This error occurs when an email is sent to Agworld which has no results XML
file attached to it.

This error can be resolved by correctly attaching a results XML to the email.

## Invalid units

Error text:
> Error importing sample with 'sample unique id' of {sample_id} (from
> <FMISSampleID>{sample_id}</FMISSampleID>) - Couldn't find Analyte match for
> element {element} with test id '{modus_id}', modus units '{units}'. Valid
> units include: {valid_units}

This error occurs when the results XML uses a unit for a Modus ID which is not
included in the declared nomenclature of the Modus standard.

This error can be resolved in one of a few ways:
1. Change the unit being used for the Modus ID to match the Modus standard.
2. Change the Modus ID being used to one for which the unit is valid.
3. Remove the analyte from the results XML and submit without it.
4. Update the Modus standard itself by submitting an issue or pull request to
   their [public repo](https://github.com/AgGateway/Modus)

## Sample not expecting results from lab

Error text:
> Error message: Error importing sample with 'sample unique id' of {sample_id}
> (from <FMISSampleID>{sample_id}</FMISSampleID>) - Sample is not expecting
> results from lab: {lab_name}

This error occurs when a sample ID exists in Agworld but is not in a state
where it is accepting results from the given lab. This could be because the
sample is has not been submitted to the lab via Agworld, or because it has been
submitted to a different lab, or because the sample is otherwise not in a valid
state.

This error can be resolved by following this approach:
1. Double check that the results email is being sent to the correct email
   address in the correct region. Agworld reuses sample IDs across regions and
   so the lab could be accidentally trying to submit results for a Canadian
   sample to the US system or vice versa.
2. Double check the state of the relevant collection job with the appropriate
   Agworld user. If it is not in a suitable state, this should be corrected
   before results can be accepted. The suitable states are `Submitted to lab`
   and `Partial results`.
3. Check that the FMISSampleIDs being used match up to the appropriate Samples
   in the Agworld system. This also requires coordination between the lab and
   the Agworld user, although Agworld Customer Success may also be able to
   assist with this.

## Sample Not found

Error text:
> Error importing sample unique id {bad_id}: Sample could not be found

This error occurs when the results contain an FMISSampleID which does not match
a sample in Agworld.

This error can be resolved by the following process:
1. Check the bad sample ID in the error. It could be a typo or transcription
   error. For example if it contains non-numeric characters this is not an
   Agworld sample ID.
2. Check with the relevant Agworld user to confirm what the correct sample IDs
   are for the collection job in question.

## XML contains no results

Error text
> There are no sample results in this XML file in the expected path:
> 'ModusResult/Event/EventSamples/Soil/SoilSample'

This error occurs either when the Modus schema is not correctly adhered to when
generating the results XML or if there are no sample results in the XML.

This error can be resolved by adhering to the Modus schema and including sample
results in the XML in the expected way.

## Unknown Modus ID

Error text
> Cannot find Modus test with id '{modus_id}' and units '{units}'

This error occurs when a Modus test ID in the results XML does not exist in the
Modus standard.

This error can be resolved in the following way:
1. Check for typos or transcription errors in the Modus ID.
2. Check that the Modus ID is a valid Modus ID and not a test ID from another
   standard.
3. Check that the Modus ID is a Soil type Modus ID. Agworld does not currently
   support other sample types such as Plant.

## Sending results from wrong email

Error text
> Email '{email_address}' not registered with Agworld. Lab import failed.

This error occurs when the incorrect email address is used to submit results to
Agworld.

This error can be resolved by the lab using the email address that is
registered with Agworld. In the case of labs which have their own domain name,
it is possible to register the whole domain with Agworld which allows any email
address at that domain to submit results to Agworld for that lab.

## Unknown Error

Error text
> Unexpected error during lab results import. Error code: {uuid}

Occassionally an error occurs which is novel. Every instance of an error like
this is assigned a unique Universally Unique ID (uuid) so that Agworld can
track this error in our observability systems to determine what happened.

This error should be reported to Agworld support, including the uuid,
preferably in text format so support staff can easily search for it in the
logs.
