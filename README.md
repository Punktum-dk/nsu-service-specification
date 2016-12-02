# NSU Service and Protocol 1.0 Specification

<!-- MarkdownTOC depth=3 -->

- [Introduction](#introduction)
- [About this Document](#about-this-document)
    - [License](#license)
    - [Document History](#document-history)
- [The .dk Registry in Brief](#the-dk-registry-in-brief)
- [The NS Update Service](#the-ns-update-service)
- [Resources](#resources)
    - [Mailing list](#mailing-list)
    - [Issue Reporting](#issue-reporting)
- [Appendices](#appendices)
    - [Error Messages](#error-messages)
        - [Form Validation Errors](#form-validation-errors)
        - [Status Resolution Errors](#status-resolution-errors)
        - [DNS Resolution Errors](#dns-resolution-errors)

<!-- /MarkdownTOC -->

<a name="introduction"></a>
# Introduction

NSU is short for NS Update. NSU is a propriety protocol and service developed and offered by DK Hostmaster's as an interface for updating the relationship between nameservers and .dk domainnames. 

The protocol is based on HTTP og the parameters are transferred as GET-variables. The response contains an HTTP header and a brief message for human interpretation.

To use NSU, send an SSL-encrypted HTTP GET request to the following address:

```
https://ns-update.dk-hostmaster.dk/
```

<a name="about-this-document"></a>
# About this Document

This specification describes the current implementation.

Printable version can be obtained via [this link](https://gitprint.com/DK-Hostmaster/nsu-service-specification/blob/master/README.md), using the gitprint service.

<a name="license"></a>
## License

This document is copyright by DK Hostmaster A/S and is licensed under the MIT License, please see the separate LICENSE file for details.

<a name="document-history"></a>
## Document History

* 1.0 2015-07-04
  * Initial revision on Github

<a name="the-dk-registry-in-brief"></a>
# The .dk Registry in Brief

DK Hostmaster is the registry for the ccTLD for Denmark (dk). The current model used in Denmark is based on a sole registry, with DK Hostmaster maintaining the central DNS registry.

The service is not subject to any sorts of standards.

<a name="the-ns-update-service"></a>
# The NS Update Service

The NSU service offers a single capability, handling of redelegation requests.

![diagram of anonymous redelegation proces][nsu_anonymous_redelegation]

A request to the service is validated as follows:

1 Parameters are syntactically validated (See: [Form Validation Errors](#form-validation-errors))
1 Domain name and nameserver are semantically validated (See: [Status Resolution Errors](#status-resolution-errors))
1 Domain name is resolved towards specified nameserver (See: [DNS Resolution Errors](#dns-resolution-errors))

If the request passes all tests a request is sent to the registrant of the domain name for approval.

<a name="resources"></a>
# Resources

Resources for DK Hostmaster NSU are listed below.

<a name="mailing-list"></a>
## Mailing list

DK Hostmaster operates a mailing list for discussion and inquiries  about the DK Hostmaster NSU service and technical issues in general. To subscribe to this list, write to the address below and follow the instructions. Please note that the list is for technical discussion only, any issues beyond the technical scope will not be responded to, please send these to the contact issue reporting address below and they will be passed on to the appropriate entities within DK Hostmaster A/S.

* `tech-discuss+subscribe@liste.dk-hostmaster.dk`

<a name="issue-reporting"></a>
## Issue Reporting

For issue reporting related to this specification, the DSU implementation or sandbox or production environments, please contact us. You are of course welcome to post these to the mailing list mentioned above, otherwise use the address specified below:

 * `info@dk-hostmaster.dk`

<a name="appendices"></a>
# Appendices

<a name="error-messages"></a>
## Error Messages

<a name="form-validation-errors"></a>
### Form Validation Errors

`[$field] field not specified`

<a name="status-resolution-errors"></a>
### Status Resolution Errors

`Domain name is not in a state eligible for redelegation`

<a name="dns-resolution-errors"></a>
### DNS Resolution Errors

`Failed to query nameservers, timed out`

`Unable to resolve nameserver`

`No nameservers supplied`

`No nameserver host info`

`No NS info on host`

`Too few hosts`

`Too many hosts`

`Host is missing NS record when compared to other responses`

`SOA Serial mismatch between responses`

`No nameservers found`

`Host has alternating SOA serial between queries`

`IP address missing in nameserver IP address response`

`Nameserver response contains non-public IP address`

`Failed to query nameservers`

`Resolved nameserver not registered with DK Hostmaster A/S`

[nsu_anonymous_redelegation]: https://raw.githubusercontent.com/DK-Hostmaster/nsu-service-specification/master/images/nsu_anonymous_redelegation_1.0.png
