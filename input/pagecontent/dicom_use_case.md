**TODO**
Clean up mapping:
1. FHIR to DICOM (help from WG-20)
2. Represent FHIR extensions in mapping tables as they would appear in Sushi


### Introduction

This use case illustrates DICOM Sex and Gender encoding, including: admission, patient prep, examination, post processing and reporting for a PET/CT examination order. In this case, there are three instances of Sex Parameters for Clinical Use (SPCU). First, the ordering physician provides instructions for interpreting lab values within a Sex Parameters for Clinical Use comment. In the second SPCU, a post-processing AI (Artificial  Intelligence) application utilizes the Sex at Birth Sequence for the basis of reference values. Third, the radiologist determines the appropriate sex to use based on the patient's body composition for a Standard Uptake Value (SUV) calculation. A patient with EHR Sex Parameters for Clinical Use (SPCU) of “female” and a EHR Gender Identity of “male” checks-in for a PET/CT examination. The examination is performed, the patient’s demographics are updated, and the report is delivered. The DICOM (Digital Imaging and COmmunications in Medicine) Standard attributes in this use case are not, at time of publication of this Implementation Guide, normative, and details in DICOM are still being defined. Readers interested in participating in development of DICOM Sex and Gender encoding, please contact the [DICOM Secretariat](mailto:dicom@dicomstandard.org).

### Actors:

#### People

1.  Patient (John Smith) - whose previous records are for studies performed when his EHR Gender Identity was “female”. 

2.  Facility Clerk - admits patient, utilizes the Radiology Information System (RIS)

3.  PET/CT Technologist

4.  Recovery Nurse

#### Systems, (using IHE Actor names)

1.  Hospital EHR (ADT, Order Placer)

2.  RIS (Order Filler)

3.  PET/CT Modality System

4.  Image Manager/Archive (PACS: Picture Archive and Communications System)

5.  Dose Information Reporter

6.  AI (Artificial Intelligence) Task Performer

### Scope Statement:

Use case covers admission, patient prep, examination, recovery, post processing and reporting for a PET/CT examination order.

### Precondition(s):

1.  John Smith is registered in the hospital record system with his old name of “Janet Smith”

2.  Patient ID has not changed

3.  John Smith arrives at an outpatient facility with an appointment

4.  Patient history, social history, medical history has already been captured upstream and are available in the facility's EHR

5.  Physician order for examination is utilizing information from the facility

6.  Facility system has not been updated for the name change

7.  Relevant prior exams for comparison are retrieved based on rules established by the radiology department, using the name Janet Smith (e.g., body region, patient ID, type of exam).

8.  Technical scan and contrast administration parameters (protocol) are pre-determined based on departmental protocols for a female patient

