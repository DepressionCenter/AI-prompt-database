![Depression Center Logo](https://github.com/DepressionCenter/.github/blob/main/images/EFDCLogo_375w.png "depressioncenter.org")


# **AI Prompts Database**
#### *__Real-world examples of UMGPT, Maizey and other AI prompts from the University of Michigan.__*

<br />

## Vendor List

### Contributors:
+ Gabriel Mongefranco ([@gabrielmongefranco](https://github.com/gabrielmongefranco)), Eisenberg Family Depression Center

### Description
Generates a table with information about companies, given a list of the company names. This table is formatted so it can be copied & pasted into Excel. It can be used, for example, to gather basic information about vendors such as what products or services they sell, other names they do business as, website links, etc.

### Template
**For: GPT**  <br />
**Prompt:**
<pre>
For the following list of companies, provide a table with the following columns:
1) Vendor Account Name: Company name (as given in the list that follows)
2) Also Known As: 2-3 other names the company is also known as or does business as, if any, or leave blank if N/A
3) Description: A short one-sentence description of the company
4) Main Products/Services: A short pipe-separated list (separate with the | character) of up to 5 of their main products and/or services, with the product/service name and a short one-sentence description of each
5) Keywords: 5-10 keywords representing their products and services, separated by commas. Include the keyword "mobile technologies" if the company deals with consumer wearable fitness trackers, CGMs, smartwatches, bluetooth blood pressure monitors, smart scales, smart rings, mobile data from consumer wearables, fitbit devices, withings devices, garmin devices, apple watch devices, or insulin pumps. Include they keyword "custom development" if the company provides custom app development or web development services.
6) Used by U-M Researchers? If both the company and at least one of their products/services are mentioned in https://experts.umich.edu on the same page, or mentioned in any research publication whose authors include University of Michigan professors, say "Yes". If unsure but leaning towards Yes, say "Maybe". Otherwise, say "No".
7) Website: A link to the vendor's website with the URL shown (even if not up-to-date). Provide the URL only without Markdown and without HTML tags.
Keep the list in the same order provided.
If any answer is "N/A" or "Not applicable" or "Not found", leave the value blank instead.

List of companies:
<var>[INPUT]</var>.
</pre>
<br />


**For: LLAMA**  <br />
**Prompt:**
<pre>
For the following list of companies, provide an HTML table (displayed as HTML, not Markdown) with the following columns:
1) Vendor Account Name: Company name (as given in the list that follows).
2) Also Known As: 2-3 other names the company is also known as or does business as, if any, or leave blank if N/A.
3) Description: A short one-sentence description of the company.
4) Main Products/Services: A list of the top 5 products or services provided by this company (separate with the | character).
5) Keywords: 5-10 keywords representing their products and services, separated by commas. Include the keyword "mobile technologies" if the company deals with consumer wearable fitness trackers, CGMs, smartwatches, bluetooth blood pressure monitors, smart scales, smart rings, mobile data from consumer wearables, fitbit devices, withings devices, garmin devices, apple watch devices, or insulin pumps. Include they keyword "custom development" if the company provides custom app development or web development services.
6) Used by U-M Researchers? If the company name and at least one of its main products/services are both listed in a publication abstract at https://experts.umich.edu, say "Yes". If unsure but leaning towards Yes, say "Maybe". Otherwise, say "No".
7) Website: The URL of the vendor's website.

Keep the list in the same order provided.
If any answer is "N/A" or "Not applicable" or "Not found", leave the value blank instead.
Display it as HTML.

List of companies:
<var>[INPUT]</var>.
</pre>
<br />


### Examples

Get basic company info from GPT about some continous glucose monitor (CGM) manufacturers.<br />
<kbd>
For the following list of companies, provide a table with the following columns:
1) Vendor Account Name: Company name (as given in the list that follows)
2) Also Known As: 2-3 other names the company is also known as or does business as, if any, or leave blank if N/A
3) Description: A short one-sentence description of the company
4) Main Products/Services: A short pipe-separated list (separate with the | character) of up to 5 of their main products and/or services, with the product/service name and a short one-sentence description of each
5) Keywords: 5-10 keywords representing their products and services, separated by commas. Include the keyword "mobile technologies" if the company deals with consumer wearable fitness trackers, CGMs, smartwatches, bluetooth blood pressure monitors, smart scales, smart rings, mobile data from consumer wearables, fitbit devices, withings devices, garmin devices, apple watch devices, or insulin pumps. Include they keyword "custom development" if the company provides custom app development or web development services.
6) Used by U-M Researchers? If both the company and at least one of their products/services are mentioned in https://experts.umich.edu on the same page, or mentioned in any research publication whose authors include University of Michigan professors, say "Yes". If unsure but leaning towards Yes, say "Maybe". Otherwise, say "No".
7) Website: A link to the vendor's website with the URL shown (even if not up-to-date). Provide the URL only without Markdown and without HTML tags.
Keep the list in the same order provided.
If any answer is "N/A" or "Not applicable" or "Not found", leave the value blank instead.
  
