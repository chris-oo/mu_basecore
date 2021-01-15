# Unified Extensible Firmware Interface Engineering Change Request (ECR)
SPDX-License-Identifier: CC-BY-4.0
# ***Draft for Review***

## Title: Unaccepted Memory Type Addition to EFI_MEMORY_TYPE
Document: ACPI 6.3A

Sponsors:<br>
***Jiewen Yao, Intel***<br>
***Chris Oo, Microsoft***<br>
***Jon Lange, Microsoft***<br>
***Sean Brogan, Microsoft***<br>
***Brijesh Singh, AMD***<br>
***Thomas Lendacky, AMD***<br>
<br>
**Submission for Review Date: x/xx/2021**<br>
**Review Approval Date: x/xx/2021**<br>
**Submission for Technical Editing Date: x/xx/2021**<br>
**Submission for Draft Review Date: xx/xx/2021**<br>
**Verification Date: x/xx/2020**<br>
**Verifiers: TBD**<br>

## Summary
### Summary of Change
This ECR introduces a new memory type to the AddressRangeUnaccepted that is to be used to represent unaccepted guest memory to OS Loaders.

This is aligned with Unaccepted Memory UEFI ECR (https://mantis.uefi.org/mantis/view.php?id=2134), Unaccepted Memory PI ECR (https://mantis.uefi.org/mantis/view.php?id=2161).

The latest draft is also at: https://github.com/microsoft/mu_basecore/pull/66

### Benefits of the Change

This change allows non-UEFI OS or OS loader pass the information to OS kernel, such as ACPI E820 table.

### References

See the following PR for the latest version of this ECR and the corresponding UEFI ECR, at https://github.com/microsoft/mu_basecore/pull/66.

## Detailed Description of the Change and Special Instructions

### Summary of Changes:
- Add AddressRangeUnaccepted to Table 15-324 Address Range Types

~~Text Removed~~,  Text Added ***Text Added*** ,  New Sections ***New Section***, Discussion/Mode Edits needed ***DME***

#### Table 15-324 Address Range Types
**Table 15-324 Address Range Types**<br>
| value |        Mnemonic        | Save in S4 | Description
|-------|------------------------|------------|-------------
| 8     | AddressRangeUnaccepted | No         | A memory region that represents unaccepted memory, that must be accepted by the boot target before it can be used. Unless otherwise noted, all other memory types are accepted. For platforms that support unaccepted memory, all unaccepted valid memory will be reported as unaccepted in the memory map. Unreported physical address ranges must be treated as not-present memory.  ***Text Added***
| 8-11 ~~Text Removed~~ | 
| 9-11 ***Text Added*** | 


***Text Added***

### Special Instructions:
#### Discussions / Opens