[**Example 01**](#example-01-imaging-order) depicts a HL7 v2.9.1 Imaging Order for this patient with mapping to DICOM Modality Worklist attributes.

### Postcondition(s):

1.  PET/CT Examination is complete or cancelled

2.  Report is generated and available

3.  Discussion to initiate name change correction in the EHR has occurred

4.  DICOM Name to Use is updated based on policy. For example:
    
    1.  EHR name (Janet Smith) is associated with a Name to Use element whose validity period ends on day of exam
    
    2.  Add a Name to Use (John Smith) whose validity period begins on day of exam

### Workflow/Storyboard:

Note: IHE transactions are noted in brackets
<div>
{%include gh-dicom-2.puml%}
</div>

Figure 1 Workflow Storyboard

#### Arrival and check-in:

In this scenario, the patient initiates the discussion with the clerk.

1.  When John arrives at the waiting room for a PET/CT examination he announces himself as “John”.

2.  The clerk asks “John Williams?”, seeing a John Williams in the schedule. 

3.  Response, “No, Smith”

4.  The clerk asks “Date of birth”

5.  Smith: “month, day, year”

6.  The clerk performs a date-of-birth based lookup and finds:

7.  A schedule entry for Janet Smith, with Patient’s Sex “F” and Patient’s Gender “M”, and with a Patient Names to Use “John Smith”. Sex Comment contains “Hormonal treatment, use affirmed gender Cr reference ranges\[1\]”.

8.  The clerk confirms that the birth dates match, confirms the patient’s identity in accordance with local policies, and checks in the patient.

9.  The HL7 v2.9.1 message is converted to DICOM Modality Worklist (MWL) Attributes (partial SOP Instance contents) for the MWL query. After check-in, the order is visible in the MWL

10. Based on clinic policies, the clerk asks whether John wants to go through the name change process at the clinic to reflect his preferred name. Name change is initiated.

11. The clerk notifies the technologist that the patient has arrived.

[**Example 02**](#example-02-patient-name-update) depicts a HL7 v2.9.1 Demographics Update message for this patient with mapping to DICOM Modality Worklist attributes.

#### Patient Preparation

1.  The technologist checks their schedule for John, and finds the order for “Janet Smith”, Patient’s Sex “F” and Patient’s Gender “M”, and with a Patient Names to Use “John Smith”. Sex Comment contains “Hormonal treatment, use affirmed gender Cr reference ranges”.

2.  The technologist greets the patient as “John” and reconfirms birthdate.

3.  The technologist directs the patient to a changing area and instructs the patient to remove jewelry and change into a gown.

4.  When the patient is ready, the technologist asks the necessary related preparation questions, e.g., pregnancy status, most recent menstruation, allergies, history, preferred arm for IV contrast administration, etc.

5.  The technologist explains the procedure to the patient and answers any questions the patient may have. 

6.  Since the protocol calls for a contrast-enhanced CT, the technologist reviews the most recent eGFR, bun and creatine.

7.  The technologist confers with the radiologist to discuss acceptable lab values for safe contrast administration, given the Sex Comment, as well as the patient’s GFR, bun and creatine.

8. The radiologist notes that the provided SPCU of Female, is not consistent with the SPCU Comment and calls the ordering physician to confirm.

9.  After discussing patient history with the ordering physician, the radiologist provides protocol alterations based on the  patient’s transgender status.

Note: The pre-identified protocol was based on a female patient (see item [8 in Precondition(s)](#preconditions)).

#### Examination

1.  The technologist knows to select the MWL entry for “Janet Smith” and expects a Patient’s Sex of “F”; this does not trigger a wrong patient concern.

2.  Patient demographics are loaded into the scanner demographics interface.

3.  The technologist applies alterations prescribed by the radiologist to scanner and contrast protocol. The radiotracer dose is not changed, as the department standardizes doses regardless of Patient’s Sex.

4.  The technologist starts an IV, administers radiotracer, and connects the contrast injector for the contrast-enhanced CT portion of the procedure.

5.  The study is performed.

6.  During administration of iodinated CT contrast, the patient complained of nausea. The technologist notifies the departmental nurse, who agrees to monitor the patient for an allergic reaction after the procedure.

7.  The images and Radiation Dose Structured Report (RDSR) are transferred to the PACS, Dose Information Reporter and AI Task
    Performer systems.

#### Analysis 

1.  The radiologist creates an SUV ROI on the PACS. The application detects the presence of Patient’s Sex (0010,0040) “F”, a Gender Code (0010,xxx4).(0008,0104) “Male” and Sex at Birth Sequence (0010,xx25) of “F” and prompts the radiologist to enter a value “M” or “F”.

2.  The Dose Information Reporter collects the RDSR, without exception.

3.  The AI task performer parses the Recorded Sex or Gender Sequence (0010,xx14) and identifies a Recorded Value(0010,xx15) of “F” for an item with a RSG Type Code Sequence (0010,xx16) of (76689-9,LN,Sex assigned at birth). The algorithm processes the images based on female reference values and transfers evidence documents to the PACS.

Note: Sex at Birth is required to determine reference values for AI and non-AI machine algorithms in various domains, such as cardiology and neurology.

#### Reporting

1.  The radiologist dictates findings pertaining to the procedure, noting scanner and contrast protocol modifications in the “Request” section of the report.

2.  The report format has been configured to include Patient’s Sex (0010,0040), Patient’s Gender Code (0010,xxx4).(0008,0104), Patient Name (0010,xxx3).(0010,xx12) and SPCU Comment (0010,xxx1) in the report.

3.  The initial report reads:
> Patient’s Name = “Janet Smith”  
> Patient’s Sex = “F”  
> Patient’s Gender = “M”  
> Name to Use = “John Smith"  


4.  After the patient’s name change has been processed, the report is addended. The addended report reads:
> Patient’s Name = “John Smith”  
> Patient’s Sex = “F”  
> Patient’s Gender = “M”  
> Name to Use = “John Smith" 

The final report may be represented in [FHIR](#example-03-fhir-mapping), [HL7 v2](#example-04-imaging-report), or a [CDA](#example-05-cda-release-2-imaging-report).

### Examples

#### Example 01: Imaging Order

See these [examples](v2dicom_use_case.html#order-for-imaging) of HL7 v2.9.1 and v2.5 OMI Imaging Orders from [precondition(s)](#preconditions).

These map to DICOM Modality Worklist as follows:

| V2                                   | Attribute Name                | Tag         | VR | Value                                                             |
| ------------------------------------ | ----------------------------- | ----------- | -- | ----------------------------------------------------------------- |
| PID-5 Name Type Code = Birth Name    | Patient's Name                | (0010,0010) | PN | Smith\^Janet^^^                                                   |
| PID-8                                | Patient's Sex                 | (0010,0040) | CS | F                                                                 |
|                                      | Patient’s Gender              | (0010,xxxx) | CS | M                                                                 |
|                                      | Gender Identity Sequence      | (0010,xxxx) | SQ |                                                                   |
|                                      | \>Gender Code Sequence        | (0010,xxx4) | SQ |                                                                   |
| GSP-5-1                              | \>\>Code Value                | (0008,0100) | SH | 446151000124109                                                   |
| GSP-5-3                              | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| GSP-5-2                              | \>\>Code Meaning              | (0008,0104) | LO | Identifies as male gender                                         |
|                                      | \>Start DateTime              | (0010,xxx6) | DT |                                                                   |
|                                      | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
|                                      | \>Gender Comment              | (0010,xxx8) | LT |                                                                   |
|                                      | Sex Parameter for Clinical Use Sequence | (0010,xxx2) | SQ |                                                         |
|                                      | \>SPCU Code Sequence          | (0010,xxx9) | SQ |                                                                   |
| GSC-4-1                              | \>\>Code Value                | (0008,0100) | SH | Male-typical                                                      |
| GSC-4-3                              | \>\>Coding Scheme Designator  | (0008,0102) | SH | SexParameterForClinicalUse                                               |
| GSC-4-2                              | \>\>Code Meaning              | (0008,0104) | LO | Male typical parameters                                           |
|                                      | \>Start DateTime              | (0010,xxx6) | DT | 20220715090000                                                    |
|                                      | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
| GSC-8                                | \>SPCU Comment                | (0010,xxx1) | LT | Hormonal treatment, use affirmed gender Cr reference ranges       |
|                                      | \>SPCU Reference              | (0010,xx10) | UR | https://doi.org/10.1210/jendso/bvab048.1607                       |
|                                      | Person Names to Use Sequence  | (0010,xxx3) | SQ |                                                                   |
| PID-5 Name Type Code = Name to Use   | \>Name to use                 | (0010,xx12) | LT | Smith, John                                                       |
|                                      | \>Validity Period Sequence    | (0010,xxx5) | SQ |                                                                   |
|                                      | \>Start DateTime              | (0010,xxx6) | DT | 20220715090000                                                    |
|                                      | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
|                                      | \>Name to Use Comment         | (0010,xx13) | LT |                                                                   |
| PID-8 (based on local mapping rules) | Sex at Birth Code Sequence    | (0010,xx25) | CS |                                                                   |
|                                      | \>\>Code Value                | (0008,0100) | SH | 248152002                                                         |
|                                      | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
|                                      | \>\>Code Meaning              | (0008,0104) | LO | Female                                                            |

#### Example 02: Patient Name Update

See these [examples](v2dicom_use_case.html#registration-of-name-change)of HL7 v2.9.1 and v2.5 ADT Demographics Updates from [arrival and check-in](#arrival-and-check-in).
Note: in previous v2 versions, the first occurrence indicated the legal name. In this case, Name to Use name is listed first for legacy compatibility.


These map to DICOM Modality Worklist as follows:

| V2                                   | Attribute Name                | Tag         | VR | Value                                                             |
| ------------------------------------ | ----------------------------- | ----------- | -- | ----------------------------------------------------------------- |
| PID-5 Name Type Code = Name to Use   | Patient's Name                | (0010,0010) | PN | Smith\^John^^^                                                    |
| PID-8                                | Patient's Sex                 | (0010,0040) | CS | F                                                                 |
|                                      | Patient’s Gender              | (0010,xxxx) | CS | M                                                                 |
|                                      | Gender Identity Sequence      | (0010,xxxx) | SQ |                                                                   |
|                                      | \>Gender Code Sequence        | (0010,xxx4) | SQ |                                                                   |
| GSP-5-1                              | \>\>Code Value                | (0008,0100) | SH | 446151000124109                                                   |
| GSP-5-3                              | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| GSP-5-2                              | \>\>Code Meaning              | (0008,0104) | LO | Identifies as male gender                                         |
|                                      | \>Start DateTime              | (0010,xxx6) | DT | 20220715010000                                                    |
|                                      | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
|                                      | \>Gender Comment              | (0010,xxx8) | LT |                                                                   |
|                                      | Sex Parameters for Clinical Use Sequence | (0010,xxx2) | SQ |                                                        |
|                                      | \>SPCU Code  Sequence         | (0010,xxx9) | SQ |                                                                   |
| GSC-4-1                              | \>\>Code Value                | (0008,0100) | SH | Male-typical                                                      |
| GSC-4-3                              | \>\>Coding Scheme Designator  | (0008,0102) | SH | SexParameterForClinicalUse                                               |
| GSC-4-2                              | \>\>Code Meaning              | (0008,0104) | LO | Male typical parameters                                           |
|                                      | \>Start DateTime              | (0010,xxx6) | DT |                                                                   |
|                                      | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
| GSC-8                                | \>SPCU Comment                | (0010,xxx1) | LT | Hormonal treatment, use affirmed gender Cr reference ranges       |
|                                      | \>SPCU Reference              | (0010,xx10) | UR | https://doi.org/10.1210/jendso/bvab048.1607                       |  
|                                      | Person Names to Use Sequence  | (0010,xxx3) | SQ |                                                                   |
|                                      | \>Name to use                 | (0010,xx12) | LT |                                                                   |
|                                      | \>Validity Period Sequence    | (0010,xxx5) | SQ |                                                                   |
|                                      | \>\>Start DateTime            | (0010,xxx6) | DT |                                                                   |
|                                      | \>\>Stop DateTime             | (0010,xxx7) | DT |                                                                   |
|                                      | \>Name to Use Comment         | (0010,xx13) | LT |                                                                   |
| PID-8 (based on local mapping rules) | Sex at Birth Code Sequence    | (0010,xx25) | CS |                                                                   |
|                                      | \>\>Code Value                | (0008,0100) | SH | 248152002                                                         |
|                                      | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
|                                      | \>\>Code Meaning              | (0008,0104) | LO | Female                                                            |

#### Example 03: FHIR Mapping

The patient is referenced as the subject of [DiagnosticReport](#reporting), DocumentReference, ImagingStudy or ImagingSelection. Mapping to DICOM is as follows:

| FHIR attribute                                | Attribute Name                | TAG         | VR | Value                                                             |
| ----------------------------------------------| ----------------------------- | ----------- | -- | ----------------------------------------------------------------- |
| Patient.name.use=official                     | Patient's Name                | (0010,0010) | PN | Smith\^John^^^                                                    |
| Patient.gender                                | Patient's Sex                 | (0010,0040) | CS | F                                                                 |
|                                               | Gender Identity Sequence      | (0010,xxxx) | SQ |                                                                   |
|                                               | \>Gender Code                 | (0010,xxx4) | SQ |                                                                   |
| Patient.genderIdentity.code                   | \>\>Code Value                | (0008,0100) | SH | 446151000124109                                                   |
| Patient.genderIdentity.system                 | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| Patient.genderIdentity.display                | \>\>Code Meaning              | (0008,0104) | LO | Identifies as male gender                                         |
| Patient.genderIdentity.period.start           | \>Start DateTime              | (0010,xxx6) | DT | 20220715010000                                                    |
| Patient.genderIdentity.period.end             | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
|                                               | \>Gender Comment              | (0010,xxx8) | LT |                                                                   |
|                                               | Sex Parameters for Clinical Use Sequence | (0010,xxx2) | SQ |                                                        |
|                                               | \>SPCU Code Sequence          | (0010,xxx9) | SQ |                                                                   |
| serviceRequest.sexForClinicalUse.code         | \>\>Code Value                | (0008,0100) | SH | 248153007                                                         |
| serviceRequest.sexForClinicalUse.system       | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| serviceRequest.sexForClinicalUse.display      | \>\>Code Meaning              | (0008,0104) | LO | Male                                                              |
| serviceRequest.sexForClinicalUse.period       | \>Validity Period sequence    | (0010,xxx5) | SQ |                                                                   |
| serviceRequest.sexForClinicalUse.period.start | \>\>Start DateTime            | (0010,xxx6) | DT | 20220715090000                                                    |
| serviceRequest.sexForClinicalUse.period.end   | \>\>Stop DateTime             | (0010,xxx7) | DT |                                                                   |
| serviceRequest.sexForClinicalUse.comment      | \>SPCU Comment                | (0010,xxx1) | LT | Hormonal treatment, use affirmed gender Cr reference ranges       |
|                                               | \>SPCU Reference              | (0010,xx10) | UR | https://doi.org/10.1210/jendso/bvab048.1607                       | 
|                                               | Person Names to Use Sequence  | (0010,xxx3) | SQ |                                                                   |
| Patient.name[use=usual]                       | \>Name to use                 | (0010,xx12) | LT | John Smith                                                        |
|                                               | \>Validity Period Sequence    | (0010,xxx5) | SQ |                                                                   |
|                                               | \>\>Start DateTime            | (0010,xxx6) | DT |                                                                   |
|                                               | \>\>Stop DateTime             | (0010,xxx7) | DT |                                                                   |
|                                               | \>Name to Use Comment         | (0010,xx13) | LT |                                                                   |
| Note: based on local mapping                  | Sex at Birth Code Sequence    | (0010,xx25) | CS | F                                                                 |
| Patient.gender.code                           | \>\>Code Value                | (0008,0100) | SH | 248152002                                                         |
| Patient.gender.system                         | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| Patient.gender.display                        | \>\>Code Meaning              | (0008,0104) | LO | Female                                                            |

#### Example 04: Imaging Report

See these [examples](v2dicom_use_case.html#result-for-imaging) of HL7 v2.9.1 and v2.5 Unsolicited Observation Results containing the narrative from the final [Imaging Report](#reporting).


*OBX Segments containing Imaging Report Narrative omitted for brevity*

#### Example 05: CDA Release 2 Imaging Report

See this [example](cdadicom_use_case.html#cda-dicom) of a CDA [Imaging Report](#reporting).
