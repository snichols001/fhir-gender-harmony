CDA Examples


```
<component>
	<!-- This sample is undergoing significant review. Please be aware that there may be signficant changes --> 
	<!-- Developed by Emma Jones --> 
	<section>
		<templateId root="2.16.840.1.113883.10.20.22.2.17" extension="2015-08-01"/>
		<templateId root="2.16.840.1.113883.10.20.22.2.17"/>
		<code code="29762-2" codeSystem="2.16.840.1.113883.6.1" codeSystemName="LOINC"/>
		<title>Social History</title>
		<text>
			<table>
				<thead>
					<tr>
						<th>Social History</th>
						<th>Observation</th>
						<th>Date</th>
					</tr>
				</thead>
				<tbody styleCode="xRowGroup">
					<tr ID="_6e185602-f807-40c6-8003-41c589ae62af">
						<td>
							<content ID="_4503c511-8e06-41a0-8f6b-3f5d23b131e2">Gender identity</content>
						</td>
						<td>
							<content ID="_71d1b8e1-b3dd-483f-80f3-49656e518994">Female-to-male transsexual</content>						
						</td>
						<td/>
						<content>2001 - </content>
						<td/>
					</tr>
				</tbody>
			</table>
		</text>		
		<entry>
			<observation classCode="OBS" moodCode="EVN">
				<templateId root="2.16.840.1.113883.10.20.22.4.38" extension="2015-08-01"/>
				<templateId root="2.16.840.1.113883.10.20.22.4.38"/>
				<id root="2.16.840.1.113883.19" extension="123456789"/>
				<!-- Per the C-CDA 2.1 IG - (CONF:1198-8558) - social history type value set is a SHOULD - therefore this value is not in the value set however, it is allowed-->
				<code code="76691-5" codeSystem="2.16.840.1.113883.6.1" displayName="Gender identity" codeSystemName="LOINC">
					<originalText>
						<reference value="#_4503c511-8e06-41a0-8f6b-3f5d23b131e2"/>
					</originalText>
				</code>
				<text>
					<reference value="#_6e185602-f807-40c6-8003-41c589ae62af"/>
				</text>
				<statusCode code="completed"/>
				<!-- interval start with no end to indicate from 2001 -->
				<effectiveTime xsi:type="IVL_TS">
					<low value="2001"/> 
				</effectiveTime>
				<!-- Selected from this value set - https://phinvads.cdc.gov/vads/ViewValueSet.action?id=B0155EA6-45BB-E711-ACE2-0017A477041A -->
				<value xsi:type="CD" code="407377005" codeSystem="2.16.840.1.113883.6.96" displayName="Female-to-male transsexual" codeSystemName="SNOMED CT">
					<originalText>
						<reference value="#_71d1b8e1-b3dd-483f-80f3-49656e518994"/>
					</originalText>
				</value>
				<author>
					<templateId root="2.16.840.1.113883.10.20.22.4.119"/>
					<time value="201406061032-0500"/>
					<assignedAuthor>
						<id extension="99999999" root="2.16.840.1.113883.4.6"/>
						<code code="200000000X" codeSystem="2.16.840.1.113883.6.101" displayName="Allopathic and Osteopathic Physicians" codeSystemName="Healthcare Provider Taxonomy (HIPAA)"/>
						<telecom use="WP" value="tel:+1(555)555-1002"/>
						<assignedPerson>
							<name>
								<given>Henry</given>
								<family>Seven</family>
							</name>
						</assignedPerson>
					</assignedAuthor>
				</author>
			</observation>
		</entry>
	</section>
</component>
```
