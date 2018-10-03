![](media/image1.jpeg)

***Book Industry Communication***

**BIC Library Web Services Standards**

**Retrieve Order List**

Version 0.9, 6 September 2018

**This document:**
<http://www.bic.org.uk/files/pdfs/BICLWSOrderList-V0.9.pdf>  
**XML schema:**
<http://www.bic.org.uk/files/xml/BICLWSOrderList-V0.9.xsd>  
**WSDL file:**  
<http://www.bic.org.uk/files/xml/BICLWSOrderListSOAP-V0.9.wsdl>  
**XML namespace:** http://www.bic.org.uk/librarywebservices/orderList  
**Next review date:** 1 July 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Retrieve Order List API.

A Retrieve Order List Request may be implemented using either SOAP or
the basic HTTPS protocol\[1\] and POST method. The payload of a P\&A
Request may be formatted either as an XML document or as an equivalent
JSON document.

The same Response format options (payload in XML or JSON) will apply to
both basic HTTP and SOAP exchanges.

The complete specification of the Retrieve Order List Request/Response
web service includes two machine-readable resources that are to be used
by implementers in conjunction with this document:

  - a WSDL Definition for the SOAP protocol version of the web service

  - an XML Schema for Requests and Response payloads in XML format.

It is strongly recommended that SOAP client implementations of this web
service be constructed using the BIC WSDL Definitions as a starting
point, as this will promote interoperability between SOAP client and
server implementations. In some development environments it may be
easier to implement a SOAP server without using the BIC WSDL
Definitions, but in this case care must be taken to ensure that the WSDL
Definitions that describe the actual implementation is functionally
equivalent to the BIC WSDL Definitions.

**Business requirements**

There is a need for library buyers, including EDI users, to ensure that
they are aware of all orders received and not yet fulfilled by a
particular supplier. This web service enables a buyer to retrieve a list
of orders, selected by date-of-issue range. Having retrieved such a
list, a buyer may then follow up individual orders using other web
services or by manual enquiry.

This service will also enable an aggregation service to reconcile their
system with those of their data suppliers, to check for missing
documents and changes made outside their system.

RETRIEVE ORDER LIST – REQUEST

**Requests using SOAP or non-SOAP protocols and using the HTTP POST
method**