List of Vendors:  

Abbott 

Dexcom 

Medtronic 

</kbd><br /><br />
<pre><code>

< table >
< thead >
< tr >
<th>Vendor Account Name</th>
<th>Also Known As</th>
<th>Description</th>
<th>Main Products/Services</th>
<th>Keywords</th>
<th>Used by U-M Researchers?</th>
<th>Website</th>
< /tr >
< /thead >
< tbody >< tr >
< td >Abbott< /td >
< td >Abbott Laboratories< /td >
< td >A multinational medical devices and health care company.< /td >
< td >Freestyle Libre (Glucose Monitoring System) - A continuous glucose monitoring system< /td >
< td >CGMs, medical devices, diagnostics, glucose monitoring, nutrition, mobile technologies< /td >
< td >Yes< /td >
< td ><a href="http://www.abbott.com">http://www.abbott.com</a>< /td >
< /tr >
< tr >
< td >Dexcom< /td >
< td >< /td >
< td >A company specializing in glucose monitoring technology for diabetes management.< /td >
< td >Dexcom G6 (Continuous Glucose Monitoring System) - A real-time CGM system without the need for fingersticks< /td >
< td >CGMs, diabetes management, glucose monitoring, mobile technologies< /td >
< td >Yes< /td >
< td ><a href="http://www.dexcom.com">http://www.dexcom.com</a>< /td >
< /tr >
< tr >
< td >Medtronic< /td >
< td >Medtronic PLC< /td >
< td >A global leader in medical technology, services, and solutions.< /td >
< td >MiniMed Insulin Pump - An insulin delivery system< /td >
< td >medical devices, insulin pumps, surgical technologies, patient monitoring, mobile technologies, custom development< /td >
< td >Yes< /td >
< td ><a href="http://www.medtronic.com">http://www.medtronic.com</a>< /td >
< /tr >
</tbody>
< /table >

</code></pre><br />

<table>
<thead>
<tr>
<th>Vendor Account Name</th>
<th>Also Known As</th>
<th>Description</th>
<th>Main Products/Services</th>
<th>Keywords</th>
<th>Used by U-M Researchers?</th>
<th>Website</th>
</tr>
</thead>
<tbody><tr>
<td>Abbott</td>
<td>Abbott Laboratories</td>
<td>A multinational medical devices and health care company.</td>
<td>Freestyle Libre (Glucose Monitoring System) - A continuous glucose monitoring system</td>
<td>CGMs, medical devices, diagnostics, glucose monitoring, nutrition, mobile technologies</td>
<td>Yes</td>
<td><a href="http://www.abbott.com">http://www.abbott.com</a></td>
</tr>
<tr>
<td>Dexcom</td>
<td></td>
<td>A company specializing in glucose monitoring technology for diabetes management.</td>
<td>Dexcom G6 (Continuous Glucose Monitoring System) - A real-time CGM system without the need for fingersticks</td>
<td>CGMs, diabetes management, glucose monitoring, mobile technologies</td>
<td>Yes</td>
<td><a href="http://www.dexcom.com">http://www.dexcom.com</a></td>
</tr>
<tr>
<td>Medtronic</td>
<td>Medtronic PLC</td>
<td>A global leader in medical technology, services, and solutions.</td>
<td>MiniMed Insulin Pump - An insulin delivery system</td>
<td>medical devices, insulin pumps, surgical technologies, patient monitoring, mobile technologies, custom development</td>
<td>Yes</td>
<td><a href="http://www.medtronic.com">http://www.medtronic.com</a></td>
</tr>
</tbody></table><br />


----

Copyright © 2024 The Regents of the University of Michigan
