# ANC FHI — System Design Document

---

## 1. Background and Purpose

The ANC package produced by FHI aims at following pregnant women from their initial visit to the postpartum period, and adding an 8-visit schedule for their interventions.

---

## 2. System Design Overview

### 2.1 Use Case

The package uses a Tracker programme configured for the Capture app, and an aggregate dataset that is used to aggregate indicators into routine data.

### 2.2 Program Structure

The program is built around a single Tracker program (`RMNCAH_ANC`) with enrollment attributes and multiple program stages aligned to the 8-visit ANC schedule.

### 2.3 Explanation of Program Structure

#### TEI Type

The program uses the TEI type **PERSON** (UID: `MCPQUTHX1Ze`) and currently has three attributes (Name, Surname, UID) which are allowed to be seen in list.

> This is not a requirement and changing it should not affect the functioning of the programme.

#### Enrollment

The enrollment has three sections: **Biodata**, **Other Details**, and **Address**.

##### Biodata Section

This section records the personal details of the patient and UIDs. Key considerations:

- **ANC UID**: This attribute has an auto-assigned numerical pattern.
- **Client No.**: The client number requires a pattern to be validated in the format `XXXXX/YY` (e.g. `00001/23`).
- **Client ID**, Surname, and given name are **mandatory** fields.

##### Other Details of the Pregnant Woman

- **National Identification Number (NIN)**: Has a validation pattern that needs to be modified to match the local ID number through rule `OtrWkxSZKu3`.
- **Phone Number**: Has a validation pattern — program rule `krKqWluxCU8to` needs to be modified.

##### Address of the Pregnant Woman

Details about the address need to be adapted to the local address format. Currently there are 4 fields: **district**, **subcounty**, **parish**, and **village**. These should be modified to fit an address pattern that matches your needs.

### 2.4 Intended Users

The program is intended for data entry use at **facility level**, including a **supervisor role** to ensure data entry is done properly. Analytics are provided for **management** to follow acceptance and key indicators.

---

## 3. Datasets and Tracker Configuration

### Tracker

The Tracker program is configured as **ANC - RMNCAh Antenatal care registry**, with the code `RMNCAH_ANC`.

- Access level: **Audited** — must be evaluated according to privacy laws and workflows in country
- Displays a front page list
- Minimum of 1 attribute required to search

### Dataset

The dataset **eHMIS ANC Data** is configured as a **monthly dataset** with data to be entered at facility level. It uses the **Data Exchange app** to gather data from program indicators and turn it into aggregate data values.

---

## 4. Datasets and Stage Details

### 8-Visit Schedule Plugin

A React plugin called **"ANC Visits"** has been added in order to schedule the 8 visits at the correct intervals. It uses the gestational age number from the "Profile and history" stage to determine the visit intervals required.

**Installation:**

The plugin must be installed from the [DHIS2 App Hub](https://apps.dhis2.org) and enabled through the Datastore Management app or the Tracker Plugin Configurator app.

**Datastore configuration:**

The `anc-visits` key must be set up in Datastore Management with the following config:

```json
{
  "metadata": {
    "dataElements": {
      "ancVisitNumber": "bXVD2EMF7UW",
      "gestationalAge": "w9p8MQDRyMr"
    },
    "programStages": {
      "firstVisit": "iF5roNU7QWm",
      "followUpVisit": "JqW7c9HYjVr"
    }
  },
  "settings": {
    "autoCalculate": true,
    "calendar": "gregory",
    "dateFormat": "yyyy/MM/dd",
    "numberOfVisits": 8
  }
}
```

**Enrollment overview layout (Capture app):**

Under the `capture` key, modify the enrollment overview file to include the plugin. Replace `YOURINSTANCE` with your DHIS2 instance URL:

```json
{
  "WSGAb5XwJ3Y": {
    "leftColumn": [
      { "name": "QuickActions", "type": "component" },
      {
        "source": "YOURINSTANCE/api/apps/ANC-Visit-Date-Calculator/plugin.html",
        "type": "plugin"
      },
      { "name": "StagesAndEvents", "type": "component" }
    ],
    "rightColumn": [
      { "name": "ErrorWidget", "type": "component" },
      { "name": "WarningWidget", "type": "component" },
      { "name": "EnrollmentNote", "type": "component" },
      { "name": "FeedbackWidget", "type": "component" },
      { "name": "IndicatorWidget", "type": "component" },
      { "name": "TrackedEntityRelationship", "type": "component" },
      {
        "name": "ProfileWidget",
        "settings": { "readOnlyMode": false },
        "type": "component"
      },
      {
        "name": "EnrollmentWidget",
        "settings": { "readOnlyMode": false },
        "type": "component"
      }
    ]
  }
}
```

For more information on the plugin, visit: [https://github.com/dev-otta/antenatal-visits-plugin](https://github.com/dev-otta/antenatal-visits-plugin)

### Notifications

#### Standard Appointment Notifications

There are 3 notifications currently configured:

##### 7 Days Before Appointment / 1 Day Before Appointment

**When:** Sent 7 days or 1 day before a scheduled ANC Examination event, to remind the patient of the upcoming appointment.

**Message:**
```
Hi A{sB1IHYu2xQT} Your upcoming ANC visit is on V{due_date} in V{days_until_due_date} from today. Best regards your nurse at V{org_unit_name}
```

**To:** Uses the attribute **"SMS Phone number"**. If the SMS phone number attribute has been removed, this will need to be modified.

##### 2 Days After Appointment (Missed Visit)

**When:** Sent 2 days after a scheduled appointment was meant to have occurred.

**Message:**
```
Hi A{sB1IHYu2xQT} We missed you on V{due_date} Please come to the clinic as soon as you can. Best regards your nurse at V{org_unit_name}
```

**To:** Uses the attribute **"SMS Phone number"**. If the SMS phone number attribute has been removed, this will need to be modified.

#### Lifestyle SMS Notifications

A script is available for sending lifestyle notifications on a roster schedule for pregnant women. See documentation here: [LINK TO SCRIPT]

---

## 5. Validation Rules / Program Rules

[Insert here table with validation rules, their description, and any needed justification.]

---

## 6. User Groups

The program follows standard user groups for metadata packages:

| User Group | Description |
|---|---|
| ANC - Access | Allows for access to analytic data |
| ANC - Admin | Allows for access to configure metadata (no data access) |
| ANC - Data Capture | Allows access for capturing of data (but no editing metadata) |

**Other user groups to consider:**

| User Group | Description |
|---|---|
| Control Group | Allows filtering clinics when they will not be following the same schedule |
| Intervention Group | Allows for some staff to perform restricted functions |

---

## 7. Analytics and Indicators

[Indicators to be documented. No requirements defined yet.]

---

## 8. Dashboards

The program includes the dashboard **"HMIS Maternal Health Report"** — a summary of ANC indicators captured by the program, ready to be used at national, regional, or facility level.

---

## 9. Android Compatibility

The program is compatible with the DHIS2 Android app.

> **Note:** Due to the complexity of program rules, older devices with low RAM may become less responsive. A **minimum of 6 GB RAM** is recommended.

> **Plugin limitation:** Android does not currently support plugins. The ANC Visit scheduling plugin for generating visit dates (ANC 2–8) only works on **web** devices.

---

## 10. Performance Considerations

With mobile implementations, consider that due to the complexity of program rules, older devices with low RAM might become less responsive.

---

## 11. References

[List of references, alphabetical order]
