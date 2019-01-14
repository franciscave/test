![BIC LOGO](media/bic.png)

***Book Industry Communication***

**BIC Library Web Services API Standards**

**Retrieve Quotes List**

Version 0.9, 6 September 2018

**This document:**
http://www.bic.org.uk/files/pdfs/BICLWSQuotesList-V0.9.pdf  
**XML schema:**
http://www.bic.org.uk/files/xml/BICLWSQuotesList-V0.9.xsd  
**WSDL file:**  
http://www.bic.org.uk/files/xml/BICLWSQuotesListSOAP-V0.9.wsdl  
**XML namespace:** http://www.bic.org.uk/librarywebservices/quotesList  
**Latest next review date:** 1 July 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Retrieve Quotes List API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

A Retrieve Quotes List Request may be implemented using either SOAP or
the basic HTTPS protocol\[1\] and POST method. The payload of a Retrieve
Quotes List Request may be formatted either as an XML document or as an
equivalent JSON document.

The same Response format options (payload in XML or JSON) will apply to
both basic HTTPS and SOAP exchanges.

The complete specification of the Retrieve Quotes List Request/Response
web service includes two machine-readable resources that are to be used
by implementers in conjunction with this document:

  - a WSDL Definition for the SOAP protocol version of the web service

  - an XML Schema for Requests and Response payloads in XML format.

A Request or Response payload expressed in JSON must be entirely
translatable into an equivalent XML representation that conforms to the
XML schema.

It is strongly recommended that SOAP client implementations of this web
service be constructed using the BIC WSDL Definitions as a starting
point, as this will promote interoperability between SOAP client and
server implementations. In some development environments it may be
easier to implement a SOAP server without using the BIC WSDL
Definitions, but in this case care must be taken to ensure that the WSDL
Definitions that describe the actual implementation is functionally
equivalent to the BIC WSDL Definitions.

**Business requirements**

There is a need for buyers, including EDI users, to ensure that they are
aware of all quotes prepared by a particular supplier. This web service
enables a buyer to retrieve a list of quotes, selected by date-of-issue
range or by quotation number pattern. Having retrieved such a list, a
buyer may then retrieve individual quotes using the BIC Library Web
Services Retrieve Quotation API.

A typical use of the Retrieve Quotes List API might involve the
following sequence:

1.  The supplier prepares quotations in accordance with the library’s
    instructions and uploads these to their web service.

2.  The library uses the Retrieve Quotes List API to retrieve a list of
    quotations prepared by the supplier.

3.  The library uses the Retrieve Quotation API one or more times to
    retrieve quotations from the supplier’s web service.

4.  The library processes each quotation and, if appropriate, uses the
    Order Request and Response API to confirm the order with the
    supplier.

This service will also enable an aggregation service to reconcile their
system with those of their data suppliers, to check for missing
documents and changes made outside their system.

RETRIEVE QUOTES LIST – REQUEST

Requests should include an XML or JSON document as specified below as
the body of a request message.

