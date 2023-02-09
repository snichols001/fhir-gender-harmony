**TODO**
Streamline storyboard based on SWF.b. add reference to SWF.b vol 1
Clean up mapping:
1. v2 to DICOM
2. FHIR to DICOM (help from WG-20)
3. Represent FHIR extensions in mapping tables as they would appear in Sushi
4. Should these maps be exposed in other ways, in a resource or profile or a conceptmap? (i.e. "computable version", something more formal)

### Introduction

This use case illustrates DICOM Sex and Gender encoding, including: admission, patient prep, examination, post processing and reporting for a PET/CT examination order. In this case, there are three instances of Sex For Clinical Use (SFCU). First, the ordering physician provides instructions for interpreting lab values within a Sex for Clinical Use comment. In the second SFCU, a post-processing AI (Artifical Intelligence) application utilizes the Sex Assigned at Birth for the basis of reference values. Third, the radiologist determines the appropriate sex to use based on the patient's body composition for a Standard Uptake Value (SUV) calculation. A patient with EHR Sex for Clinical Use (SFCU) of “female” and a EHR Gender Identity of “male” checks-in for a PET/CT examination. The examination is performed, the patient’s demographics are updated, and the report is delivered. The DICOM (Digital Imaging and COmmunications in Medicine) Standard attributes in this use case are not, at time of publication of this Implementation Guide, normative, and details in DICOM are still being defined. Readers interested in participating in development of DICOM Sex and Gender encoding, please contact the [DICOM Secretariat](mailto:dicom@dicomstandard.org).

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

![](gh-dicom-2.svg)

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

8. The radiologist notes that the provided SFCU of Female, is not consistent with the SFCU Comment and calls the ordering physician to confirm.

8.  After discussing patient history with the ordering physician, the radiologist provides protocol alterations based on the  patient’s transgender status.

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

1.  The radiologist creates an SUV ROI on the PACS. The application detects the presence of Patient’s Sex (0010,0040) “F”, a Gender Code (0010,xxx4).(0008,0104) “Male” and Sex Assigned at Birth (0010,xx25) of “F” and prompts the radiologist to enter a value “M” or “F”.

2.  The Dose Information Reporter collects the RDSR, without exception.

3.  The AI task performer detects the Sex Assigned at Birth (0010,xx25) of “F”, the algorithm processes the images based on female reference values and transfers evidence documents to the PACS.

Note: Sex at Birth is required to determine reference values for AI and non-AI machine algorithims in various domains, such as cardiology and neurology.

#### Reporting

1.  The radiologist dictates findings pertaining to the procedure, noting scanner and contrast protocol modifications in the “Request” section of the report.

2.  The report format has been configured to include Patient’s Sex (0010,0040), Patient’s Gender Code (0010,xxx4).(0008,0104), Patient Name (0010,xxx3).(0010,xx12) and SFCU Comment (0010,xxx1) in the report.

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

