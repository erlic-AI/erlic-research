# Data and Licensing Policy

## Rule

A repository's software license does not relicense training data. The model-weight license also requires a separate legal and policy review. Do not state that all training data is Apache-2.0 unless every upstream source and redistribution condition supports that statement.

## Required manifest fields

For each dataset/source, record:
- Source name and canonical URL.
- Dataset revision / snapshot date.
- Upstream license identifier and link.
- Terms of use and redistribution restrictions.
- Commercial-use status (allowed / restricted / unclear).
- Personal data and sensitive-data handling.
- Transformations applied.
- Number of documents and tokens retained.
- Deduplication and quality filtering method.
- Whether raw examples can be redistributed.

## Current source list requiring per-source review

The existing code references FineWeb-2 Turkish, Wikimedia Wikipedia Turkish, FineWeb-Edu, CodeParrot, The Stack Smol, OpenWebMath, Turkish instruction datasets, GSM8K and MBPP. This list is not a blanket approval for training or commercial use. Check each dataset card and each upstream code license before use.

## Release policy

- Publish source manifests and filtering code.
- Do not redistribute raw datasets unless permitted.
- Include attribution and share-alike obligations where applicable.
- Exclude sources with unclear or incompatible terms until reviewed.
- Document synthetic-data teacher models and their applicable terms.
- Publish a separate model card describing training data and limitations.
