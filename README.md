# NSU Service and Protocol 1.0 Specification

![GitHub Workflow build status badge markdownlint](https://github.com/Punktum-dk/nsu-service-specification/workflows/Markdownlint%20Workflow/badge.svg)

2021-09-30
Revision: 2.1

# Table of Contents

<!-- MarkdownTOC bracket=round levels="1,2,3,4" indent="  " autolink="true" autoanchor="true" -->

- [Introduction](#introduction)
- [About this Document](#about-this-document)
  - [License](#license)
  - [Document History](#document-history)
- [The .dk Registry in Brief](#the-dk-registry-in-brief)
- [The NS Update Service](#the-ns-update-service)
  - [Examples](#examples)
    - [Using `curl`](#using-curl)
    - [Using `httpie`](#using-httpie)
- [Resources](#resources)
  - [Public Service Page](#public-service-page)
  - [Mailing list](#mailing-list)
  - [Issue Reporting](#issue-reporting)
- [Appendices](#appendices)
  - [Error Messages](#error-messages)
    - [Parameter Validation Errors](#parameter-validation-errors)
    - [Semantical Validation Errors](#semantical-validation-errors)
    - [DNS Resolution Errors](#dns-resolution-errors)

<!-- /MarkdownTOC -->

<a id="introduction"></a>
# Introduction

NSU is short for NS Update. NSU is a propriety protocol and service developed and offered by Punktum dk's as an interface for updating the relationship between nameservers and .dk domainnames.

The protocol is based on HTTP og the parameters are transferred as GET-variables. The response contains an HTTP header and a brief message for human interpretation.

To use NSU, send an SSL-encrypted HTTP GET request to the following address:

- `https://ns-update.dk-hostmaster.dk/`

<a id="about-this-document"></a>
# About this Document

This specification describes the current implementation.

Printable version can be obtained via [this link](https://gitprint.com/Punktum-dk/nsu-service-specification/blob/master/README.md), using the gitprint service.

<a id="license"></a>
## License

This document is copyright by Punktum dk A/S and is licensed under the MIT License, please see the separate LICENSE file for details.

<a id="document-history"></a>
## Document History

- 2.1 2021-09-30
  - Specified section on the execution and the deletion of DSRECORDs

- 2.0 2021-09-17
  - Added two new error scenarios for:
    - Domain names under registrar management
    - Domain names with VID
    - Change of name servers cannot be initiated from the NSU service with the new change of name servers process
  - These new error scenarios is introduced in NSU service version 2.0.0

- 1.0 2015-07-04
  - Initial revision on GitHub

<a id="the-dk-registry-in-brief"></a>
# The .dk Registry in Brief

Punktum dk is the registry for the ccTLD for Denmark (dk). The current model used in Denmark is based on a sole registry, with Punktum dk maintaining the central DNS registry.

The service is not subject to any sorts of standards.

<a id="the-ns-update-service"></a>
# The NS Update Service

The NSU service offers a single capability, handling of redelegation requests. The service supports danish and english.

![diagram of anonymous redelegation proces][nsu_anonymous_redelegation]

A request to the service is validated as follows:

1. Parameters are syntactically validated (See: [Parameter Validation Errors](#parameter-validation-errors))
1. Domain name and nameserver fields are semantically validated (See: [Semantical Validation Errors](#semantical-validation-errors))
1. Domain name is resolved towards specified nameserver (See: [DNS Resolution Errors](#dns-resolution-errors))

If the request passes all tests a request is sent to the registrant of the domain name for approval.

The following text string is returned if the request was successfully created:

- English: `Request submitted`
- Danish: `Redelegeringsanmodning afsendt`

The request requires the following fields:

- `id` - has to hold the value of either: `52` (danish) or `250` (english)
- `domain` - has to hold a domain name registered with Punktum dk, the domain name cannot be in a state of: `deactivated`
- `nameserver` - has to hold a host name registered with Punktum dk and the nameserver has to be active
- `redel` - has to hold the value `true`
- `submit` - has to hold the value of either: `bekræft` (danish) or `confirm` (english)

Some of the fields are insignificant for the actual proces, but are required to ensure backwards compability.

In addition to the above error categories the service can render a fatal error, this error occurs only if key resources like database access etc. are not available or other circumstances rendering the service is a critical state.

When the registrant or proxy approves the request the change is completed. Any possible DSRECORDs will be deleted as part of the change.

<a id="examples"></a>
## Examples

A populated URL:

`https://ns-update.dk-hostmaster.dk/english/self-service/redelegate-domain-name/?id=250&redel=true&domain=eksempel.dk&nameserver=ns1.hostname.com&confirm=true`

The pages returned are fully rendered HTML pages for human interpretation.

<a id="using-curl"></a>
### Using `curl`

```bash
$ curl --get https://ns-update.dk-hostmaster.dk/en/ -d id=250 -d redel=true -d domain=eksempel.dk -d nameserver=ns1.hostname.com -d confirm=true -d submit=confirm
```

<a id="using-httpie"></a>
### Using `httpie`

```bash
$ http "https://ns-update.dk-hostmaster.dk/en/?id=250&redel=true&domain=eksempel.dk&nameserver=ns1.hostname.com&confirm=true"
```

<a id="resources"></a>
# Resources

Resources for the Punktum dk NSU services are listed below.

<a id="issue-reporting"></a>
## Issue Reporting

For issue reporting related to this specification, the DSU implementation or sandbox or production environments, please contact us.

<a id="appendices"></a>
# Appendices

<a id="error-messages"></a>
## Error Messages

As described earlier in this document the service can render a fatal error, this error occurs only if key resources like database access etc. are not available or other circumstances rendering the service is a critical state, this category of errors are not listed below.

<a id="parameter-validation-errors"></a>
### Parameter Validation Errors

These errors can occur when the submitted data are initially validated

- English: `id field is required`
- Danish: `id felt er påkrævet`

Please see the required value earlier in this document.

- English: `confirm field is required`
- Danish: `confirm felt er påkrævet`

Please see the required value earlier in this document.

- English: `redel field is required`
- Danish: `redel felt er påkrævet`

Please see the required value earlier in this document.

- English: `domain field is required`
- Danish: `domain felt er påkrævet`

Please see the required value earlier in this document.

- English: `nameserver field is required`
- Danish: `nameserver felt er påkrævet`

Please see the required value earlier in this document.

- English: `submit field is required`
- Danish: `submit felt er påkrævet`

Please see the required value earlier in this document.

<a id="semantical-validation-errors"></a>
### Semantical Validation Errors

These errors can occur when the specified domain name and nameserver are validated towards the registry.

- English: `Provided domain name is not registered with Punktum dk`
- Danish: `Angivet domæne navn er ikke registreret hos Punktum dk`

This error occurs if the specified domain name is not registered with Punktum dk, please register the domain name via a registrar or specify a registered domain name for which the operation is relevant.

- English: `Provided nameserver is not registered with Punktum dk`
- Danish: `Angivet navneserver er ikke registreret hos Punktum dk`

This error occurs if the specified host is not registered with Punktum dk, either register the host using our self-service platform or EPP service or specify a registered nameserver for which the operation is relevant.

- English: `Domain name is not in a state eligible for redelegation`
- Danish: `Domænenavn er ikke i en tilstand som kan redelegeres`

This error occurs if a domain is the state: `deactivated`

- English: `Provided domain name can not be a VID domain`
- Danish: `Domænet må ikke være et VID domæne`

This error is emmitted in case the domain name is a VID domain name, meaning the service only works for registrant managed regular domain names.

Name server changes for VID domains has to use a name server change process implemented in a service which support authentication, like SB supplemented with EPP and RP.

- English: `Provided domain name may not be registrar handled`
- Danish: `Domænet må ikke være forhandler håndteret`

Domain names under registrar management can only have their names services changed via a service which support authentication, like SB supplemented with EPP and RP.

<a id="dns-resolution-errors"></a>
### DNS Resolution Errors

These error can occur when the specified domain name and nameserver are attempted resolved using DNS.

- English: `Failed to query nameservers, timed out`
- Danish: `Forespørgsel mod navneservere fejlede, tidsfrist udløbet`

- English: `Unable to resolve nameserver`
- Danish: `Ikke muligt at slå nameserver op`

- English: `No nameservers supplied`
- Danish: `Ingen nameservers angivet`

- English: `No nameserver host info`
- Danish: `Ingen nameserver værts info`

- English: `No NS info on host`
- Danish: `Ingen NS info på vært`

- English: `Too few hosts`
- Danish: `For få værter`

- English: `Too many hosts`
- Danish: `For mange værter`

- English: `Host is missing NS record when compared to other responses`
- Danish: `Vært mangler NS optegnelse når sammenlignet med andre svar`

- English: `Host has extra NS record when compared to other responses`
- Danish: `Vært har ekstra NS optegnelse når sammenlignet med andre svar`

- English: `SOA Serial mismatch between responses`
- Danish: `SOA serienummer misforhold mellem svar`

- English: `No nameservers found`
- Danish: `Ingen navne servere fundet`

- English: `Host has alternating SOA serial between queries`
- Danish: `Vært har skiftedne SOA serienummer mellem forespørgsler`

- English: `IP address missing in nameserver IP address response`
- Danish: `IP adresse mangler i nameserver IP adresse svar`

- English: `Nameserver response contains non-public IP address`
- Danish: `Nameserver svar indeholder ikke-offentlig IP adresse`

- English: `Failed to query nameservers`
- Danish: `Fejlede forespørgsel mod navneservere`

- English: `Resolved nameserver not registered with Punktum dk`
- Danish: `Lokaliseret navneserver ikke registreret hos Punktum dk`

[nsu_anonymous_redelegation]: https://raw.githubusercontent.com/Punktum-dk/nsu-service-specification/master/images/nsu_anonymous_redelegation_1.0.png
