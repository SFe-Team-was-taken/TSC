# Trailing sdta chunk (TSC) mode specification

# Version 1.0.2 (Draft specification)

Copyright © 2024 sylvia-leaf

## 0.1 Revision history

|          |                      |                                                           |
|----------|----------------------|-----------------------------------------------------------|
| Revision | Date                 | Description                                               |
| 1.0.2    | November 19, 2024    | Versioning update <br> Added SFty subchunk information    |
| 1.0.1    | October 17, 2024     | First version <br> Specification based on SFe 4.00.5      |

* * *

## 0.2 Disclaimers

This isn't an integral part of the SFe specification. This is a supplementary specification that may or may not be implemented alongside SFe by program developers.

This is a draft. Expect errors, and feel free to report them. sleaf has access to a computer with an Sound Blaster X-Fi Titanium, so she will be able to test or reproduce any issues found on real hardware.

Do not use "draft" specifications (version number x.yy.zL) to base final products on. Always refer to a "final" specification (version number x.yyL).

* * *

## 0.3 Updates and comments

Please send all comments about this specification to the SFe Team:

- GitHub: https://github.com/SFe-Team-was-taken

* * *

## 0.4 Table of contents

The table of contents is currently not implemented.

# Section 1: Introduction

## 1.1 Scope and purpose of this document

- This is a draft specification for TSC mode for legacy SF and SFe.

## 1.2 Organisation of this document

The organisation of the document is similar to that of the SFe document (and thus also SFSPEC24.PDF), but adapted to the requirements of the TSC specification.

## 1.3 Improvements and enhancements

The TSC specification will go through a few draft milestones, after which the specification will freeze. This is a very simple feature, and there is little need for extra features to be added to this specification.

# Section 2: Information about TSC mode

## 2.1 What is TSC mode?

As Saga of OpenMPT has found out, it is in fact possible to increase the maximum size of the 32-bit SF 2.04 format to 8 gibibytes (GiB), if the sdta chunk is placed at the end of the file rather than in the middle as usual for SF 2.04. This is referred to in this specification as trailing sdta chunk mode, or TSC mode for short.

## 2.2 Use of TSC mode

- When TSC mode is off, only the first 4 GiB of the SFe32 file is loaded.
- When TSC mode is on, up to 8 GiB of the SFe32 file can be loaded.
- No SF file that uses TSC will have exactly 8 GiB. The size of the sdta-list chunk, which contains the sample data, is still limited to 4 GiB.

## 2.3 SFty sub-chunk information

The `ISFe-info` sub-chunk in an SFe bank contains two sub-chunks that contain data related to TSC. Because TSC can be used with legacy SF, the absence of an `ISFe-info` sub-chunk does not mean that a TSC bank is invalid. 

The `SFty` sub-chunk found inside the `ISFe-info` sub-chunk can have the values `SFe32 with TSC` or `SFe32L with TSC`. This tells an SFe-capable program that the `sdta-info` chunk is the last chunk. 

If the program can handle such a bank, then it should assume that `sdta-info` is at the end.

If the program cannot, then it should reject the file as unsupported (but not Structurally Unsound if it is a syntactically valid SFe32 with TSC bank). All SFe players must recognise the above values and respond accordingly.

# Section 3: Compatibility specification

## 3.1 TSC mode compatibility concerns

Such files will not work with all legacy SF players, including sound cards. Therefore, we strongly recommend that you use SFe64 (or SFe64L) rather than TSC mode.

Saga also warns against using TSC mode due to compatibility concerns.

## 3.2 TSC requirements for SFe programs

No SFe programs will require TSC. However, SFe programs should detect the TSC feature flag in ISFe and reject TSC banks that aren't compatible.

SFe players not compatible with TSC should automatically detect a file size above 4 GiB, and return an error message as described in the program specification.

## 3.3 Assuming TSC mode for supported programs

If an SF player that supports TSC attempts to load a bank larger than 4 GiB, and a 32-bit header is found, then TSC mode is assumed. 
