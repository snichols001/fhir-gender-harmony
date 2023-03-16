### Registration of Name Change 
#### Using v2.9.1  
```
MSH|^~\&|||||20220715151029||ADT^A08|TwxxneTRWE9JGX4U2p3hCLJH|P|2.9.1
EVN||20220715151118||01^Patient Request^HL70062
PID|||patientID^^^^MR||Smith^John^^^N~Smith^Janet^^^B~Smith^Janet^^^^NOUSE||19780410000000|F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Identifies as male gender^SCT|20220715010000
PV1||O
```
#### Using the pre-adoption profile in v2.5
```
MSH|^~\&|||||20220715151029||ADT^A08|TwxxneTRWE9JGX4U2p3hCLJH|P|2.5|||||||||GenderHarmony^^2.16.840.1.113883.9.282^ISO
EVN||20220715151118||01^Patient Request^HL70062
PID|||patientID^^^^MR||Smith^John^^^N~Smith^Janet^^^B~Smith^Janet^^^^NOUSE||19780410000000|F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Identifies as male gender^SCT|20220715010000
PV1||O
```
### Order for imaging
#### Using v2.9.1
```
MSH|^~\&|||||20220715142240||OMI^O23|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.9.1
PID|||patientID^^^^MR||Smith^Janet^^^^^B~Smith^John^^^^^N||19780410000000|F
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Identifies as male gender^SCT|20220715010000
GSC|1|S||Male-typical^Male typical parameters^SexParameterForClinicalUse||OBR^4|20220715090000|Hormonal treatment, use affirmed gender Cr reference ranges
GSC|2|S||Female-typical^Female typical parameters^SexParameterForClinicalUse|19780410000000^20220715090000|OBR^4||Sex at Birth
ORC|NW
OBR||||241439007^PET heart study^SCT|||||||||||||||||||||||||||||||||||||||||82800-4^PET+CT Heart W contrast IV^LN
IPC|accessionNum|procedureID|studyInstanceUID|schProcedureStepId|PT^Positron emission tomography^DCM|122793^PET Myocardial Perfusion, Rest and Stress^DCM
```
#### Using the pre-adoption profile in v2.5
```
MSH|^~\&|||||20220715142240||OMI^O23|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.5|||||||||GenderHarmony^^2.16.840.1.113883.9.282^ISO
PID|||patientID^^^^MR||Smith^Janet^^^^^B~Smith^John^^^^^N||19780410000000|F
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Identifies as male gender^SCT|20220715010000
GSC|1|S||Male-typical^Male typical parameters^SexParameterForClinicalUse||OBR^4|20220715090000|Hormonal treatment, use affirmed gender Cr reference ranges
GSC|2|S||Female-typical^Female typical parameters^SexParameterForClinicalUse|19780410000000^20220715090000|OBR^4||Sex at Birth
ORC|NW
OBR||||241439007^PET heart study^SCT|||||||||||||||||||||||||||||||||||||||||82800-4^PET+CT Heart W contrast IV^LN
IPC|accessionNum|procedureID|studyInstanceUID|schProcedureStepId|PT^Positron emission tomography^DCM|122793^PET Myocardial Perfusion, Rest and Stress^DCM
```
### Result for imaging 
#### Using v2.9.1
```
MSH|^~\&|||||20220715142240||ORU^R01^ORU_R01|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.9.1|
PID|||patientID^^^^MR||Smith^John^^^^^N||19780410000000|F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Identifies as male gender^SCT|20220715010000
GSC|1|S||Male-typical^Male typical parameters^SexParameterForClinicalUse||OBR^4|20220715090000|Hormonal treatment, use affirmed gender Cr reference ranges
GSC|2|S||Female-typical^Female typical parameters^SexParameterForClinicalUse|19780410000000^20220715090000|OBR^4||Sex at Birth
OBR|1|accessionNum||82800-4^PET+CT Heart W contrast IV^LN|||20220715142240||||
OBX|1|CWE|55111-9^Current Imaging Procedure Description^LN||Imaging technique (protocol, contrast, radiotracer) described here||||||F|
OBX|2|FT|19005-8^Impression^LN||Report narrative goes here||||||F|
OBX|3|TX|55110-1^Conclusion^LN||Conclusion goes here||||||F|
```
#### Using the pre-adoption profile in v2.5
```
MSH|^~\&|||||20220715142240||ORU^R01^ORU_R01|WSA5mY0UBuCGrytRTAFR8UWJ|P|2.5|||||||||GenderHarmony^^2.16.840.1.113883.9.282^ISO
PID|||patientID^^^^MR||Smith^John^^^^^N||19780410000000|F|
GSP|1|S||76691-5^Gender identity^LN|446151000124109^Identifies as male gender^SCT|20220715010000
GSC|1|S||Male-typical^Male typical parameters^SexParameterForClinicalUse||OBR^4|20220715090000|Hormonal treatment, use affirmed gender Cr reference ranges
GSC|2|S||Female-typical^Female typical parameters^SexParameterForClinicalUse|19780410000000^20220715090000|OBR^4||Sex at Birth
OBR|1|accessionNum||82800-4^PET+CT Heart W contrast IV^LN|||20220715142240||||
OBX|1|CWE|55111-9^Current Imaging Procedure Description^LN||Imaging technique (protocol, contrast, radiotracer) described here||||||F|
OBX|2|FT|19005-8^Impression^LN||Report narrative goes here||||||F|
OBX|3|TX|55110-1^Conclusion^LN||Conclusion goes here||||||F|
```
 
	

