# ANC DHIS2 Metadata Package

> Antenatal Care Tracker — Configured by Devotta for the Norwegian Institute of Public Health

---

## Overview

The **ANC DHIS2 Metadata Package** is a configurable DHIS2 implementation designed to support the tracking of pregnant women from their initial antenatal care (ANC) visit through the postpartum period. The package follows an **8-visit schedule** for interventions and provides both individual-level (Tracker) and population-level (Aggregate) data collection.

The package includes:

- A **Tracker programme** (`RMNCAH_ANC`) for individual-level data capture in the Capture app
- An **Aggregate dataset** (`eHMIS ANC Data`) that consolidates program indicators into routine monthly data via the Data Exchange app
- A **dashboard** (`HMIS Maternal Health Report`) with key ANC indicators for national, regional, and facility-level analytics
- An **8-visit scheduling plugin** (`ANC Visits`) for calculating visit intervals from gestational age
- **SMS notification rules** for appointment reminders and missed visits

---

## Repository Structure

```
anc-dhis2-metadata/
├── README.md                        ← This file
├── docs/
│   ├── system-design.md             ← System design document
│   └── installation-guide.md        ← Step-by-step installation guide
├── metadata/
│   └── anc_metadata.json            ← DHIS2 metadata package (JSON)
└── data-dictionary/
    └── data-dictionary.md           ← Data dictionary for all metadata objects
```

---

## Quick Links

| Document | Description |
|---|---|
| [System Design](docs/system-design.md) | Architecture, program structure, configuration decisions |
| [Installation Guide](docs/installation-guide.md) | How to import and configure the package into your DHIS2 instance |
| [Data Dictionary](data-dictionary/data-dictionary.md) | List of metadata objects with descriptions |
| [Metadata Package](metadata/anc_metadata.json) | Importable DHIS2 metadata JSON |

---

## Compatibility

| Requirement | Details |
|---|---|
| DHIS2 Version | 2.41+ |
| Android Compatibility | Yes (web plugin not supported on Android) |
| Recommended Android RAM | Minimum 6 GB |

---

## Contributors

This toolkit is based on the initial RMNCAH toolkit developed by Folkehelseinstitutet and further developed by Devotta.

Special thanks to HISP Uganda for their configurations for the Eposit project, HISP Ethiopia for their implementation efforts in Ethiopia.

---

## License


BSD 3-Clause "New" or "Revised" License

Copyright (c) 2004-2022, University of Oslo
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:
* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.
* Neither the name of the HISP project nor the names of its contributors may
  be used to endorse or promote products derived from this software without
  specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


