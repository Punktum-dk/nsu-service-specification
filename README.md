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
        - [Parameter Validation Errors](#parameter-validation-errors)
        - [Semantical Validation Errors](#semantical-validation-errors)
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

The NSU service offers a single capability, handling of redelegation requests. The service supports danish and english.

![diagram of anonymous redelegation proces][nsu_anonymous_redelegation]

A request to the service is validated as follows:

1. Parameters are syntactically validated (See: [Parameter Validation Errors](#parameter-validation-errors))
1. Domain name and nameserver fields are semantically validated (See: [Semantical Validation Errors](#semantical-validation-errors))
1. Domain name is resolved towards specified nameserver (See: [DNS Resolution Errors](#dns-resolution-errors))

If the request passes all tests a request is sent to the registrant of the domain name for approval.

The following text string is returned if the request was successfully created:

* English: `Request submitted`
* Danish: `Redelegeringsanmodning afsendt`

The request requires the following fields:

* `id` - has to hold the value of either: `52` (danish) or `250` (english)
* `domain` - has to hold a domain name registered with DK Hostmaster, the domain name cannot be in a state of: `deactivated`
* `nameserver` - has to hold a host name registered with DK Hostmaster and the nameserver has to be active
* `redel` - has to hold the value `true`
* `redel` - has to hold the value `true`
* `submit` - has to hold the value of either: `bekræft` (danish) or `confirm` (english)

Some of the fields are insignificant for the actual proces, but are required to ensure backwards compability.

In addition to the above error categories the service can render a fatal error, this error occurs only if key resources like database access etc. are not available or other circumstances rendering the service is a critical state.

<a name="resources"></a>
# Resources

Resources for the DK Hostmaster NSU services are listed below.

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

As described earlier in this document the service can render a fatal error, this error occurs only if key resources like database access etc. are not available or other circumstances rendering the service is a critical state, this category of errors are not listed below.

<a name="parameter-validation-errors"></a>
### Parameter Validation Errors

* English: `id field is required`
* Danish: `id felt er påkrævet`

Please see the required value earlier in this document.

* English: `confirm field is required`
* Danish: `confirm felt er påkrævet`

Please see the required value earlier in this document.

* English: `redel field is required`
* Danish: `redel felt er påkrævet`

Please see the required value earlier in this document.

* English: `domain field is required`
* Danish: `domain felt er påkrævet`

Please see the required value earlier in this document.

* English: `nameserver field is required`
* Danish: `nameserver felt er påkrævet`

Please see the required value earlier in this document.

* English: `submit field is required`
* Danish: `submit felt er påkrævet`

Please see the required value earlier in this document.

<a name="semantical-validation-errors"></a>
### Semantical Validation Errors

* English: `Provided domain name is not registered with DK Hostmaster`
* Danish: `Angivet domæne navn er ikke registreret hos DK Hostmaster`

This error occurs if the specified domain name is not registered with DK Hostmaster, please register the domain name via a registrar or specify a registered domain name for which the operation is relevant.

* English: `Provided nameserver is not registered with DK Hostmaster` 
* Danish: `Angivet navneserver er ikke registreret hos DK Hostmaster`

This error occurs if the specified host is not registered with DK Hostmaster, either register the host using our self-service platform or EPP service or specify a registered nameserver for which the operation is relevant.

* English: `Domain name is not in a state eligible for redelegation`
* Danish: `Domænenavn er ikke i en tilstand som kan redelegeres`

This error occurs if a domain is the state: `deactivated`

<a name="dns-resolution-errors"></a>
### DNS Resolution Errors

* English: `Failed to query nameservers, timed out`
* Danish: `Forespørgsel mod navneservere fejlede, tidsfrist udløbet`


* English: `Unable to resolve nameserver`
* Danish: `Ikke muligt at slå nameserver op`


* English: `No nameservers supplied`
* Danish: `Ingen nameservers angivet`


* English: `No nameserver host info`
* Danish: `Ingen nameserver værts info`


* English: `No NS info on host`
* Danish: `Ingen NS info på vært`


* English: `Too few hosts`
* Danish: `For få værter`


* English: `Too many hosts`
* Danish: `For mange værter`


* English: `Host is missing NS record when compared to other responses`
* Danish: `Vært mangler NS optegnelse når sammenlignet med andre svar`


* English: `Host has extra NS record when compared to other responses`
* Danish: `Vært har ekstra NS optegnelse når sammenlignet med andre svar`


* English: `SOA Serial mismatch between responses`
* Danish: `SOA serienummer misforhold mellem svar`


* English: `No nameservers found`
* Danish: `Ingen navne servere fundet`


* English: `Host has alternating SOA serial between queries`
* Danish: `Vært har skiftedne SOA serienummer mellem forespørgsler`


* English: `IP address missing in nameserver IP address response`
* Danish: `IP adresse mangler i nameserver IP adresse svar`


* English: `Nameserver response contains non-public IP address`
* Danish: `Nameserver svar indeholder ikke-offentlig IP adresse`


* English: `Failed to query nameservers`
* Danish: `Fejlede forespørgsel mod navneservere`


* English: `Resolved nameserver not registered with DK Hostmaster`
* Danish: `Lokaliseret navneserver ikke registreret hos DK Hostmaster`

[nsu_anonymous_redelegation]: https://raw.githubusercontent.com/DK-Hostmaster/nsu-service-specification/master/images/nsu_anonymous_redelegation_1.0.png
qui