**Request document name and version**

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Order list request Version 0.9</strong></td>
<td><h1 id="section"></h1></td>
<td><strong>&lt;OrderListRequest version=”0.9”&gt;<br />
{ &quot;OrderListRequest&quot;: { &quot;version&quot;: ,...</strong></td>
<td></td>
</tr>
</tbody>
</table>

**Request document content**

<table>
<tbody>
<tr class="odd">
<td>1</td>
<td>A unique identifier for the sender of the request. An alphanumeric string not containing spaces or punctuation</td>
<td>D[2]</td>
<td>ClientID</td>
<td></td>
</tr>
<tr class="even">
<td>2</td>
<td>A password to further authenticate the sender of the request</td>
<td>D</td>
<td>ClientPassword</td>
<td></td>
</tr>
<tr class="odd">
<td>3</td>
<td>Account identifier. Mandatory in all order list requests.</td>
<td>M</td>
<td>AccountIdentifier.</td>
<td></td>
</tr>
<tr class="even">
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
<tr class="odd">
<td></td>
<td>Account identifier for this request, using the specified scheme</td>
<td>M</td>
<td>IDValue</td>
<td></td>
</tr>
<tr class="even">
<td>4</td>
<td>Identification number / string of this request</td>
<td>D</td>
<td>RequestNumber</td>
<td></td>
</tr>
<tr class="odd">
<td>5</td>
<td><p>Document date/time: the date/time when the request was generated. Permitted formats are:</p>
<p>YYYYMMDD</p>
<p>YYYYMMDDTHHMM</p>
<p>YYYYMMDDTHHMMZ (universal time)</p>
<p>YYYYMMDDTHHMM±HHMM (time zone)</p>
<p>where &quot;T&quot; represents itself, ie letter T</p></td>
<td>D</td>
<td>IssueDateTime</td>
<td></td>
</tr>
<tr class="even">
<td>6</td>
<td>Supplier to whom this request should be forwarded, if it is not addressed to the web service host (use only for requests sent to aggregation services).</td>
<td>D</td>
<td>SupplierIdentifier.</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>Supplier ID type - see ONIX codelist 92</td>
<td>M</td>
<td>SupplierIDType</td>
<td></td>
</tr>
<tr class="even">
<td></td>
<td>ID type name, only if ID type = proprietary</td>
<td>D</td>
<td>IDTypeName</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>Identifier</td>
<td>M</td>
<td>IDValue</td>
<td></td>
</tr>
<tr class="even">
<td>7</td>
<td>Start date of the period for which the list of orders placed in that period is requested. – YYYYMMDD</td>
<td>D</td>
<td>PeriodStartDate</td>
<td></td>
</tr>
<tr class="odd">
<td>8</td>
<td>End date of the period for which the list of orders placed in that period is requested. – YYYYMMDD</td>
<td>D</td>
<td>PeriodEndDate</td>
<td></td>
</tr>
<tr class="even">
<td>9</td>
<td>Order number pattern to be matched. Use a regular expression that conforms to <a href="http://www.w3.org/TR/xmlschema11-2/#regexs">Appendix G of W3C XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes</a>.</td>
<td>D</td>
<td>ReferenceNumberPattern</td>
<td></td>
</tr>
<tr class="odd">
<td>10</td>
<td><p>Order line status has changed or not. Used to request a list of orders in which at least one order line status has changed, or no order line status has changed. If this element is included, the changed-after date must be specified (see line 11).</p>
<blockquote>
<p><em>00</em> Order status has not changed<br />
<em>01</em> Order status has changed</p>
</blockquote></td>
<td>D</td>
<td>OrderStatusChanged</td>
<td></td>
</tr>
<tr class="even">
<td>11</td>
<td>Date after which the order line status has changed or not, depending upon the OrderStatusChanged code. Mandatory if OrderStatusChanged is included, otherwise not used. – YYYYMMDD</td>
<td>D</td>
<td>ChangedAfterDate</td>
<td></td>
</tr>
</tbody>
</table>

*  
Example of a Retrieve Order List Request XML payload using either the
SOAP or the HTTP protocol and the HTTP POST method, in which the request
is for all orders issued from 1 April 2015 onwards:*

\<OrderListRequest version="0.9"

xmlns="http://www.bic.org.uk/librarywebservices/orderList"\>

\<AccountIdentifier\>

\<AccountIDType\>01\</AccountIDType\>

\<IDValue\>12345\</IDValue\>

\</AccountIdentifier\>

\<RequestNumber\>001\</RequestNumber\>

\<IssueDateTime\>20180422T1525\</IssueDateTime\>

\<PeriodStartDate\>20180401\</PeriodStartDate\>

\</OrderListRequest\>

*Equivalent JSON payload:*

{

"OrderListRequest": {

"version": "0.9",

"xmlns": "http://www.bic.org.uk/librarywebservices/orderList",

"AccountIdentifier": {

"AccountIDType": "01",

"IDValue": "12345"

},

"RequestNumber": "001",

"IssueDateTime": "20180422T1525",

"PeriodStartDate": "20180401"

}

}

*Example of a Retrieve Order List Request XML payload using either the
SOAP or the HTTP protocol and the HTTP POST method, in which the request
is for all orders with order numbers that match the regular expression
pattern ‘01020\\d+’ (i.e. numbers beginning ‘01020’):*

\<OrderListRequest version="0.9"

xmlns="http://www.bic.org.uk/librarywebservices/orderList"\>

\<AccountIdentifier\>

\<AccountIDType\>01\</AccountIDType\>

\<IDValue\>12345\</IDValue\>

\</AccountIdentifier\>

\<RequestNumber\>001\</RequestNumber\>

\<IssueDateTime\>20150422T1525\</IssueDateTime\>

\<ReferenceNumberPattern\>01020\\d+\</ReferenceNumberPattern\>

\</OrderListRequest\>

*Equivalent JSON payload:*

{

"OrderListRequest": {

"version": "0.9",

"xmlns": "http://www.bic.org.uk/librarywebservices/orderList",

"AccountIdentifier": {

"AccountIDType": "01",

"IDValue": "12345"

},

"RequestNumber": "001",

"IssueDateTime": "20150422T1525",

"ReferenceNumberPattern": "01020\\\\d+"

}

}

RETRIEVE ORDER LIST – RESPONSE

The Response will use the protocol corresponding to the Request. If the
Request uses the basic HTTP protocol, the Response will be an XML
document as specified below attached to a normal HTTP header. If the
Request uses the SOAP protocol, the Response will contain a SOAP
response message whose body will contain the XML document specified
below.

**Response document name and version**

<table>
<tbody>
<tr class="odd">
<td></td>
<td><strong>Retrieve order list response Version 0.9</strong></td>
<td><h1 id="section-1"></h1></td>
<td><strong>&lt;OrderListResponse version=”0.9”&gt;<br />
{ &quot;OrderListRequest&quot;: { &quot;version&quot;: ,...</strong></td>
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
<td><h1 id="m">M</h1></td>
<td><strong>Header.</strong></td>
<td>[3]</td>
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
<td>Account identifier. Mandatory in all responses.</td>
<td>M</td>
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
<td></td>
</tr>
<tr class="even">
<td></td>
<td><blockquote>
<p>Reference type<br />
<em>01</em> Number or date/time of associated order list request</p>
</blockquote></td>
<td>M</td>
<td>ReferenceTypeCode</td>
<td></td>
</tr>
<tr class="odd">
<td></td>
<td>Reference number / string</td>
<td>M</td>
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
<td><h1 id="m-1">M</h1></td>
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
<td><strong>Details of all orders that meet the selection criteria in the request. Mandatory unless the header reports a condition that prevents any response</strong></td>
<td><h1 id="d">D</h1></td>
<td><strong>ItemDetail.</strong></td>
<td><strong>R</strong></td>
</tr>
<tr class="even">
<td>1</td>
<td>Order list response item line number</td>
<td><h1 id="d-1">D</h1></td>
<td>LineNumber</td>
<td></td>
</tr>
<tr class="odd">
<td>2</td>
<td>Document references. It is mandatory to include the buyer’s order reference in all items. If the order has been partly or completely fulfilled, one or more delivery note references must be included.</td>
<td>M</td>
<td>ReferenceCoded</td>
<td>R</td>
</tr>
<tr class="even">
<td></td>
<td>Reference type code<br />
<em>11</em> Buyer’s order reference<br />
<em>23</em> Supplier’s order reference</td>
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
<td>Reference date or date and time. See Header line 1 for format options.</td>
<td>D</td>
<td>ReferenceDateTime</td>
<td></td>
</tr>
<tr class="odd">
<td>3</td>
<td>Total number of order lines on this order</td>
<td>M</td>
<td>NumberOfLines</td>
<td></td>
</tr>
<tr class="even">
<td>4</td>
<td>Total number of order lines not yet fulfilled on this order</td>
<td>M</td>
<td>NumberOfOpenLines</td>
<td></td>
</tr>
</tbody>
</table>

*  
Example of a Retrieve Order List Response XML payload using either the
SOAP or the HTTP protocol and the HTTP POST method:*

\<OrderListResponse version="0.9"

xmlns="http://www.bic.org.uk/librarywebservices/orderList"\>

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

\<ReferenceTypeCode\>11\</ReferenceTypeCode\>

\<ReferenceNumber\>O1020304\</ReferenceNumber\>

\<ReferenceDateTime\>20180409\</ReferenceDateTime\>

\</ReferenceCoded\>

\<ReferenceCoded\>

\<ReferenceTypeCode\>23\</ReferenceTypeCode\>

\<ReferenceNumber\>DN0123456\</ReferenceNumber\>

\</ReferenceCoded\>

\<NumberOfLines\>10\</NumberOfLines\>

\<NumberOfOpenLines\>5\</NumberOfOpenLines\>

\</ItemDetail\>

\<ItemDetail\>

\<ReferenceCoded\>

\<ReferenceTypeCode\>11\</ReferenceTypeCode\>

\<ReferenceNumber\>O1020405\</ReferenceNumber\>

\<ReferenceDateTime\>20180419\</ReferenceDateTime\>

\</ReferenceCoded\>

\<NumberOfLines\>8\</NumberOfLines\>

\<NumberOfOpenLines\>8\</NumberOfOpenLines\>

\</ItemDetail\>

\</OrderListResponse\>

*  
Example of a Retrieve Order List Response JSON payload using either the
SOAP or the HTTP protocol and the HTTP POST method:*

{

"OrderListResponse": {

"version": "0.9",

"xmlns": "http://www.bic.org.uk/librarywebservices/orderList",

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

"ReferenceTypeCode": "11",

"ReferenceNumber": "O1020304",

"ReferenceDateTime": "20180409"

},

{

"ReferenceTypeCode": "23",

"ReferenceNumber": "DN0123456"

}

\],

"NumberOfLines": 10,

"NumberOfOpenLines": 5

},

{

"ReferenceCoded": {

"ReferenceTypeCode": "11",

"ReferenceNumber": "O1020405",

"ReferenceDateTime": "20180419"

},

"NumberOfLines": 8,

"NumberOfOpenLines": 8

}

\]

}

}

1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

2.  In the third column “M” means mandatory and “D” means dependent upon
    the business or message context.

3.  An ‘R’ in the right-most column means that the element is
    repeatable.