Below is a HL7 v2.9.1 OMI Imaging Order from [precondition(s)](#preconditions).

```
MSH|^~\&|||||20220715090000||OMI^O23|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.9.1
PID|||patientID^^^^MR||Smith^Janet^^^^^B~Smith^John^^^^^N|||F
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Male^SCT  
GSC|1|S||248152002^Female^SCT||OBR^4||Hormonal treatment, use affirmed gender Cr reference ranges
ORC|NW
OBR||||241439007^PET heart study^SCT|||||||||||||||||||||||||||||||||||||||||82800-4^PET+CT Heart W contrast IV^LN
IPC|accessionNum|procedureID|studyInstanceUID|schProcedureStepId|PT^Positron emission tomography^DCM|122793^PET Myocardial Perfusion, Rest and Stress^DCM
```
Below is a HL7 v2.5 OMI Imaging Order using the pre-adoption profile
```
MSH|^~\&|||||20220715142240||OMI^O23|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.5|||||||||GenderHarmony^^2.16.840.1.113883.9.282^ISO
PID|||patientID^^^^MR||Smith^Janet^^^^^B~Smith^John^^^^^N|||F
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Male^SCT
GSC|1|S||248152002^Female^SCT||OBR^4||Hormonal treatment, use affirmed gender Cr reference ranges
ORC|NW
OBR||||241439007^PET heart study^SCT|||||||||||||||||||||||||||||||||||||||||82800-4^PET+CT Heart W contrast IV^LN
IPC|accessionNum|procedureID|studyInstanceUID|schProcedureStepId|PT^Positron emission tomography^DCM|122793^PET Myocardial Perfusion, Rest and Stress^DCM
```

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
|                                      | Sex for Clinical Use Sequence | (0010,xxx2) | SQ |                                                                   |
|                                      | \>SFCU Code Sequence          | (0010,xxx9) | SQ |                                                                   |
| GSC-4-1                              | \>\>Code Value                | (0008,0100) | SH | 248153007                                                         |
| GSC-4-3                              | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| GSC-4-2                              | \>\>Code Meaning              | (0008,0104) | LO | Male                                                              |
|                                      | \>Start DateTime              | (0010,xxx6) | DT | 20220715090000                                                    |
|                                      | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
| GSC-8                                | \>SFCU Comment                | (0010,xxx1) | LT | Hormonal treatment, use affirmed gender Cr reference ranges       |
|                                      | \>SFCU Reference              | (0010,xx10) | UR | https://doi.org/10.1210/jendso/bvab048.1607                       |
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

Below is a HL7 v2.9.1 ADT Demographics Update from [arrival and check-in](#arrival-and-check-in).

```
MSH|^~\&|||||20220715010000||ADT^A08|TwxxneTRWE9JGX4U2p3h|P|2.9.1
EVN||20220715151118||01
PID|||patientID^^^^MR||Smith^John^^^N~Smith^Janet^^^B~Smith^Janet^^^^NOUSE|||F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Male^SCT|20220715
PV1||O
```
Below is a HL7 v2.5 ADT Demographics Update using the pre-adoption profile
```
MSH|^~\&|||||20220715151029||ADT^A08|TwxxneTRWE9JGX4U2p3hCLJH|P|2.5|||||||||GenderHarmony^^2.16.840.1.113883.9.282^ISO
EVN||20220715151118||01^Patient Request^HL70062
PID|||patientID^^^^MR||Smith^John^^^N~Smith^Janet^^^B~Smith^Janet^^^^NOUSE|||F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Male^SCT|20220715
PV1||O
```
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
|                                      | Sex for Clinical Use Sequence | (0010,xxx2) | SQ |                                                                   |
|                                      | \>SFCU Code  Sequence         | (0010,xxx9) | SQ |                                                                   |
| GSC-4-1                              | \>\>Code Value                | (0008,0100) | SH | 248153007                                                         |
| GSC-4-3                              | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| GSC-4-2                              | \>\>Code Meaning              | (0008,0104) | LO | Male                                                              |
|                                      | \>Start DateTime              | (0010,xxx6) | DT |                                                                   |
|                                      | \>Stop DateTime               | (0010,xxx7) | DT |                                                                   |
| GSC-8                                | \>SFCU Comment                | (0010,xxx1) | LT | Hormonal treatment, use affirmed gender Cr reference ranges       |
|                                      | \>SFCU Reference              | (0010,xx10) | UR | https://doi.org/10.1210/jendso/bvab048.1607                       |                                                           |
|                                      | Person Names to Use Sequence  | (0010,xxx3) | SQ |                                                                   |
|                                      | \>Name to use                 | (0010,xx12) | LT |                                                                   |
|                                      | \>Validity Period Sequence    | (0010,xxx5) | SQ |                                                                   |
|                                      | \>\>Start DateTime            | (0010,xxx6) | DT | 20220715009000                                                    |
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
|                                               | Sex for Clinical Use Sequence | (0010,xxx2) | SQ |                                                                   |
|                                               | \>SFCU Code Sequence          | (0010,xxx9) | SQ |                                                                   |
| serviceRequest.sexForClinicalUse.code         | \>\>Code Value                | (0008,0100) | SH | 248153007                                                         |
| serviceRequest.sexForClinicalUse.system       | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| serviceRequest.sexForClinicalUse.display      | \>\>Code Meaning              | (0008,0104) | LO | Male                                                              |
| serviceRequest.sexForClinicalUse.period       | \>Validity Period sequence    | (0010,xxx5) | SQ |                                                                   |
| serviceRequest.sexForClinicalUse.period.start | \>\>Start DateTime            | (0010,xxx6) | DT | 20220715090000                                                    |
| serviceRequest.sexForClinicalUse.period.end   | \>\>Stop DateTime             | (0010,xxx7) | DT |                                                                   |
| serviceRequest.sexForClinicalUse.comment      | \>SFCU Comment                | (0010,xxx1) | LT | Hormonal treatment, use affirmed gender Cr reference ranges       |
|                                               | \>SFCU Reference              | (0010,xx10) | UR | https://doi.org/10.1210/jendso/bvab048.1607                       |                                                          |
|                                               | Person Names to Use Sequence  | (0010,xxx3) | SQ |                                                                   |
| Patient.name.use=usual                        | \>Name to use                 | (0010,xx12) | LT | John Smith                                                        |
|                                               | \>Validity Period Sequence    | (0010,xxx5) | SQ |                                                                   |
|                                               | \>\>Start DateTime            | (0010,xxx6) | DT |                                                                   |
|                                               | \>\>Stop DateTime             | (0010,xxx7) | DT |                                                                   |
|                                               | \>Name to Use Comment         | (0010,xx13) | LT |                                                                   |
| Note: based on local mapping                  | Sex at Birth Code Sequence    | (0010,xx25) | CS | F                                                                 |
| Patient.gender.code                           | \>\>Code Value                | (0008,0100) | SH | 248152002                                                         |
| Patient.gender.system                         | \>\>Coding Scheme Designator  | (0008,0102) | SH | SCT                                                               |
| Patient.gender.display                        | \>\>Code Meaning              | (0008,0104) | LO | Female                                                            |

#### Example 04: Imaging Report

Below is a HL7 v2.9.1 Unsolicited Observation Result containing the narrative from the final [Imaging Report](#reporting).

```
MSH|^~\&|||||20220715142240||ORU^R01|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.9.1
PID|||patientID^^^^MR||Smith^John^^^^^N|||F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Male^SCT
GSC|1|S||248152002^Female^SCT||OBR^4||Hormonal treatment, use affirmed gender Cr reference ranges
ORC|CN||||||||20220715090000|||^Cure^Matt^^^^MD
OBR|1|accessionNum||82800-4^PET+CT Heart W contrast IV^LN|||20220715110000||||
OBX|1|CWE|55111-9^Current Imaging Procedure Description^LN||Imaging technique (protocol, contrast, radiotracer) described here||||||F|
OBX|2|CWE|19005-8^Impression^LN||Report narrative goes here||||||F|
OBX|3|CWE|55110-1^Conclusion^LN||Conclusion goes here||||||F|
```
Below is a HL7 v2.5 Unsolicited Observation Result using the pre-adoption profile
```
MSH|^~\&|||||20220715142240||ORU^R01^ORU_R01|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.5|||||||||GenderHarmony^^2.16.840.1.113883.9.282^ISO
PID|||patientID^^^^MR||Smith^John^^^^^N|||F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Male^SCT
GSC|1|S||248152002^Female^SCT||OBR^4||Hormonal treatment, use affirmed gender Cr reference ranges
OBR|1|accessionNum||82800-4^PET+CT Heart W contrast IV^LN|||20220715142240||||
OBX|1|CWE|55111-9^Current Imaging Procedure Description^LN||Imaging technique (protocol, contrast, radiotracer) described here||||||F|
OBX|2|FT|19005-8^Impression^LN||Report narrative goes here||||||F|
OBX|3|TX|55110-1^Conclusion^LN||Conclusion goes here||||||F|
```

*OBX Segments containing Imaging Report Narrative omitted for brevity*

#### Example 05: CDA Release 2 Imaging Report
Below is a CDA [Imaging Report](#reporting) example.

```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="CDA.xsl"?>
<!--
	Title: Sex for Clinical Use CDA Template Example file
	Version: 1.0
	Revision History: 
	31-Jan-2011 source document created
	19-Aug-2022 example drafted [Jay Lyle, JP Systems for the VHA; 
								Rob Horn, Fairhaven Technology; 
								Steven Nichols, GE Healthcare]
-->

<ClinicalDocument xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="urn:hl7-org:v3"
	xmlns:mif="urn:hl7-org:v3/mif" xmlns:voc="urn:hl7-org:v3/voc"
	xsi:schemaLocation="urn:hl7-org:v3 CDA.xsd">
	<!-- 	

********************************************************
  CDA Header
********************************************************
	-->

	<realmCode code="US"/>
	<typeId root="2.16.840.1.113883.1.3" extension="POCD_HD000040"/>
	<!-- US General Header Template -->
	<templateId root="2.16.840.1.113883.10.20.22.1.1"/>
	<!-- Diagnostic Imaging Report Template -->
	<templateId root="2.16.840.1.113883.10.20.22.1.5"/>
	<id root="2.16.840.1.113883.19.4.27" extension="20060828170821659"/>
	<code code="18748-4" codeSystem="2.16.840.1.113883.6.1" codeSystemName="LOINC"
		displayName="Diagnostic Imaging Report"/>
	<title>PET/CT Heart W contrast IV</title>
	<effectiveTime value="20050329171504+0500"/>
	<confidentialityCode code="N" codeSystem="2.16.840.1.113883.5.25"/>
	<languageCode code="en-US"/>
	<setId extension="111199021" root="2.16.840.1.113883.19"/>
	<versionNumber value="1"/>
	<recordTarget>
		<!--NEW CONF per base CDA - patientRole SHALL be present of [1..*]-->
		<patientRole>
			<id extension="12345" root="2.16.840.1.113883.19.5"/>
			<addr use="HP">
				<streetAddressLine>3300 Washtenaw Ave # 227</streetAddressLine>
				<city>Ann Arbor</city>
				<state>MI</state>
				<postalCode>48104</postalCode>
				<country>USA</country>
			</addr>
			<telecom value="tel:(734)555-1212" use="HP"/>
			<patient>
				<name use="L">
					<given>Smith</given>
					<family>John</family>
				</name>
				<administrativeGenderCode code="F" codeSystem="2.16.840.1.113883.5.1"/>
				<birthTime value="19541125"/>
				<maritalStatusCode code="S" displayName="Single" codeSystem="2.16.840.1.113883.5.2"
					codeSystemName="MaritalStatusCode"/>
				<religiousAffiliationCode code="1013" displayName="Christian"
					codeSystemName="HL7 Religious Affiliation "
					codeSystem="2.16.840.1.113883.5.1076"/>
				<raceCode code="2106-3" displayName="White" codeSystem="2.16.840.1.113883.6.238"
					codeSystemName="Race &amp; Ethnicity - CDC"/>
				<ethnicGroupCode code="2186-5" displayName="Not Hispanic or Latino"
					codeSystem="2.16.840.1.113883.6.238" codeSystemName="Race &amp; Ethnicity - CDC"/>
				<guardian>
					<code code="GRFTH" displayName="Grandfather"
						codeSystem="2.16.840.1.113883.5.111" codeSystemName="HL7 Role code"/>
					<addr use="HP">
						<streetAddressLine>3300 Washtenaw Ave # 227</streetAddressLine>
						<city>Ann Arbor</city>
						<state>MI</state>
						<postalCode>48104</postalCode>
						<country>USA</country>
					</addr>
					<telecom value="tel:(734)555-1212" use="HP"/>
					<guardianPerson>
						<name>
							<given>Ralph</given>
							<family>Relative</family>
						</name>
					</guardianPerson>
				</guardian>
				<birthplace>
					<place>
						<addr>
							<state>MI</state>
							<postalCode>48104</postalCode>
							<country>USA</country>
						</addr>
					</place>
				</birthplace>
				<languageCommunication>
					<languageCode code="fr-CN"/>
					<modeCode code="RWR" displayName="Recieve Written"
						codeSystem="2.16.840.1.113883.5.60" codeSystemName="LanguageAbilityMode"/>
					<preferenceInd value="true"/>
				</languageCommunication>
			</patient>
			<providerOrganization>
				<id root="2.16.840.1.113883.19.5"/>
				<name>Good Imaging Clinic</name>
				<telecom value="tel:(734)555-1212"/>
				<addr>
					<streetAddressLine>21 North Ave</streetAddressLine>
					<city>Ann Arbor</city>
					<state>MI</state>
					<postalCode>48104</postalCode>
					<country>USA</country>
				</addr>
			</providerOrganization>
		</patientRole>
	</recordTarget>
	<author>
		<time value="20050329224411+0500"/>
		<assignedAuthor>
			<id extension="KP00017" root="2.16.840.1.113883.19.5"/>
			<addr>
				<streetAddressLine>21 North Ave.</streetAddressLine>
				<city>Ann Arbor</city>
				<state>MI</state>
				<postalCode>48104</postalCode>
				<country>USA</country>
			</addr>
			<telecom value="tel:(555)555-1003"/>
			<assignedPerson>
				<name>
					<given>Bill</given>
					<family>Roentgen</family>
				</name>
			</assignedPerson>
		</assignedAuthor>
	</author>
	<dataEnterer>
		<assignedEntity>
			<id root="2.16.840.1.113883.19.5" extension="43252"/>
			<addr>
				<streetAddressLine>21 North Ave.</streetAddressLine>
				<city>Ann Arbor</city>
				<state>MI</state>
				<postalCode>48104</postalCode>
				<country>USA</country>
			</addr>
			<telecom value="tel:(555)555-1003"/>
			<assignedPerson>
				<name>
					<given>Bill</given>
					<family>Roentgen</family>
				</name>
			</assignedPerson>
		</assignedEntity>
	</dataEnterer>
	<custodian>
		<assignedCustodian>
			<representedCustodianOrganization>
				<id root="2.16.840.1.113883.19.5"/>
				<name>Good Imaging Clinic</name>
				<telecom value="tel:(555)555-1212" use="WP"/>
				<addr use="HP">
					<streetAddressLine>3300 Washtenaw Ave # 227</streetAddressLine>
					<city>Ann Arbor</city>
					<state>MI</state>
					<postalCode>48104</postalCode>
					<country>USA</country>
				</addr>
			</representedCustodianOrganization>
		</assignedCustodian>
	</custodian>
	<informationRecipient>
		<intendedRecipient>
			<informationRecipient>
				<name>
					<given>Bill</given>
					<family>Roentgen</family>
				</name>
			</informationRecipient>
			<receivedOrganization>
				<name>Good Imaging Clinic</name>
			</receivedOrganization>
		</intendedRecipient>
	</informationRecipient>
	<legalAuthenticator>
		<time value="20050329224411+0500"/>
		<signatureCode code="S"/>
		<assignedEntity>
			<id extension="KP00017" root="2.16.840.1.113883.19.5"/>
			<addr>
				<streetAddressLine>21 North Ave.</streetAddressLine>
				<city>Ann Arbor</city>
				<state>MI</state>
				<postalCode>48104</postalCode>
				<country>USA</country>
			</addr>
			<telecom value="tel:(555)555-1003"/>
			<assignedPerson>
				<name>
					<given>Bill</given>
					<family>Roentgen</family>
				</name>
			</assignedPerson>
		</assignedEntity>
	</legalAuthenticator>
	<authenticator>
		<time value="20050329224411+0500"/>
		<signatureCode code="S"/>
		<assignedEntity>
			<id extension="KP00017" root="2.16.840.1.113883.19.5"/>
			<addr>
				<streetAddressLine>21 North Ave.</streetAddressLine>
				<city>Ann Arbor</city>
				<state>MI</state>
				<postalCode>48104</postalCode>
				<country>USA</country>
			</addr>
			<telecom value="tel:(555)555-1003"/>
			<assignedPerson>
				<name>
					<given>Bill</given>
					<family>Roentgen</family>
				</name>
			</assignedPerson>
		</assignedEntity>
	</authenticator>
	<inFulfillmentOf>
		<order>
			<id extension="10523475" root="1.2.840.113619.2.62.994044785528.27"/>
			<code code="121022" codeSystem="1.2.840.10008.2.16.4" codeSystemName="DCM"
				displayName="Accession Number"/>
		</order>
	</inFulfillmentOf>
	<documentationOf>
		<serviceEvent classCode="ACT">
			<id root="1.2.840.113619.2.62.994044785528.114289542805"/>
			<!-- study instance UID -->
			<code code="78814"
				displayName="Positron emission tomography (PET) with concurrently acquired computed tomography (CT)"
				codeSystem="2.16.840.1.113883.6.12" codeSystemName="CPT4"/>
			<effectiveTime value="20060823222400"/>
			<performer typeCode="PRF">
				<templateId root="2.16.840.1.113883.10.20.6.2.1"/>
				<assignedEntity>
					<id extension="121008" root="2.16.840.1.113883.19.5"/>
					<code code="2085R0202X" codeSystem="2.16.840.1.113883.11.19465"
						codeSystemName="NUCC" displayName="Diagnostic Radiology"/>
					<addr nullFlavor="NI"/>
					<telecom nullFlavor="NI"/>
					<assignedPerson>
						<name>
							<given>Matt</given>
							<family>Cure</family>
							<suffix>MD</suffix>
						</name>
					</assignedPerson>
				</assignedEntity>
			</performer>
		</serviceEvent>
	</documentationOf>
	<relatedDocument typeCode="XFRM">
		<parentDocument>
			<id root="1.2.840.113619.2.62.994044785528.20060823.200608232232322.9"/>
			<!-- SOP Instance UID (0008,0018) -->
		</parentDocument>
	</relatedDocument>
	<componentOf>
		<encompassingEncounter>
			<id extension="9937012" root="1.3.6.4.1.4.1.2835.12"/>
			<effectiveTime value="20060828170821"/>
			<encounterParticipant typeCode="ATND">
				<templateId root="2.16.840.1.113883.10.20.6.2.2"/>
				<assignedEntity>
					<id extension="4" root="2.16.840.1.113883.19"/>
					<code code="208D00000X" codeSystem="2.16.840.1.113883.11.19465"
						codeSystemName="NUCC" displayName="General Practice"/>
					<addr nullFlavor="NI"/>
					<telecom nullFlavor="NI"/>
					<assignedPerson>
						<name>
							<prefix>Dr.</prefix>
							<given>Fay</given>
							<family>Family</family>
						</name>
					</assignedPerson>
				</assignedEntity>
			</encounterParticipant>
		</encompassingEncounter>
	</componentOf>
	<component>
		<structuredBody>
			<component>
				<!--
**********************************************************************
                     DICOM Object Catalog Section
**********************************************************************
-->
				<section classCode="DOCSECT" moodCode="EVN">
					<templateId root="2.16.840.1.113883.10.20.6.1.1"/>
					<code code="121181" codeSystem="1.2.840.10008.2.16.4" codeSystemName="DCM"
						displayName="DICOM Object Catalog"/>
					<entry>
						<!--	
**********************************************************************
                     Study
**********************************************************************
-->
						<act classCode="ACT" moodCode="EVN">
							<templateId root="2.16.840.1.113883.10.20.6.2.6"/>
							<id root="1.2.840.113619.2.62.994044785528.114289542805"/>
							<code code="113014" codeSystem="1.2.840.10008.2.16.4"
								codeSystemName="DCM" displayName="Study"/>
							<!--		 
			        *****************************************************************
                      Series and SopInstance UIDs removed for brevity
                    *****************************************************************
 -->
						</act>
					</entry>
				</section>
				<!--							
**********************************************************************
                    End of  DICOM Object Catalog Section
**********************************************************************
-->
			</component>
			<component>
				<!--
**********************************************************************
Social History Section 
**********************************************************************
-->
				<section>
					<templateId root="2.16.840.1.113883.10.20.22.2.17" extension="2015-08-01"/>
					<templateId root="2.16.840.1.113883.10.20.22.2.17"/>
					<code code="29762-2" codeSystem="2.16.840.1.113883.6.1" codeSystemName="LOINC"/>
					<title>Social History</title>
					<text> The patient was born female, identifes as male and is currently
						undergoing gender affirming hormone therapy. <table border="1" width="100%"
							cellpadding="0" cellspacing="0">
							<thead>
								<tr>
									<th>Obs</th>
									<th>Value</th>
									<th>Kind</th>
									<th>Jurisdiction</th>
									<th>Date Acquired</th>
								</tr>
							</thead>
							<tbody>
								<tr>
									<td>Gender Identity</td>
									<td>Identifies as male gender</td>
									<td/>
									<td/>
									<td/>
								</tr>
								<tr>
									<td>Pronouns</td>
									<td>He, Him, His, Himself</td>
									<td/>
									<td/>
									<td/>
								</tr>
								<tr>
									<td>Recorded Sex or Gender</td>
									<td>Female</td>
									<td>Sex Assigned at Birth</td>
									<td>California</td>
									<td>201201011450+0600</td>
								</tr>
							</tbody>
						</table>
					</text>
					<entry>
						<!-- Gender Identity -->
						<observation classCode="OBS" moodCode="EVN">
							<templateId root="2.16.840.1.113883.10.15.1" extension="2022-09-01"/>
							<code code="76691-5" codeSystem="2.16.840.1.113883.6.1"
								displayName="Gender Identity"/>
							<statusCode code="completed"/>
							<value xsi:type="CD" codeSystem="2.16.840.1.113883.6.96"
								codeSystemName="SNOMED CT" code="446151000124109"
								displayName="Identifies as male gender"> </value>
						</observation>
					</entry>
					<entry>
						<!-- Pronouns -->
						<observation classCode="OBS" moodCode="EVN">
							<templateId root="2.16.840.1.113883.10.15.2" extension="2022-09-01"/>
							<code code="90778-2" codeSystem="2.16.840.1.113883.6.1"
								displayName="Personal pronouns"/>
							<statusCode code="completed"/>
							<value xsi:type="CD" codeSystem="2.16.840.1.113883.6.1"
								codeSystemName="LOINC" code="LA29518-0"
								displayName="He, Him, His, Himself"/>
						</observation>
					</entry>
					<entry>
						<!-- Recorded Sex or Gender -->
						<observation classCode="OBS" moodCode="EVN">
							<templateId root="2.16.840.1.113883.10.15.4" extension="2022-09-01"/>
							<code code="99502-7" codeSystem="2.16.840.1.113883.6.1"
								displayName="Recorded sex or gender"/>
							<derivationExpr>Enter whether the infant is male, female, or if the sex
								of the infant is ambiguous, enter “unknown.”</derivationExpr>
							<statusCode code="completed"/>
							<value xsi:type="CD" code="F">
								<translation code="F" codeSystem="2.16.840.1.113883.11.19756"
									codeSystemName="International Civil Aviation Organization Sex or Gender"
									displayName="Female"/>
							</value>
							<entryRelationship typeCode="COMP">
								<!-- Jurisdiction -->
								<observation classCode="OBS" moodCode="EVN">
									<code code="77969-4" codeSystem="2.16.840.1.113883.6.1"
										displayName="Jurisdiction code"/>
									<statusCode code="completed"/>
									<value xsi:type="CD" nullFlavor="OTH" codeSystem="NP">
										<!-- This may be coded but does not have to be -->
										<originalText>California</originalText>
									</value>
								</observation>
							</entryRelationship>
							<entryRelationship typeCode="COMP">
								<!-- Document Type -->
								<observation classCode="OBS" moodCode="EVN">
									<code code="92183-3" codeSystem="2.16.840.1.113883.6.1"
										displayName="Document type"/>
									<statusCode code="completed"/>
									<value xsi:type="CD" codeSystem="2.16.840.1.113883.6.1"
										codeSystemName="LOINC" code="71230-7"
										displayName="Birth Certificate"/>
								</observation>
							</entryRelationship>
							<entryRelationship typeCode="COMP">
								<!-- Date of Entry -->
								<observation classCode="OBS" moodCode="EVN">
									<code code="50786-3" codeSystem="2.16.840.1.113883.6.1"
										displayName="Date of entry"/>
									<statusCode code="completed"/>
									<value xsi:type="TS" value="201201011450+0600"/>
								</observation>
							</entryRelationship>
						</observation>
					</entry>
				</section>
				<!--							
**********************************************************************
End of Social History Section
**********************************************************************
-->
			</component>
			<component>
				<!--
**********************************************************************
Reason for study Section 
**********************************************************************
-->
				<section>
					<code code="121109" codeSystem="1.2.840.10008.2.16.4" codeSystemName="DCM"
						displayName="Indications for Procedure"/>
					<title>Indications for Procedure</title>
					<text>Discordant clinical, ECG, and myocardial perfusion SPECT results</text>
				</section>
				<!--							
**********************************************************************
End of Reason for study Section
**********************************************************************
-->
			</component>
			<component>
				<!--
**********************************************************************
                   History Section 
**********************************************************************
-->
				<section>
					<code code="11329-0" codeSystem="2.16.840.1.113883.6.1" codeSystemName="LOINC"
						displayName="History"/>
					<title>History</title>
					<text>
						<paragraph>
							<caption>History</caption>
							<content ID="Fndng1">History goes here...</content>
						</paragraph>
					</text>
					<entry>
						<!-- History report element (TEXT) -->
						<observation classCode="OBS" moodCode="EVN">
							<templateId root="2.16.840.1.113883.10.20.6.2.12"/>
							<code code="121060" codeSystem="1.2.840.10008.2.16.4"
								codeSystemName="DCM" displayName="History"/>
							<value xsi:type="ED"> History text </value>
						</observation>
					</entry>
					<!--*****************************************************************************
This Substance Administration is supporting information for Sex for Clinical Use. It 
is not likely to appear in a PET/CT report but is provided to illustrate the use of 
the supporting reference.
*****************************************************************************-->
					<entry>
						<substanceAdministration classCode="SBADM" moodCode="EVN">
							<!-- ** Medication Activity (V2) ** -->
							<templateId root="2.16.840.1.113883.10.20.22.4.16"
								extension="2014-06-09"/>
							<id root="6C844C75-AA34-411C-B7BD-5E4A9F206E29"/>
							<statusCode code="active"/>
							<effectiveTime xsi:type="IVL_TS">
								<low value="20120318"/>
							</effectiveTime>
							<doseQuantity value="1"/>
							<consumable>
								<manufacturedProduct classCode="MANU">
									<!-- ** Medication information ** -->
									<templateId root="2.16.840.1.113883.10.20.22.4.23"
										extension="2014-06-09"/>
									<id root="2a620155-9d11-439e-92b3-5d9815ff4ee8"/>
									<manufacturedMaterial>
										<code code="403922"
											displayName="168 HR estradiol 0.00156 MG/HR Transdermal System"
											codeSystem="2.16.840.1.113883.6.88"
											codeSystemName="RxNorm"/>
									</manufacturedMaterial>
								</manufacturedProduct>
							</consumable>
						</substanceAdministration>
					</entry>
				</section>
			</component>
			<!--							
**********************************************************************
                    End of History Section
**********************************************************************
-->
			<!--
**********************************************************************
                     Imaging Procedure Description Section
**********************************************************************
-->
			<component>
				<section classCode="DOCSECT" moodCode="EVN">
					<templateId root="1.2.840.10008.9.3"/>
					<id root="1.2.840.10213.2.62.9940434234785528.11428954534542805"/>
					<code code="55111-9" codeSystem="2.16.840.1.113883.6.1" codeSystemName="LOINC"
						displayName="Current Imaging Procedure Description"/>
					<title>Imaging Procedure Description</title>
					<text>
						<table border="1" width="100%" cellpadding="0" cellspacing="0">
							<tbody>
								<tr>
									<td>Sex For Clinical Use</td>
									<td>Female</td>
								</tr>
								<tr>
									<td>Imaging Technique</td>
									<td>The patient is a transgender male, undergoing hormonal
										treatment. Based on physician instructions, affirmed gender
										creatinine reference ranges were confirmed to be within
										normal values prior to the administration of non-ionic
										iodinated contrast agent.. CT images for attenuation
										correction and anatomic localization followed by PET images
										were obtained..</td>
								</tr>
							</tbody>
						</table>
					</text>
					<entry>
						<procedure moodCode="EVN" classCode="PROC">
							<id root="1.2.840.10213.2.62.7044785528.999999999"/>
							<code code="78814"
								displayName="Positron emission tomography (PET) with concurrently acquired 
								computed tomography (CT)"
								codeSystem="2.16.840.1.113883.6.12" codeSystemName="CPT4"/>
							<!--*****************************************************************************
This is the first instance of Sex for Clinical Use. It is a subentry of the PET scan 
procedure for which  it is relevant.
*****************************************************************************-->
							<entryRelationship typeCode="COMP">
								<observation classCode="OBS" moodCode="EVN">
									<templateId root="2.16.840.1.113883.10.15.3"
										extension="2022-09-01"/>
									<code code="99501-9" codeSystem="2.16.840.1.113883.6.1"
										displayName="Sex for clinical use"/>
									<statusCode code="completed"/>
									<value xsi:type="CD" codeSystem="2.16.840.1.113883.4.642.1.983"
										codeSystemName="Sex For Clinical Use" code="female"
										displayName="Female sex for clinical use"> </value>
									<!--*****************************************************************************
This is reference points to supporting information for Sex for Clinical Use. It is 
the basis on which the SFCU is assigned.
*****************************************************************************-->
									<entryRelationship typeCode="SPRT">
										<act classCode="ACT" moodCode="EVN">
											<templateId root="2.16.840.1.113883.10.20.22.4.122"/>
											<id root="6C844C75-AA34-411C-B7BD-5E4A9F206E29"/>
											<code nullFlavor="OTH" codeSystem="NP"/>
											<statusCode code="completed"/>
										</act>
									</entryRelationship>
								</observation>
							</entryRelationship>

						</procedure>

					</entry>
				</section>
			</component>
			<!--							
**********************************************************************
                    End of Imaging Procedure Description Section
**********************************************************************
-->
			<component>
				<!--
**********************************************************************
                     Findings Section
**********************************************************************
-->
				<!--
**********************************************************************
                     SUV value - Radiologist determines relevant sex at time of 
                     measurement and adds as DICOM acquisition context
**********************************************************************
-->
				<section>
					<templateId root="2.16.840.1.113883.10.20.6.1.2"/>
					<code code="121070" codeSystem="1.2.840.10008.2.16.4" codeSystemName="DCM"
						displayName="Findings"/>
					<title>Findings</title>
					<text>
						<paragraph>
							<caption>Finding</caption>
							<content ID="Fndng2">Findings narritive goes here...</content>
						</paragraph>
						<paragraph>
							<caption>Standardized uptake value</caption>
							<content ID="Suv2">12g/ml{SUVlbm}</content>
						</paragraph>
						<paragraph>
							<caption>Source of Measurement</caption>
							<content ID="SrceOfMeas2">
								<linkHtml
									href="http://www.example.org/radiology1.2.840.113619.2.62.994044785528.114289542805/series/1.2.250.1.59.40211.789001276.14556172.67789/instances/1.2.250.1.59.40211.2678810.87991027.899772.2;contentType=application/dicom"
									>Coronal</linkHtml>
							</content>
						</paragraph>
					</text>
					<entry>
						<observation classCode="OBS" moodCode="EVN">
							<!-- Text Observation -->
							<templateId root="2.16.840.1.113883.10.20.6.2.12"/>
							<code code="121071" codeSystem="1.2.840.10008.2.16.4"
								codeSystemName="DCM" displayName="Finding"/>
							<value xsi:type="ED">
								<reference value="#Fndng2"/>
							</value>
							<!-- inferred from measurement -->
							<entryRelationship typeCode="SPRT">
								<observation classCode="OBS" moodCode="EVN">
									<templateId root="2.16.840.1.113883.10.20.6.2.14"/>
									<code code="52988006" codeSystem="2.16.840.1.113883.6.96"
										codeSystemName="SNOMED" displayName="Lesion">
										<originalText>
											<reference value="#Suv2"/>
										</originalText>
									</code>
									<!-- no DICOM attribute   -->
									<statusCode code="completed"/>
									<effectiveTime value="20060823223912"/>
									<value xsi:type="PQ" value="12" unit="g/ml{SUVlbm}">
										<translation code="g/ml{SUVlbm}"
											codeSystem="2.16.840.1.113883.6.8" codeSystemName="UCUM"
											codeSystemVersion="1.5"/>
									</value>
									<!--*****************************************************************************
This Substance Administration is supporting information for Sex for Clinical Use. It 
is not likely to appear in a PET/CT report but is provided to illustrate the use of 
the supporting reference.
*****************************************************************************-->									<entryRelationship typeCode="COMP">
										<observation classCode="OBS" moodCode="EVN">
											<templateId root="2.16.840.1.113883.10.15.3"
												extension="2022-09-01"/>
											<code code="99501-9" codeSystem="2.16.840.1.113883.6.1"
												displayName="Sex for clinical use"/>
											<statusCode code="completed"/>
											<value xsi:type="CD"
												codeSystem="2.16.840.1.113883.4.642.1.983"
												codeSystemName="Sex For Clinical Use" code="male"
												displayName="Male sex for clinical use"> </value>
										</observation>
									</entryRelationship>

									<!-- inferred from image -->
									<entryRelationship typeCode="SUBJ">
										<observation classCode="DGIMG" moodCode="EVN">
											<templateId root="2.16.840.1.113883.10.20.6.2.8"/>
											<!-- (0008,1155) Referenced SOP Instance UID-->
											<id
												root="1.2.840.113619.2.62.994044785528.20060823.200608232232322.3"/>
											<!-- (0008,1150) Referenced SOP Class UID -->
											<code code="1.2.840.10008.5.1.4.1.1.128"
												codeSystem="1.2.840.10008.2.6.1"
												codeSystemName="DCMUID"
												displayName="Positron Emission Tomography Image Storage"> </code>
											<text mediaType="application/dicom">
												<!-- reference to PET DICOM image -->
												<reference
												value="http://www.example.org/radiology1.2.840.113619.2.62.994044785528.114289542805/series/1.2.250.1.59.40211.789001276.14556172.67789/instances/1.2.250.1.59.40211.2678810.87991027.899772.2;contentType=application/dicom"
												/>
											</text>
											<effectiveTime value="20060823223232"/>
											<!-- Referenced Frames -->
											<entryRelationship typeCode="COMP">
												<observation classCode="ROIBND" moodCode="EVN">
													<templateId root="2.16.840.1.113883.10.20.6.2.10"/>
													<code code="121190"
												codeSystem="1.2.840.10008.2.16.4"
												displayName="Referenced Frames"/>
													<entryRelationship typeCode="COMP">
														<!-- Boundary Observation -->
														<observation classCode="OBS" moodCode="EVN">
															<templateId root="2.16.840.1.113883.10.20.6.2.11"/>
															<code code="113036"
												codeSystem="1.2.840.10008.2.16.4"
												displayName="Group of Frames for Display"/>
															<value xsi:type="INT" value="1"/>
														</observation>
													</entryRelationship>
												</observation>
											</entryRelationship>
											<!-- Purpose of Reference -->
											<entryRelationship typeCode="RSON">
												<observation classCode="OBS" moodCode="EVN">
													<templateId root="2.16.840.1.113883.10.20.6.2.9"/>
													<code code="ASSERTION"
												codeSystem="2.16.840.1.113883.5.4"/>
													<value xsi:type="CD" code="121112"
												codeSystem="1.2.840.10008.2.16.4"
												codeSystemName="DCM"
												displayName="Source of Measurement">
														<originalText>
															<reference value="#SrceOfMeas2"/>
														</originalText>
													</value>
												</observation>
											</entryRelationship>
										</observation>
									</entryRelationship>
								</observation>
							</entryRelationship>
						</observation>
					</entry>
				</section>
				<!--							
**********************************************************************
                    End of Findings Section
**********************************************************************
-->
			</component>
			<component>
				<!--
**********************************************************************
                    Impressions Section 
**********************************************************************
-->
				<section>
					<code code="121072" codeSystem="1.2.840.10008.2.16.4" codeSystemName="DCM"
						displayName="Impressions"/>
					<title>Impressions</title>
					<text>
						<paragraph>
							<caption>Impression</caption>
							<content ID="Fndng3">Impression goes here...</content>
						</paragraph>
					</text>
					<entry>
						<!-- Impression report element (TEXT) -->
						<observation classCode="OBS" moodCode="EVN">
							<!-- Text Observation -->
							<templateId root="2.16.840.1.113883.10.20.6.2.12"/>
							<code code="121073" codeSystem="1.2.840.10008.2.16.4"
								codeSystemName="DCM" displayName="Impression"/>
							<value xsi:type="ED">
								<reference value="#Fndng3"/>
							</value>
						</observation>
					</entry>
					<entry>
						<act moodCode="EVN" classCode="ACT">
							<templateId root="2.16.840.1.113883.10.20.6.2.5"/>
							<!-- Procedure Context template -->
							<code code="78814"
								displayName="Positron emission tomography (PET) with concurrently acquired computed tomography (CT)"
								codeSystem="2.16.840.1.113883.6.12" codeSystemName="CPT4"/>
							<!-- Note: This code is slightly different than the code used in the header documentationOf and overrides it, which is what this entry is for. -->
							<effectiveTime value="20060823222400"/>
						</act>
					</entry>
				</section>
				<!--							
**********************************************************************
                    End of Impressions Section
**********************************************************************
-->
			</component>
		</structuredBody>
	</component>
</ClinicalDocument>


```
