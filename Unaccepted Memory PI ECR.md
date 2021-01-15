# Unified Extensible Firmware Interface Engineering Change Request (ECR)
SPDX-License-Identifier: CC-BY-4.0
# ***Draft for Review***

## Title: Unaccepted Memory Type Addition to PI HOB Resource types and GcdMemoryType
Document: PI V1.8

Sponsors:<br>
***Chris Oo, Microsoft***<br>
***Jon Lange, Microsoft***<br>
***Sean Brogan, Microsoft***<br>
***Brijesh Singh, AMD***<br>
***Thomas Lendacky, AMD***<br>
***Jiewen Yao, Intel***<br>
<br>
**Submission for Review Date: x/xx/2020**<br>
**Review Approval Date: x/xx/2020**<br>
**Submission for Technical Editing Date: x/xx/2020**<br>
**Submission for Draft Review Date: xx/xx/2020**<br>
**Verification Date: x/xx/2020**<br>
**Verifiers: TBD**<br>

## Summary
### Summary of Change
This ECR supports the introduction of a new memory type, unaccepted memory, with updates to the corresponding PI HOB Resource types and GcdMemoryTypes. See the corresponding ECR introducing a new memory type to the EFI_MEMORY_TYPE for more information on unaccepted memory.

### Benefits of the Change
This change allows PEI to reflect the current state of accepted memory to DXE so that it can be used correctly, along with reporting it to the OS loader.

### References

See the following PR for the latest version of this ECR and the corresponding UEFI ECR, at https://github.com/microsoft/mu_basecore/pull/66.

## Detailed Description of the Change and Special Instructions

### Summary of Changes:
- Update Section 7.2.4 Platform Initialization Specification, Vol. 2, Services - DXE Services with AddMemorySpace new unaccepted type
- Update Section 9.8.4 Platform Initialization Specification, Vol. 2, DXE Foundation with Resource Descriptor HOBs Table 2-20 adding a new row describing unaccepted memory
- Update Section 5.3 Platform Initialization Specification, Vol. 3, HOB Code definitions with PHIT version revision
- Update Section 5.5 Platform Initialization Specification, Vol. 3, HOB Code definitions with new resource type

~~Text Removed~~,  Text Added ***Text Added*** ,  New Sections ***New Section***, Discussion/Mode Edits needed ***DME***

#### 7.2.4 Global Coherency Domain Services

**AddMemorySpace()**<br>
...<br>
**Parameters**<br>

**GcdMemoryType**<br>
The type of memory resource being added.  Type EFI_GCD_MEMORY_TYPE is defined in “Related Definitions” below.  The only types allowed are EfiGcdMemoryTypeReserved, EfiGcdMemoryTypeSystemMemory, EfiGcdMemoryTypePersistent, EfiGcdMemoryTypeMoreReliable, EfiGcdMemoryTypeUnaccepted, and EfiGcdMemoryTypeMemoryMappedIo. ***Text Added***<br>
...<br>
**Description**<br>
...<br>
If GcdMemoryType is not EfiGcdMemoryTypeReserved, EfiGcdMemoryTypeSystemMemory, EfiGcdMemoryTypeMemoryMappedIo, EfiGcdMemoryPersistent, ~~or~~ EfiGcdMemoryTypeMoreReliable or EfiGcdMemoryTypeUnaccepted then EFI_INVALID_PARAMETER is returned. ***Text Added***<br>
...<br>
**Related Definitions**<br>
``` c
//*******************************************************
// EFI_GCD_MEMORY_TYPE
//*******************************************************
typedef enum {
  EfiGcdMemoryTypeNonExistent,
  EfiGcdMemoryTypeReserved,
  EfiGcdMemoryTypeSystemMemory,
  EfiGcdMemoryTypeMemoryMappedIo,
  EfiGcdMemoryTypePersistent,
  EfiGcdMemoryTypeMoreReliable,
  EfiGcdMemoryTypeUnaccepted,
  EfiGcdMemoryTypeMaximum
} EFI_GCD_MEMORY_TYPE;
```
...<br>
**EfiGcdMemoryTypeMoreReliable**<br>
A memory region that provides higher reliability relative to other memory in the system. If all memory has the same reliability, then this bit is not used.<br>

