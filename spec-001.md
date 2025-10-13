# Infrastructure Naming Convention Spec

## Version

This is spec version 0.1.2.

## Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY" and "OPTIONAL" in the document are to be interpreted as described in [RFC 2119][rfc-2119] under [BCP 14][bcp-14].

The semantic versioning in the document is specified in [Semantic Versioning Specification][semver].

The key words "unspecified", "undefined", and "implementation-defined" are to be interpreted as described in the [rationale for the C99 standard][c99-std].

[rfc-2119]: https://www.rfc-editor.org/rfc/rfc2119.txt
[bcp-14]: https://www.rfc-editor.org/info/bcp14
[semver]: https://semver.org/
[c99-std]: https://www.open-std.org/jtc1/sc22/wg14/www/C99RationaleV5.10.pdf#page=18

## Summary

This document proposes naming conventions for infrastructure instances.

The infrastructure naming conventions define:
1. Series of naming rules for infrastructure instances.

## Section 1: Naming Components

### Cloud Service Provider & Region

***Cloud Service Provider*** (*Cloud Provider*, or abbr. *CSP*) is the cloud service provider for infrastructure instances. It SHOULD be defined by the infrastructure department.

***Region*** is the name or short name of the geographic location where the infrastructure instances are located, given by Cloud Provider.
It SHALL be disambiguated from the available zones.

For the on-premise data centers, servers, clusters, those are NOT provided by any Cloud Provider, the provider MUST be marked as the fixed value `onp` for *On-Premises*.

**Naming Format**
- CSP MUST be a short code, matching the regex pattern `^[a-z][a-z0-9]{1,4}$`.
  For examples:
  - `aws` for Amazon Web Service
  - `tce` for Tencent Cloud Enterprise
  - `hwc` for Huawei Cloud
- Region MUST be given by CSP.
  For examples:
  - `shanghai` for Shanghai region

### Business Product

***Business Product*** is a short code for the enterprise business product or enterprise business system.

**Naming Format**
- It MUST be a short code, matching the regex pattern `^[a-z][a-z0-9]{1,6}$`.
  For example:
  - `oa` for OA system (Office Automation)
  - `fin` for the enterprise finance system
  - `edw` for the enterprise data warehouse

### ECS/BMS/VMH

- ECS for Elasitc Cloud Server.
- BMS for Bare Metal Server.
- VMH for Virtual Machine Host.

### Machine Type

***Machine Type*** is the actual machine type of the instance. It is generally classified into the CPU/GPU/NPU types. GPU product code SHOULD be given by GPU manufacturer, e.g. NVIDIA A100.

**Naming Format**

It MUST be a short code, matching the regex pattern `^(cpu|(gpu|npu)-[a-z0-9]{1,6})$`.
- Type `CPU`: The instance is a regular compute instance, which is not a GPU instance, usually specified to the x86_64 compute instances.
  - It MUST be fixed value `cpu`.
- Type `GPU`: The server equipped with GPU devices.
  - It MUST be a `gpu-` prefixed lowercased GPU product code given by GPU device manufacturer.
  - It MUST NOT be determined by GPU allocation, e.g. single card, MIG, vGPU.

  For examples:
  - `gpu-a100` for NVIDIA A100
  - `gpu-l20` for NVIDIA L20
- Type `NPU`: The server equipped with NPU devices.
  - It MUST be a `npu-` prefixed lowercased GPU product code given by NPU device manufacturer.
- (OPTIONAL) Lower-endcomputer types are given by specific industry domains.
  
  For examples:
  - `adcu` for ADCU (ADAS Domain Controller Unit)
  - `cdc` for CDC (Cockpit Domain Controller)

### Deployment Environment

***Deployment Environment***, usually called Environment, is the environment where the applications and services deploy through the developing stages. It varies from the entriprise development mode.

**Naming Format**

It MUST be the enumerated code, matching the regex pattern `^(cpu|(gpu|npu)-[a-z0-9]{1,6})$`. For examples:
  - `prod` = Production (Production Environment)
  - `dr` = DR (Disaster Recovery Environment)
  - `pre` = PRE (Pre-Production Environment)
  - `uat` = UAT (User Acceptance Test Environment)
  - `sit` = SIT (System Integration Test Environment)
  - `dev` = DEV (Development Environment)

### Server Suffix

Server suffix is the unique identifier for this specific server.

It MAY be the one of the following:
- Using sequence number:
  - Sequence number MUST start at 1 and succeeds in sequence.
  - Left pading with 0 to 3 digits.
  - Sequence numbers greater than 999 are written in the actual digits.
  
  For examples:
  - `server-001` for the first server
  - `server-1001` for the 1001th server
- Using IPv4 address identifier:
  - It MUST bt last two segments of IPv4 address, joined with `-`.
  - If the server has more than one ethernet interface or addresses, the business address MUST be used.
  
  For examples:
  - `server-xxx-77-19` for `10.x.77.19`
- Parts of machine serial number.

## Section 2: Naming Format

### Server / Host

**Naming Format**

Server / host instance name MUST be in the form of FQDN:

- \<CSP>-\<Region>-\<Product>-\<Name>-\<Type>-\<Env>-\<Suffix>.ecs.example.com
- \<CSP>-\<Region>-\<Product>-\<Name>-\<Type>-\<Env>-\<Suffix>.bms.example.com
- \<CSP>-\<Region>-\<Product>-\<Name>-\<Type>-\<Env>-\<Suffix>.vmh.example.com
- \<CSP>-\<Region>-\<Product>-\<Name>-\<Env>-k8s.k8s.example.com

Components:
- CSP: Cloud Service Provider in Section 1
- Region: CSP Region in Section 1
- Product: Business Product in Section 1
- (OPTIONAL) Name: Business name
- Env: Environment in Section 1
- Suffix: Server Suffix in Section 1

For examples:
  - `aws-ua-east-1-oa-gpu-a800-prod-001.ecs.example.com`
  - `hwc-shanghai-fin-cpu-dev-214-29.bms.example.com`

## Appendix

### Appendix A 

Abbreviations for cloud productions

| Abbr. | Production |
|-------|------------|
| bms   | Bare Metal Server |
| cr    | Contaienr Registry |
| dc    | Direct Connect |
| dcg   | Direct Connect Gateway |
| ecs   | Elasitc Cloud Server |
| lb    | Load Balancer |
| pl    | Private Link |
| rtb   | Route Table |
| sg    | Security Group |
| vmh   | Virtual Machine Host |
| vpc   | VPC (Virtual Private Cloud) |