**Request document name and version**

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Quotes list request Version 0.9</strong></td>
<td><h1 id="section"></h1></td>
<td><strong>&lt;QuotesListRequest version=”0.9”&gt;<br />
{ "QuotesListRequest": { "version": ,...</strong></td>
<td></td>
</tr>
</tbody>
</table>

**Request document content**

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Request header</strong></td>
<td><h1 id="m">M[2]</h1></td>
<td><strong>Header.</strong></td>
<td>[3]</td>
</tr>
<tr class="even">
<td>1</td>
<td><blockquote>
<p>A unique identifier for the sender of the request. An alphanumeric string not containing spaces or punctuation</p>
</blockquote></td>
<td>D</td>
<td>ClientID</td>
<td></td>
</tr>
<tr class="odd">
<td>2</td>
<td>A password to further authenticate the sender of the request</td>
<td>D</td>
<td>ClientPassword</td>
<td></td>
</tr>
<tr class="even">
<td>3</td>
<td>Account identifier.</td>
<td>D</td>
<td>AccountIdentifier.</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td><p>A code value from a BIC-controlled codelist for the scheme used for the account identifier (see ONIX codelist 44). Permitted schemes are:</p>
<p><em>01</em> Proprietary</p>
<p><em>06</em> EAN-UCC GLN</p>
<p><em>07</em> SAN</p>
<p><em>11</em> PubEasy PIN</p></td>
<td>M</td>
<td>AccountIDType</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Account identifier for this request, using the specified scheme</td>
<td>M</td>
<td>IDValue</td>
<td></td>
</tr>
<tr class="odd">
<td>4</td>
<td>Identification number / string of this request</td>
<td>D</td>
<td>RequestNumber</td>
<td></td>
</tr>
<tr class="even">
<td>5</td>
<td><p>Document date/time: the date/time when the request was generated. Permitted formats are:</p>
<p>YYYYMMDD</p>
<p>YYYYMMDDTHHMM</p>
<p>YYYYMMDDTHHMMZ (universal time)</p>
<p>YYYYMMDDTHHMM±HHMM (time zone)</p>
<p>where "T" represents itself, ie letter T</p></td>
<td>D</td>
<td>IssueDateTime</td>
<td></td>
</tr>
<tr class="odd">
<td>6</td>
<td>References. If included, must contain a reference number or a reference date-time or both.</td>
<td>D</td>
<td>ReferenceCoded.</td>
<td>R</td>
</tr>
<tr class="even">
<td></td>
<td>Reference type<br />
<em>16</em> Contract reference<br />
<em>35</em> Library’s supplier reference<br />
<em>36</em> Supplier’s library customer reference</td>
<td>M</td>
<td>ReferenceTypeCode</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>Reference</td>
<td>D</td>
<td>ReferenceNumber</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Reference date-time (for format options see line 5)</td>
<td>D</td>
<td>ReferenceDateTime</td>
<td></td>
</tr>
<tr class="odd">
<td>7</td>
<td>Supplier to whom this request should be forwarded, if it is not addressed to the web service host (use only for requests sent to aggregation services).</td>
<td>D</td>
<td>SupplierIdentifier.</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Supplier ID type - see ONIX codelist 92</td>
<td>M</td>
<td>SupplierIDType</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>ID type name, only if ID type = proprietary</td>
<td>D</td>
<td>IDTypeName</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Identifier</td>
<td>M</td>
<td>IDValue</td>
<td></td>
</tr>
<tr class="odd">
<td>8</td>
<td>Start date of the period for which the list of quotations prepared in that period is requested. – YYYYMDD</td>
<td>D</td>
<td>PeriodStartDate</td>
<td></td>
</tr>
<tr class="even">
<td>9</td>
<td>End date of the period for which the list of quotations prepared in that period is requested. – YYYYMMDD</td>
<td>D</td>
<td>PeriodEndDate</td>
<td></td>
</tr>
<tr class="odd">
<td>10</td>
<td>Quotation number pattern to be matched. Use a regular expression that conforms to <a href="http://www.w3.org/TR/xmlschema11-2/#regexs">Appendix G of W3C XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes</a>.</td>
<td>D</td>
<td>ReferenceNumberPattern</td>
<td></td>
</tr>
</tbody>
</table>

*  
Example of a Retrieve Quotes List Request XML payload using either the
SOAP or the HTTPS protocol and the POST method, in which the request is
for all quotes issued from 1 April 2018 onwards:*

\<QuotesListRequest version="0.9"

xmlns="http://www.bic.org.uk/librarywebservices/quotesList"\>

\<AccountIdentifier\>

\<AccountIDType\>01\</AccountIDType\>

\<IDValue\>12345\</IDValue\>

\</AccountIdentifier\>

\<RequestNumber\>001\</RequestNumber\>

\<IssueDateTime\>20180422T1525\</IssueDateTime\>

\<PeriodStartDate\>20180401\</PeriodStartDate\>

\</QuotesListRequest\>

*JSON equivalent of the above payload:*

{

"QuotesListRequest": {

"version": "0.9",

"xmlns": "http://www.bic.org.uk/librarywebservices/quotesList",

"AccountIdentifier": {

"AccountIDType": "01",

"IDValue": "12345"

},

"RequestNumber": "001",

"IssueDateTime": "20180422T1525",

"PeriodStartDate": "20180401"

}

}

*  
Example of a Retrieve Quotes List Request XML payload using either the
SOAP or the HTTPS protocol and the POST method, in which the request is
for all quotes with numbers that match the regular expression pattern
‘01020\\d+’ (i.e. numbers beginning ‘01020’):*

\<QuotesListRequest version="0.9"

xmlns="http://www.bic.org.uk/librarywebservices/quotesList"\>

\<AccountIdentifier\>

\<AccountIDType\>01\</AccountIDType\>

\<IDValue\>12345\</IDValue\>

\</AccountIdentifier\>

\<RequestNumber\>001\</RequestNumber\>

\<IssueDateTime\>20180422T1525\</IssueDateTime\>

\<ReferenceNumberPattern\>01020\\d+\</ReferenceNumberPattern\>

\</QuotesListRequest\>

*JSON equivalent of the above payload:*

{

"QuotesListRequest": {

"version": "0.9",

"xmlns": "http://www.bic.org.uk/librarywebservices/quotesList",

"AccountIdentifier": {

"AccountIDType": "01",

"IDValue": "12345"

},

"RequestNumber": "001",

"IssueDateTime": "20180422T1525",

"ReferenceNumberPattern": "01020\\\\d+"

}

}

RETRIEVE QUOTES LIST – RESPONSE

The Response will use the protocol corresponding to the Request. If the
Request uses the basic HTTPS protocol, the Response will be an XML or
JSON document as specified below attached to a normal HTTPS header. If
the Request uses the SOAP protocol, the Response will contain a SOAP
response message whose body will contain the XML or JSON document
specified below.

**Response document name and version**

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Retrieve quotes list response Version 0.9</strong></td>
<td><h1 id="section-1"></h1></td>
<td><p><strong>&lt;QuotesListResponse version=”0.9”&gt;</strong></p>
<p><strong>{ "QuotesListResponse": { "version": ...</strong></p></td>
<td></td>
</tr>
</tbody>
</table>

**Header**

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Response payload header</strong></td>
<td><h1 id="m-1">M</h1></td>
<td><strong>Header.</strong></td>
<td></td>
</tr>
<tr class="even">
<td>1</td>
<td><p>Document date/time: the date/time when the report was generated. Permitted formats are:</p>
<p>YYYYMMDD</p>
<p>YYYYMMDDTHHMM</p>
<p>YYYYMMDDTHHMMZ (universal time)</p>
<p>YYYYMMDDTHHMM±HHMM (time zone)</p>
<p>where “T” represents itself, ie letter T</p></td>
<td>M</td>
<td>IssueDateTime</td>
<td></td>
</tr>
<tr class="odd">
<td>2</td>
<td>Sender (web service host)</td>
<td>M</td>
<td>SenderIdentifier.</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Sender ID type - see ONIX codelist 92</td>
<td>M</td>
<td>SenderIDType</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>ID type name, only if ID type = proprietary</td>
<td>D</td>
<td>IDTypeName</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Identifier</td>
<td>M</td>
<td>IDValue</td>
<td></td>
</tr>
<tr class="odd">
<td>3</td>
<td>Identification number / string of this response</td>
<td>D</td>
<td>ResponseNumber</td>
<td></td>
</tr>
<tr class="even">
<td>4</td>
<td>Account identifier.</td>
<td>D</td>
<td>AccountIdentifier.</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td><p>A code value from a BIC-controlled codelist for the scheme used for the account identifier (see ONIX codelist 44). Must be specified if an account identifier is specified. Permitted schemes are:</p>
<p><em>01</em> Proprietary</p>
<p><em>06</em> EAN-UCC GLN</p>
<p><em>07</em> SAN</p>
<p><em>11</em> PubEasy PIN</p></td>
<td>M</td>
<td>AccountIDType</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Account identifier for this request, using the specified scheme</td>
<td>M</td>
<td>IDValue</td>
<td></td>
</tr>
<tr class="odd">
<td>5</td>
<td>References: request number and/or date/time of request must be quoted if included in the request.</td>
<td>D</td>
<td>ReferenceCoded</td>
<td>R</td>
</tr>
<tr class="even">
<td></td>
<td><blockquote>
<p>Reference type<br />
<em>01</em> Number or date/time of associated quotes list request<br />
<em>16</em> Contract reference<br />
<em>35</em> Library’s supplier reference<br />
<em>36</em> Supplier’s library customer reference</p>
</blockquote></td>
<td>M</td>
<td>ReferenceTypeCode</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>Reference number / string</td>
<td>D</td>
<td>ReferenceNumber</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Reference date or date and time. Mandatory if an IssueDateTime is included in the request. See Header line 1 for format options.</td>
<td>D</td>
<td>ReferenceDateTime</td>
<td></td>
</tr>
<tr class="odd">
<td>6</td>
<td>Supplier identifier (only included if specified in the request; mandatory if the response type code is ‘19’)</td>
<td>D</td>
<td>SupplierIdentifier.</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Supplier ID type - see ONIX codelist 92</td>
<td>M</td>
<td>SupplierIDType</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>ID type name, only if Supplier ID type is proprietary</td>
<td>D</td>
<td>IDTypeName</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Identifier</td>
<td>M</td>
<td>IDValue</td>
<td></td>
</tr>
</tbody>
</table>

**  
Response header** *(continued)*

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Payload header</strong></td>
<td><h1 id="m-2">M</h1></td>
<td><strong>Header.</strong></td>
<td></td>
</tr>
<tr class="even">
<td>7</td>
<td>Response code, if there are exception conditions.</td>
<td>D</td>
<td>ResponseCoded.</td>
<td>R</td>
</tr>
<tr class="odd">
<td></td>
<td>Response type code. Suggested code values:<br />
<em>01</em> Service unavailable<br />
<em>02</em> Invalid ClientID or ClientPassword<br />
<em>03</em> Server unable to process request – a reason<br />
should normally be given as a free text<br />
description – see below<br />
<em>16</em> Invalid or unknown account, supplier or<br />
ship-to party identifier<br />
<em>17</em> Invalid period start or end date<br />
<em>18</em> Server unable to process request - specified range too large<br />
<em>19</em> Server unable to process request –<br />
unable to contact supplier</td>
<td>M</td>
<td>ResponseType</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Free text description / reason for response</td>
<td>D</td>
<td>ResponseTypeDescription</td>
<td></td>
</tr>
</tbody>
</table>

**Response detail**

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Details of all quotations that meet the selection criteria in the request. Mandatory unless the header reports a condition that prevents any response</strong></td>
<td><h1 id="d">D</h1></td>
<td><strong>ItemDetail.</strong></td>
<td><strong>R</strong></td>
</tr>
<tr class="even">
<td>1</td>
<td>Quotes list response item line number</td>
<td><h1 id="d-1">D</h1></td>
<td>LineNumber</td>
<td></td>
</tr>
<tr class="odd">
<td>2</td>
<td>Document references. It is mandatory to include the supplier’s quotation reference in all items. If the quotation has already resulted in orders, one or more buyer’s order references must be included.</td>
<td>M</td>
<td>ReferenceCoded</td>
<td>R</td>
</tr>
<tr class="even">
<td></td>
<td>Reference type code<br />
<em>11</em>  Buyer’s order reference<br />
<em>29</em> Supplier’s quotation reference</td>
<td></td>
<td>ReferenceTypeCode</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>Reference number / string</td>
<td>D</td>
<td>ReferenceNumber</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>Reference date or date and time. See Header line 1 for format options.</td>
<td>D</td>
<td>ReferenceDateTime</td>
<td></td>
</tr>
<tr class="odd">
<td>3</td>
<td>Total number of lines on this quotation</td>
<td>M</td>
<td>NumberOfLines</td>
<td></td>
</tr>
</tbody>
</table>

*  
Example of a Retrieve Quotes List Response XML payload using either the
SOAP or the HTTPS protocol and the POST method:*

\<QuotesListResponse version="0.9"

xmlns="http://www.bic.org.uk/librarywebservices/quotesList"\>

\<Header\>

\<IssueDateTime\>20180422T1527\</IssueDateTime\>

\<SenderIdentifier\>

\<SenderIDType\>01\</SenderIDType\>

\<IDValue\>XYZ\</IDValue\>

\</SenderIdentifier\>

\<AccountIdentifier\>

\<AccountIDType\>01\</AccountIDType\>

\<IDValue\>12345\</IDValue\>

\</AccountIdentifier\>

\</Header\>

\<ItemDetail\>

\<ReferenceCoded\>

\<ReferenceTypeCode\>29\</ReferenceTypeCode\>

\<ReferenceNumber\>Q12345\</ReferenceNumber\>

\<ReferenceDateTime\>20180409\</ReferenceDateTime\>

\</ReferenceCoded\>

\<ReferenceCoded\>

\<ReferenceTypeCode\>11\</ReferenceTypeCode\>

\<ReferenceNumber\>01020304\</ReferenceNumber\>

\</ReferenceCoded\>

\<ReferenceCoded\>

\<ReferenceTypeCode\>11\</ReferenceTypeCode\>

\<ReferenceNumber\>01020305\</ReferenceNumber\>

\</ReferenceCoded\>

\<NumberOfLines\>10\</NumberOfLines\>

\</ItemDetail\>

\<ItemDetail\>

\<ReferenceCoded\>

\<ReferenceTypeCode\>29\</ReferenceTypeCode\>

\<ReferenceNumber\>Q12346\</ReferenceNumber\>

\<ReferenceDateTime\>20180419\</ReferenceDateTime\>

\</ReferenceCoded\>

\<NumberOfLines\>8\</NumberOfLines\>

\</ItemDetail\>

\</QuotesListResponse\>

*  
Example of a Retrieve Quotes List Response JSON payload using either the
SOAP or the HTTPS protocol and the POST method:*

{"QuotesListResponse": {

"version": "0.9",

"xmlns": "http://www.bic.org.uk/librarywebservices/quotesList",

"Header": {

"IssueDateTime": "20180422T1527",

"SenderIdentifier": {

"SenderIDType": "01",

"IDValue": "XYZ"

},

"AccountIdentifier": {

"AccountIDType": "01",

"IDValue": "12345"

}

},

"ItemDetail": \[

{

"ReferenceCoded": \[

{

"ReferenceTypeCode": "29",

"ReferenceNumber": "Q12345",

"ReferenceDateTime": "20180409"

},

{

"ReferenceTypeCode": "11",

"ReferenceNumber": "1020304"

},

{

"ReferenceTypeCode": "11",

"ReferenceNumber": "1020305"

}

\],

"NumberOfLines": 10

},

{

"ReferenceCoded": {

"ReferenceTypeCode": "29",

"ReferenceNumber": "Q12346",

"ReferenceDateTime": "20180419"

},

"NumberOfLines": 8

}

\]

}}

1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

2.  In the third column “M” means mandatory and “D” means dependent upon
    the business or message context.

3.  An ‘R’ in the right-most column means that the element is
    repeatable.