***Text Added***<br>
**EfiGcdMemoryTypeUnaccepted**<br>
A memory region that is unaccepted. This region must be accepted before it can be converted to system memory.<br>
***Text Added***<br>
...<br>

#### 9.8.4 Resource Descriptor HOBs
...<br>
**Table 2-20: Resource Descriptor HOB to GCD Type Mapping**<br>
| Resource Descriptor   HOB |            | GCD Map                     |          |
|---------------------------|------------|-----------------------------|----------|
| Resource Type             | Attributes | Memory Type                 | I/O Type |
| System Memory             | Present    | Reserved                    |          |
| …                         | …          | …                           | …        |
| Memory Reserved           |            | Reserved                    |          |
| Unaccepted Memory         |            | Unaccepted ***Text Added*** |          |
| …                         | …          | …                           | …        |
...<br>

#### 5.3 PHIT HOB
...<br>
**Related Definitions**<br>
``` c
//*****************************************************
// Version values
//*****************************************************
#define EFI_HOB_HANDOFF_TABLE_VERSION  0x000a // Text Edited
```
...<br>
#### 5.5  Resource Descriptor HOB
...<br>

**Related Definitions**<br>
There can only be a single ResourceType field, characterized as follows.
``` c
//*********************************************************
// EFI_RESOURCE_TYPE
//*********************************************************

typedef UINT32 EFI_RESOURCE_TYPE;

#define EFI_RESOURCE_SYSTEM_MEMORY           0x00000000
#define EFI_RESOURCE_MEMORY_MAPPED_IO        0x00000001
#define EFI_RESOURCE_IO                      0x00000002
#define EFI_RESOURCE_FIRMWARE_DEVICE         0x00000003
#define EFI_RESOURCE_MEMORY_MAPPED_IO_PORT   0x00000004
#define EFI_RESOURCE_MEMORY_RESERVED         0x00000005
#define EFI_RESOURCE_IO_RESERVED             0x00000006
#define EFI_RESOURCE_MEMORY_UNACCEPTED       0x00000007 // Text Added
#define EFI_RESOURCE_MAX_MEMORY_TYPE         0x00000008 // Text Edited
```

The following table describes the fields listed in the above definition.
| EFI_RESOURCE_SYSTEM_MEMORY         | Memory that persists out of the HOB producer phase.                                                                                                                                                                                |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| EFI_RESOURCE_MEMORY_MAPPED_IO      | Memory-mapped I/O that is programmed in the HOB producer phase.                                                                                                                                                                    |
| EFI_RESOURCE_IO                    | Processor I/O space.                                                                                                                                                                                                               |
| EFI_RESOURCE_FIRMWARE_DEVICE       | Memory-mapped firmware devices.                                                                                                                                                                                                    |
| EFI_RESOURCE_MEMORY_MAPPED_IO_PORT | Memory that is decoded to produce I/O cycles.                                                                                                                                                                                      |
| EFI_RESOURCE_MEMORY_RESERVED       | Reserved memory address space.                                                                                                                                                                                                     |
| EFI_RESOURCE_IO_RESERVED           | Reserved I/O address space.                                                                                                                                                                                                        |
| EFI_RESOURCE_MEMORY_UNACCEPTED     | Memory that is unaccepted. ***Text Added***                                                                                                                                                                                        |
| EFI_RESOURCE_MAX_MEMORY_TYPE       | Any reported HOB value of this type or greater should be deemed illegal. This value could increase with successive revisions of this specification, so the “illegality” will also be based upon the revision field of the PHIT HOB |

### Special Instructions:
#### Discussions / Opens