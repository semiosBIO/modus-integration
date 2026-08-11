# Frequently Asked Questions

## Where in the Modus XML does the Unique Sample ID go?

The `FMISSampleID` node under the `SampleMetaData` node. This is a mandatory
field and your import will not work if this does not match an Agworld sample ID
which is expecting results from your lab.

See documentation in the Modus Schema
[FMISSampleID](https://github.com/aggateway/Modus/blob/7f041e6ed7685446e40214b5b9b39dfcf8f9ef3d/Schema/Modus%201/modus_global.xsd#L650)

See [example](example_modus_result.xml#L45).

## Where in the Modus XML does the Sample Job ID / Collection Job ID go?

The `EventCode` node under the `EventMetadata` node. This is an optional field
and is ignored by the importer but can be helpful to include in the XML for
debugging purposes.

See documentation in the Modus Schema
[EventCode](https://github.com/aggateway/Modus/blob/7f041e6ed7685446e40214b5b9b39dfcf8f9ef3d/Schema/Modus%201/modus_global.xsd#L914)

See [example](example_modus_result.xml#L18).

## Where in the Modus XML does the barcode go?

The `SampleNumber` node under the `SampleMetaData`. This is an optional field
and is ignored by the importer but can be helpful to include in the XML for
debugging purposes.

See documentation in the Modus Schema
[SampleNumber](https://github.com/aggateway/Modus/blob/7f041e6ed7685446e40214b5b9b39dfcf8f9ef3d/Schema/Modus%201/modus_global.xsd#L645)

See [example](example_modus_result.xml#L46).

## How can I resubmit results for a sample I already submitted results for?

By default Agworld will reject resubmissions to avoid accidental overwrites. To
force an overwrite the `OverwriteResult` node should be included asSampleNumbera sibling
to the `FMISSampleID` node.

See documentation in the Modus Schema
[OverwriteResult](https://github.com/aggateway/Modus/blob/7f041e6ed7685446e40214b5b9b39dfcf8f9ef3d/Schema/Modus%201/modus_global.xsd#L670)

See [example](example_modus_result.xml#L47).
