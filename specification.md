

**Version** 1.1.0

The Electrum QR Payment Service describes an interface for supporting payments via scanning of a QR code transactions.

Email [**Electrum Support**](mailto:support@electrum.co.za)

Licensed under [**Apache 2.0**](http://www.apache.org/licenses/LICENSE-2.0.html)








# Security Definitions


### httpBasic




<table>
    <tr>
        <th>type</th>
        <td colspan="2">basic</td>
    </tr>

        <tr>
            <th>description</th>
            <td colspan="2">All requests require HTTP basic authentication, where user name equals the value used in Client.id and password has been agreed with Electrum.</td>
        </tr>

</table>





# Operations








## pay



Requests a payment to be processed via a QR Partner.

**POST /payments**

This request contains conventional payment information (POS information, transaction amount, date etc.) as well as Partner information. If a merchant is unable to supply the Partner information in this request, then the request is directed to an intermediate system which also receives notifications from Partners of QR scans. This intermediate system will match the payment request from the merchant with the scan notification from the Partner using the QR code information common to both messages. The payment request may then be forwarded to the correct Partner for processing.




#### Security




* httpBasic




#### Request


**Content-Type:** application/json


Parameters
<table border="1">
    <tr>
        <th>Name</th>
        <th>Located in</th>
        <th>Required</th>
        <th>Description</th>
        <th>Default</th>
        <th>Schema</th>
    </tr>

<tr>
    <th>body</th>
    <td>body</td>
    <td>yes</td>
    <td>A QR payment request.</td>
    <td></td>
    <td>[PaymentRequest](/specification/definitions/#paymentrequest)
</tr>


</table>



#### Response


**Content-Type:** application/json


| Status Code | Reason      | Response Model |
|-------------|-------------|----------------|
| 201    | Created | [PaymentResponse](/specification/definitions/#paymentresponse)|
| 400    | Bad Request | [ErrorDetail](/specification/definitions/#errordetail)|
| 500    | Internal Server Error | [ErrorDetail](/specification/definitions/#errordetail)|
| 503    | Service Unavailable | [ErrorDetail](/specification/definitions/#errordetail)|
| 504    | Gateway Timeout | [ErrorDetail](/specification/definitions/#errordetail)|



















## confirmPayment



Confirms completion of tender initiated by a payment request.

**POST /payments/confirmations**

This operation confirms that a QR payment transaction has completed successfully between the consumer and the merchant. Such a transaction may may be settled between the merchant and the Partner that processed the payment. Such a transaction cannot be reversed.




#### Security




* httpBasic




#### Request


**Content-Type:** application/json


Parameters
<table border="1">
    <tr>
        <th>Name</th>
        <th>Located in</th>
        <th>Required</th>
        <th>Description</th>
        <th>Default</th>
        <th>Schema</th>
    </tr>

<tr>
    <th>body</th>
    <td>body</td>
    <td>yes</td>
    <td>A QR payment confirmation.</td>
    <td></td>
    <td>[PaymentConfirmation](/specification/definitions/#paymentconfirmation)
</tr>


</table>



#### Response


**Content-Type:** application/json


| Status Code | Reason      | Response Model |
|-------------|-------------|----------------|
| 202    | Accepted | [PaymentConfirmation](/specification/definitions/#paymentconfirmation)|
| 400    | Bad Request | [ErrorDetail](/specification/definitions/#errordetail)|
| 404    | Not Found | [ErrorDetail](/specification/definitions/#errordetail)|
| 500    | Internal Server Error | [ErrorDetail](/specification/definitions/#errordetail)|
| 503    | Service Unavailable | [ErrorDetail](/specification/definitions/#errordetail)|
| 504    | Gateway Timeout | [ErrorDetail](/specification/definitions/#errordetail)|



















## reversePayment



Reverses a payment request that failed or timed out

**POST /payments/reversals**

This operation indicates that the sale did not complete and the payment should be reversed if it took place. Once a payment is reversed it canot be confirmed and need not be settled between the merchant and the QR Partner.




#### Security




* httpBasic




#### Request


**Content-Type:** application/json


Parameters
<table border="1">
    <tr>
        <th>Name</th>
        <th>Located in</th>
        <th>Required</th>
        <th>Description</th>
        <th>Default</th>
        <th>Schema</th>
    </tr>

<tr>
    <th>body</th>
    <td>body</td>
    <td>yes</td>
    <td>A QR payment reversal.</td>
    <td></td>
    <td>[PaymentReversal](/specification/definitions/#paymentreversal)
</tr>


</table>



#### Response


**Content-Type:** application/json


| Status Code | Reason      | Response Model |
|-------------|-------------|----------------|
| 202    | Accepted | [PaymentReversal](/specification/definitions/#paymentreversal)|
| 400    | Bad Request | [ErrorDetail](/specification/definitions/#errordetail)|
| 404    | Not Found | |
| 500    | Internal Server Error | [ErrorDetail](/specification/definitions/#errordetail)|
| 503    | Service Unavailable | [ErrorDetail](/specification/definitions/#errordetail)|
| 504    | Gateway Timeout | [ErrorDetail](/specification/definitions/#errordetail)|



















## createQrCode



Requests a QR Code to display to a customer to be scanned.

**POST /qrCodes**

The customer may scan this code with a Partner&#x27;s application and thereby allow the Partner to identify the QR code provider.




#### Security




* httpBasic




#### Request


**Content-Type:** application/json


Parameters
<table border="1">
    <tr>
        <th>Name</th>
        <th>Located in</th>
        <th>Required</th>
        <th>Description</th>
        <th>Default</th>
        <th>Schema</th>
    </tr>

<tr>
    <th>body</th>
    <td>body</td>
    <td>yes</td>
    <td>Information pertaining to the QR code, which may be available at the time of the request. This may include details such as the entity requesting the QR code, the value of the transaction for which the QR code will be used and the specific purpose of the QR code. The request for a QR code should convey information about the merchant requesting the QR code (e.g. POS information) and not information about the consumer who will scan the QR code (e.g. customer or loyalty information).</td>
    <td></td>
    <td>[CreateQrCodeRequest](/specification/definitions/#createqrcoderequest)
</tr>


</table>



#### Response


**Content-Type:** application/json


| Status Code | Reason      | Response Model |
|-------------|-------------|----------------|
| 201    | Created | [CreateQrCodeResponse](/specification/definitions/#createqrcoderesponse)|
| 400    | Bad Request | [ErrorDetail](/specification/definitions/#errordetail)|
| 500    | Internal Server Error | [ErrorDetail](/specification/definitions/#errordetail)|
| 503    | Service Unavailable | [ErrorDetail](/specification/definitions/#errordetail)|
| 504    | Gateway Timeout | [ErrorDetail](/specification/definitions/#errordetail)|



















## notifyScan



Notify that a QR code has been scanned.

**POST /scans**

Partners are not notified by a QR code provider when a QR code is generated. Only when a consumer scans a QR code using a Partner&#x27;s application is the Partner aware that the QR code is available. The Partner subsequently informs the provider of the QR code that their code has been scanned. The QR code provider shall then associate any other transactions pertaining to the QR code with the Partner.




#### Security




* httpBasic




#### Request


**Content-Type:** application/json


Parameters
<table border="1">
    <tr>
        <th>Name</th>
        <th>Located in</th>
        <th>Required</th>
        <th>Description</th>
        <th>Default</th>
        <th>Schema</th>
    </tr>

<tr>
    <th>body</th>
    <td>body</td>
    <td>yes</td>
    <td>A get QR code request.</td>
    <td></td>
    <td>[ScanNotification](/specification/definitions/#scannotification)
</tr>


</table>



#### Response


**Content-Type:** application/json


| Status Code | Reason      | Response Model |
|-------------|-------------|----------------|
| 202    | Accepted | |
| 400    | Bad Request | [ErrorDetail](/specification/definitions/#errordetail)|
| 500    | Internal Server Error | [ErrorDetail](/specification/definitions/#errordetail)|
| 503    | Service Unavailable | [ErrorDetail](/specification/definitions/#errordetail)|
| 504    | Gateway Timeout | [ErrorDetail](/specification/definitions/#errordetail)|














# Definitions

## Amounts



Amounts which make up the transaction. Absent amounts have zero value.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>requestAmount</td>
    <td>[LedgerAmount](#ledgeramount)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The transaction amount requested by the customer to be authorised or approved. This is the total amount the customer wishes to pay for a service or virtual product.</td>
</tr>
<tr>
    <td>approvedAmount</td>
    <td>[LedgerAmount](#ledgeramount)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The transaction amount which was approved by the upstream entity.</td>
</tr>
<tr>
    <td>feeAmount</td>
    <td>[LedgerAmount](#ledgeramount)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Fees charged by the upstream entity for processing the transaction.</td>
</tr>
<tr>
    <td>balanceAmount</td>
    <td>[LedgerAmount](#ledgeramount)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The remaining balance on the customer's account.</td>
</tr>
<tr>
    <td>additionalAmounts</td>
    <td>object</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Any additional amounts that are involved in a transaction which don't appropriately fit into the other amount fields.</td>
</tr>

</table>


## Barcode



Used to indicate barcode information for a slip line.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>data</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data to be encoded in the barcode</td>
</tr>
<tr>
    <td>encoding</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Specifies the encoding used in the barcode</td>
</tr>

</table>


## CreateQrCodeRequest



A request from the merchant for a QR code to be generated. The QR code returned should be suitable to be displayed to a consumer to be scanned.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The randomly generated UUID identifying this request. This may be a variant 3 or 4 as defined in [RFC 4122](https://tools.ietf.org/html/rfc4122)</td>
</tr>
<tr>
    <td>time</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>format:<i>date-time</i><br/></td>
    <td>The date and time of the message as recorded by the sender. The format shall be as defined for date-time in [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). It is recommended that the optional time-secfrac be included up to millisecond precision</td>
</tr>
<tr>
    <td>originator</td>
    <td>[Originator](#originator)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the originator of the request.</td>
</tr>
<tr>
    <td>client</td>
    <td>[Institution](#institution)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the sender of the request.</td>
</tr>
<tr>
    <td>thirdPartyIdentifiers</td>
    <td>array[[ThirdPartyIdentifier](#thirdpartyidentifier)]</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An array of identifiers which each identify the transaction within each entity's system.</td>
</tr>
<tr>
    <td>rrn</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>This is a reference set by the original source of the request.</td>
</tr>
<tr>
    <td>stan</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The System Trace Audit Number can be used to locate transactions across different systems.</td>
</tr>
<tr>
    <td>amounts</td>
    <td>[Amounts](#amounts)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The amounts pertaining to the QR code to be created.</td>
</tr>

</table>


## CreateQrCodeResponse



The response to a CreateQrCodeRequest which contains the specific code assigned by the QR code provider as well as the full QR code in EMVCo format.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The randomly generated UUID identifying this request. This may be a variant 3 or 4 as defined in [RFC 4122](https://tools.ietf.org/html/rfc4122)</td>
</tr>
<tr>
    <td>time</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>format:<i>date-time</i><br/></td>
    <td>The date and time of the message as recorded by the sender. The format shall be as defined for date-time in [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). It is recommended that the optional time-secfrac be included up to millisecond precision</td>
</tr>
<tr>
    <td>originator</td>
    <td>[Originator](#originator)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the originator of the request.</td>
</tr>
<tr>
    <td>client</td>
    <td>[Institution](#institution)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the sender of the request.</td>
</tr>
<tr>
    <td>thirdPartyIdentifiers</td>
    <td>array[[ThirdPartyIdentifier](#thirdpartyidentifier)]</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An array of identifiers which each identify the transaction within each entity's system.</td>
</tr>
<tr>
    <td>rrn</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>This is a reference set by the original source of the request.</td>
</tr>
<tr>
    <td>stan</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The System Trace Audit Number can be used to locate transactions across different systems.</td>
</tr>
<tr>
    <td>amounts</td>
    <td>[Amounts](#amounts)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The amounts pertaining to the QR code to be created.</td>
</tr>
<tr>
    <td>tranId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The unique transaction identifier assigned by the QR code provider to this QR code. This value is also encoded in the QR code returned in the qrCode field. The QR code provider is responsible for ensuring appropriate uniqueness of the QR code for the appropriate period of time. No specific restrictions are placed on the format of the QR code (length, characters etc.) but implementors should consider the following aspects; ***Length*** - Longer QR codes require more detailed resolution on display screens and scanning devices and are also harder to scan. ***Manual Entry*** - While manual entry of QR codes is not explicitly supported by the QR Payments Service Interface, implementors may choose to support such fallback mechanisms if a QR code cannot be scanned. Longer and more complicated codes will be more susceptible to errors when inputted manually. This value must be provided in subsequent 'notifyScan' and 'pay' operations to link payments to specific Partners.</td>
</tr>
<tr>
    <td>qrCode</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The full set of data to be encoded in the graphical QR code. The data is provided in a Tag-Length-Value format as described in the EMVCo specification but is not a fully EMVCo compliant string e.g. Tags which are mandatory under the EMVCo specification may be omitted. The precise set of Tags to be populated in the QR code should be discussed and agreed upon by implementation partners.</td>
</tr>

</table>


## ErrorDetail



Describes a failed outcome of an operation.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>20</i><br/></td>
    <td>The randomly generated UUID identifying this errorDetail, as defined for a variant 4 UUID in [RFC 4122](https://tools.ietf.org/html/rfc4122).</td>
</tr>
<tr>
    <td>originalId</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The UUID of the original request message in the case of an error occurring for an advice message.</td>
</tr>
<tr>
    <td>errorType</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td><i>[DUPLICATE_RECORD, FORMAT_ERROR, FUNCTION_NOT_SUPPORTED, GENERAL_ERROR, INVALID_AMOUNT, ROUTING_ERROR, TRANSACTION_NOT_SUPPORTED, UNABLE_TO_LOCATE_RECORD, UPSTREAM_UNAVAILABLE, ACCOUNT_ALREADY_SETTLED, INVALID_MERCHANT, DO_NOT_HONOR, DECLINED_BY_PARTNER, DECLINED_BY_ACQUIRER, DECLINED_BY_ISSUER, INSUFFICIENT_FUNDS, INVALID_CARD_NUMBER, CARD_EXPIRED, INVALID_TRAN_ID, PARTNER_UNKNOWN, NO_SCAN_RECEIVED, INVALID_ACCOUNT]</i></td>
    <td>The type of error that occurred. This value should be used for programmatic handling of errors.</td>
</tr>
<tr>
    <td>errorMessage</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>40</i><br/></td>
    <td>A short description of the error. This value should be suitable for display to an operator.</td>
</tr>
<tr>
    <td>detailMessage</td>
    <td>object</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>A free form detailed description of a particular failure condition may optionally be supplied. This information is intended for informational purposes only when investigating the cause of a failure.</td>
</tr>
<tr>
    <td>providerErrorCode</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The error code returned by the service provider if available. Note that this should be used for informational purposes only. Messages displayed on the POS should make use of errorType and errorMessage to ensure a consistent set of responses.</td>
</tr>
<tr>
    <td>providerErrorMsg</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The error message returned by the service provider if available. Note that this should be used for informational purposes only. Messages displayed on the POS should make use of errorType and errorMessage to ensure a consistent set of responses.</td>
</tr>
<tr>
    <td>providerRef</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The reference returned by the service provider if available.</td>
</tr>
<tr>
    <td>tranId</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The unique transaction identifier related to this transaction if available. This is the value returned in the tranId field of the CreateQrCodeResponse or the ScanNotification.</td>
</tr>

</table>


## Institution



Originating, acquiring, processing, or receiving institution details
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The institution's ID. API implementations should take care to set this field as appropriate for the implementation.</td>
</tr>
<tr>
    <td>name</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>40</i><br/></td>
    <td>The institutions's name</td>
</tr>

</table>


## LedgerAmount



An amount object only containing value and currency, and optionally an indicator of DEBIT/CREDIT
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>amount</td>
    <td>integer</td>
    <td>required</td>
    <td></td>
    <td>format:<i>int64</i><br/></td>
    <td>Amount in minor denomination, e.g. R799.95 is encoded as 79995</td>
</tr>
<tr>
    <td>currency</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>pattern:<i>[0-9]{3}</i><br/></td>
    <td>Three digit currency number from ISO 4217, e.g. South African Rand is encoded as 710</td>
</tr>
<tr>
    <td>ledgerIndicator</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[DEBIT, CREDIT]</i></td>
    <td>Indicates whether this amount is a debit or a credit. Only required when the amount can be either a debit or a credit</td>
</tr>

</table>


## Merchant



Merchant related data. Must be included if available
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>merchantType</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>pattern:<i>[0-9]{4}</i><br/></td>
    <td>The assigned four digit merchant category code</td>
</tr>
<tr>
    <td>merchantId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>15</i><br/>minLength:<i>15</i><br/></td>
    <td>The assigned merchant identifier. Also known as card acceptor id</td>
</tr>
<tr>
    <td>merchantName</td>
    <td>[MerchantName](#merchantname)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The name of a merchant</td>
</tr>

</table>


## MerchantName



A container object representing the Merchant Name and Location
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>name</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>23</i><br/></td>
    <td>The merchant or trading as name associated with the merchant</td>
</tr>
<tr>
    <td>city</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>13</i><br/></td>
    <td>The city where the merchant is located</td>
</tr>
<tr>
    <td>region</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>2</i><br/></td>
    <td>The state or region where the merchant is located</td>
</tr>
<tr>
    <td>country</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>2</i><br/></td>
    <td>The country where the merchant is located</td>
</tr>

</table>


## Originator



The Originator object encapsulates data relating to the originator of the transaction
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>institution</td>
    <td>[Institution](#institution)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The institution originating the request, as issued by Electrum</td>
</tr>
<tr>
    <td>terminalId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>maxLength:<i>8</i><br/>minLength:<i>8</i><br/></td>
    <td>The ID that uniquely identifies each device or system in an originator's institution capable of sending requests. Required for transactions initiated from physical card entry or point-of-sale devices</td>
</tr>
<tr>
    <td>merchant</td>
    <td>[Merchant](#merchant)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Merchant data. Required if available</td>
</tr>

</table>


## PaymentConfirmation



Confirm that a previous {@link PaymentRequest} has completed successfully at the POS. Where possible all optional fields should be supplied to ensure smooth processing. If optional fields are not present then processing may require retrieval of the original transaction leading to unnecessary processing overheads.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The randomly generated UUID identifying this advice, as defined for a variant 4 UUID in [RFC 4122](https://tools.ietf.org/html/rfc4122)</td>
</tr>
<tr>
    <td>requestId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The UUID identifying the request that this advice relates to</td>
</tr>
<tr>
    <td>time</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>format:<i>date-time</i><br/></td>
    <td>The date and time of the message as recorded by the sender. The format shall be as defined for date-time in [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). It is recommended that the optional time-secfrac be included up to millisecond precision</td>
</tr>
<tr>
    <td>thirdPartyIdentifiers</td>
    <td>array[[ThirdPartyIdentifier](#thirdpartyidentifier)]</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The unaltered thirdPartyIdentifiers array as supplied in the related BasicResponse message. Required if thirdPartyIdentifiers field was present in the BasicResponse. If no thirdPartyIdentifiers was received in the BasicResponse or no BasicResponse was received then this should be set to the thirdPartyIdentifiers sent in the original request.</td>
</tr>
<tr>
    <td>stan</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The System Trace Audit Number can be used to locate transactions across different systems.</td>
</tr>
<tr>
    <td>rrn</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>This is a reference set by the original source of the transaction.</td>
</tr>
<tr>
    <td>partner</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An echo of the value in the original PaymentRequest.</td>
</tr>
<tr>
    <td>tranId</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An echo of the value in the original PaymentRequest.</td>
</tr>

</table>


## PaymentRequest



A request to effect a payment with a linked QR code. Such requests originate from the Merchant&#x27;s system and are typically directed to the Partner for processing. If the Partner for a PaymentRequest is not known, then the PaymentRequest may be directed to an intermediate system which receives ScanNotification messages from Partners. This intermediate system is then responsible for identifying the correct Partner to which a PaymentRequest should be directed.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The randomly generated UUID identifying this transaction, as defined for a variant 4 UUID in [RFC 4122](https://tools.ietf.org/html/rfc4122)</td>
</tr>
<tr>
    <td>time</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>format:<i>date-time</i><br/></td>
    <td>The date and time of the message as recorded by the sender. The format shall be as defined for date-time in [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). It is recommended that the optional time-secfrac be included up to millisecond precision</td>
</tr>
<tr>
    <td>originator</td>
    <td>[Originator](#originator)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the originator of the transaction.</td>
</tr>
<tr>
    <td>client</td>
    <td>[Institution](#institution)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the sender of Transaction.</td>
</tr>
<tr>
    <td>settlementEntity</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity with whom the Merchant will settle the transaction.</td>
</tr>
<tr>
    <td>receiver</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity which ultimately processes the request.</td>
</tr>
<tr>
    <td>thirdPartyIdentifiers</td>
    <td>array[[ThirdPartyIdentifier](#thirdpartyidentifier)]</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>An array of identifiers which each identify the transaction within each entity's system.</td>
</tr>
<tr>
    <td>slipData</td>
    <td>[SlipData](#slipdata)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Text to be printed on the customer receipt.</td>
</tr>
<tr>
    <td>basketRef</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Used to group multiple transactions which would otherwise be considered independent.</td>
</tr>
<tr>
    <td>tranType</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[GOODS_AND_SERVICES, CASH_WITHDRAWAL, DEBIT_ADJUSTMENT, GOODS_AND_SERVICES_WITH_CASH_BACK, NON_CASH, RETURNS, DEPOSIT, CREDIT_ADJUSTMENT, GENERAL_CREDIT, AVAILABLE_FUNDS_INQUIRY, BALANCE_INQUIRY, GENERAL_INQUIRY, CARD_VERIFICATION_INQUIRY, CARDHOLDER_ACCOUNTS_TRANSFER, GENERAL_TRANSFER, PAYMENT_FROM_ACCOUNT, GENERAL_PAYMENT, PAYMENT_TO_ACCOUNT, PAYMENT_FROM_ACCOUNT_TO_ACCOUNT, PLACE_HOLD_ON_CARD, GENERAL_ADMIN, CHANGE_PIN, CARD_HOLDER_INQUIRY, POINTS_INQUIRY]</i></td>
    <td>Data relating to the type of transaction taking place (i.e. cash withdrawal, goods and services etc.).</td>
</tr>
<tr>
    <td>srcAccType</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[DEFAULT, SAVINGS, CHEQUE, CREDIT, UNIVERSAL, ELECTRONIC_PURSE, GIFT_CARD, STORED_VALUE]</i></td>
    <td>This specifies the type of source account being used in the transaction (i.e. cheque, savings).</td>
</tr>
<tr>
    <td>destAccType</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[DEFAULT, SAVINGS, CHEQUE, CREDIT, UNIVERSAL, ELECTRONIC_PURSE, GIFT_CARD, STORED_VALUE]</i></td>
    <td>This specifies the type of destination account being used in the transaction (i.e. cheque, savings).</td>
</tr>
<tr>
    <td>stan</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The System Trace Audit Number can be used to locate transactions across different systems.</td>
</tr>
<tr>
    <td>rrn</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>This is a reference set by the original source of the transaction.</td>
</tr>
<tr>
    <td>partner</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity who will process the payment. This identifies the entity who provided the ScanNotification for the QR code associated with this PaymentRequest. This should be populated if known to aid in routing the PaymentRequest to the entity which provided the ScanNotification.</td>
</tr>
<tr>
    <td>amounts</td>
    <td>[Amounts](#amounts)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The amounts pertaining to the transaction. Note that the requestAmount herein maybe be different to that submitted when the QR code was requested. This request amount describes the actual amount to be processed in the transaction.</td>
</tr>
<tr>
    <td>tranId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The unique transaction identifier related to this transaction. Retailers must set this to the same value as that returned in the tranId field of the CreateQrCodeResponse that preceded this PaymentRequest. Partners may associate this PaymentRequest with the QR code whose ScanNotification they submitted with this value.</td>
</tr>
<tr>
    <td>partnerPaymentToken</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>A payment token received from the Partner in the ScanNotification. A Partner may provide such a value in the ScanNotification so that it is included in the PaymentRequest to the Partner. This field should be populated if known. A Partner may expect to receive this value in the PaymentRequest if it was provided in the ScanNotification.</td>
</tr>

</table>


## PaymentResponse



The response to a successful payment with a linked QR code scan.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The randomly generated UUID identifying this transaction, as defined for a variant 4 UUID in [RFC 4122](https://tools.ietf.org/html/rfc4122)</td>
</tr>
<tr>
    <td>time</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>format:<i>date-time</i><br/></td>
    <td>The date and time of the message as recorded by the sender. The format shall be as defined for date-time in [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). It is recommended that the optional time-secfrac be included up to millisecond precision</td>
</tr>
<tr>
    <td>originator</td>
    <td>[Originator](#originator)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the originator of the transaction.</td>
</tr>
<tr>
    <td>client</td>
    <td>[Institution](#institution)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the sender of Transaction.</td>
</tr>
<tr>
    <td>settlementEntity</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity with whom the Merchant will settle the transaction.</td>
</tr>
<tr>
    <td>receiver</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity which ultimately processes the request.</td>
</tr>
<tr>
    <td>thirdPartyIdentifiers</td>
    <td>array[[ThirdPartyIdentifier](#thirdpartyidentifier)]</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>An array of identifiers which each identify the transaction within each entity's system.</td>
</tr>
<tr>
    <td>slipData</td>
    <td>[SlipData](#slipdata)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Text to be printed on the customer receipt.</td>
</tr>
<tr>
    <td>basketRef</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Used to group multiple transactions which would otherwise be considered independent.</td>
</tr>
<tr>
    <td>tranType</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[GOODS_AND_SERVICES, CASH_WITHDRAWAL, DEBIT_ADJUSTMENT, GOODS_AND_SERVICES_WITH_CASH_BACK, NON_CASH, RETURNS, DEPOSIT, CREDIT_ADJUSTMENT, GENERAL_CREDIT, AVAILABLE_FUNDS_INQUIRY, BALANCE_INQUIRY, GENERAL_INQUIRY, CARD_VERIFICATION_INQUIRY, CARDHOLDER_ACCOUNTS_TRANSFER, GENERAL_TRANSFER, PAYMENT_FROM_ACCOUNT, GENERAL_PAYMENT, PAYMENT_TO_ACCOUNT, PAYMENT_FROM_ACCOUNT_TO_ACCOUNT, PLACE_HOLD_ON_CARD, GENERAL_ADMIN, CHANGE_PIN, CARD_HOLDER_INQUIRY, POINTS_INQUIRY]</i></td>
    <td>Data relating to the type of transaction taking place (i.e. cash withdrawal, goods and services etc.).</td>
</tr>
<tr>
    <td>srcAccType</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[DEFAULT, SAVINGS, CHEQUE, CREDIT, UNIVERSAL, ELECTRONIC_PURSE, GIFT_CARD, STORED_VALUE]</i></td>
    <td>This specifies the type of source account being used in the transaction (i.e. cheque, savings).</td>
</tr>
<tr>
    <td>destAccType</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[DEFAULT, SAVINGS, CHEQUE, CREDIT, UNIVERSAL, ELECTRONIC_PURSE, GIFT_CARD, STORED_VALUE]</i></td>
    <td>This specifies the type of destination account being used in the transaction (i.e. cheque, savings).</td>
</tr>
<tr>
    <td>stan</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The System Trace Audit Number can be used to locate transactions across different systems.</td>
</tr>
<tr>
    <td>rrn</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>This is a reference set by the original source of the transaction.</td>
</tr>
<tr>
    <td>partner</td>
    <td>[Institution](#institution)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity who processed the PaymentRequest. This identifies the entity who provided the ScanNotification for the QR code associated with this payment.</td>
</tr>
<tr>
    <td>tenders</td>
    <td>array[[Tender](#tender)]</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An array of tenders used to pay for the transaction. This may be used to describe the payment which was effected as a result of the QR code scan e.g. the card detail ultimately used for the payment.</td>
</tr>
<tr>
    <td>amounts</td>
    <td>[Amounts](#amounts)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The amounts pertaining to the transaction.</td>
</tr>
<tr>
    <td>tranId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>This value is echoed from the PaymentRequest.</td>
</tr>
<tr>
    <td>partnerPaymentToken</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>This value is echoed from the PaymentRequest.</td>
</tr>

</table>


## PaymentReversal



Reverse a previous PaymentRequest. This may be due to a cancellation at the POS or because the original PaymentRequest failed or is in an unknown state. Where possible all optional fields should be supplied to ensure smooth processing. If optional fields are not present then processing may require retrieval of the original transaction leading to unnecessary processing overheads.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The randomly generated UUID identifying this advice, as defined for a variant 4 UUID in [RFC 4122](https://tools.ietf.org/html/rfc4122)</td>
</tr>
<tr>
    <td>requestId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The UUID identifying the request that this advice relates to</td>
</tr>
<tr>
    <td>time</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>format:<i>date-time</i><br/></td>
    <td>The date and time of the message as recorded by the sender. The format shall be as defined for date-time in [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). It is recommended that the optional time-secfrac be included up to millisecond precision</td>
</tr>
<tr>
    <td>thirdPartyIdentifiers</td>
    <td>array[[ThirdPartyIdentifier](#thirdpartyidentifier)]</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The unaltered thirdPartyIdentifiers array as supplied in the related BasicResponse message. Required if thirdPartyIdentifiers field was present in the BasicResponse. If no thirdPartyIdentifiers was received in the BasicResponse or no BasicResponse was received then this should be set to the thirdPartyIdentifiers sent in the original request.</td>
</tr>
<tr>
    <td>stan</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The System Trace Audit Number can be used to locate transactions across different systems.</td>
</tr>
<tr>
    <td>rrn</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>This is a reference set by the original source of the transaction.</td>
</tr>
<tr>
    <td>reversalReason</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td><i>[TIMEOUT, CANCELLED, RESPONSE_NOT_FINAL]</i></td>
    <td>The reason for the reversal</td>
</tr>
<tr>
    <td>partner</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An echo of the value in the original PaymentRequest.</td>
</tr>
<tr>
    <td>tranId</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An echo of the value in the original PaymentRequest.</td>
</tr>

</table>


## ScanNotification



A notification sent by the Partner indicating that the Partner received a scan of the QR code linked to the transaction ID. Any PaymentRequest with a matching tranId value should be forwarded to the Partner for processing.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>id</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The randomly generated UUID identifying this notification. This may be a variant 3 or 4 as defined in [RFC 4122](https://tools.ietf.org/html/rfc4122)</td>
</tr>
<tr>
    <td>time</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td>format:<i>date-time</i><br/></td>
    <td>The date and time of the message as recorded by the sender. The format shall be as defined for date-time in [RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). It is recommended that the optional time-secfrac be included up to millisecond precision</td>
</tr>
<tr>
    <td>partner</td>
    <td>[Institution](#institution)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity whose customer scanned a QR code. PaymentRequest messages which have a matching tranId value should be be sent to the Partner for processing.</td>
</tr>
<tr>
    <td>settlementEntity</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity with whom the Merchant will settle the transaction. A Partner may provide this information if known at the time the QR code was scanned.</td>
</tr>
<tr>
    <td>receiver</td>
    <td>[Institution](#institution)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Data relating to the entity which ultimately processes the request. A Partner may provide this information if known at the time the QR code was scanned.</td>
</tr>
<tr>
    <td>thirdPartyIdentifiers</td>
    <td>array[[ThirdPartyIdentifier](#thirdpartyidentifier)]</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An array of identifiers which identify the transaction within each entity's system.</td>
</tr>
<tr>
    <td>amounts</td>
    <td>[Amounts](#amounts)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>The amounts pertaining to the QR code which was scanned.</td>
</tr>
<tr>
    <td>tranId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The transaction identifier encoded within the QR Code which was scanned. Any PaymentRequest with a matching tranId value should be forwarded to the Partner for processing.</td>
</tr>
<tr>
    <td>partnerPaymentToken</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>A payment token received from the partner in the ScanNotification. If supplied by the Partner then it will be echoed in the PaymentRequest to the Partner.</td>
</tr>

</table>


## SlipData



Data that may be printed on the customer slip for information purposes
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>messageLines</td>
    <td>array[[SlipLine](#slipline)]</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>An array of text lines and optional formatting to be printed on the customer slip.</td>
</tr>
<tr>
    <td>slipWidth</td>
    <td>integer</td>
    <td>optional</td>
    <td></td>
    <td>format:<i>int32</i><br/></td>
    <td>The width of the slip in normal (unformatted) characters.</td>
</tr>
<tr>
    <td>issuerReference</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td>pattern:<i>[A-Z0-9]{1,40}</i><br/></td>
    <td>An identifier that is printed on the customer slip and uniquely identifies the payment on the service provider's system. This value is used by the customer to request a refund when the service supports this function, and it is thus important that this number is unique.</td>
</tr>

</table>


## SlipLine



A line of text to be printed on the till slip
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>barcode</td>
    <td>[Barcode](#barcode)</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Barcode information for this line</td>
</tr>
<tr>
    <td>text</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>Text contained on the line</td>
</tr>
<tr>
    <td>fontWidthScaleFactor</td>
    <td>number</td>
    <td>optional</td>
    <td></td>
    <td>format:<i>double</i><br/></td>
    <td>Scale factor for font width. Assume 1.0 (i.e. normal size) if not present.</td>
</tr>
<tr>
    <td>fontHeightScaleFactor</td>
    <td>number</td>
    <td>optional</td>
    <td></td>
    <td>format:<i>double</i><br/></td>
    <td>Scale factor for font height. Assume 1.0 (i.e. normal size) if not present.</td>
</tr>
<tr>
    <td>line</td>
    <td>boolean</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Denotes a solid line on the slip. Assume false if not present.</td>
</tr>
<tr>
    <td>cut</td>
    <td>boolean</td>
    <td>optional</td>
    <td></td>
    <td></td>
    <td>Indicates the slip should be cut at this line. Assume false if not present.</td>
</tr>

</table>


## Tender



Details of the Tender used by a customer towards a payment
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>accountType</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td><i>[DEFAULT, SAVINGS, CHEQUE, CREDIT, UNIVERSAL, ELECTRONIC_PURSE, STORED_VALUE]</i></td>
    <td>The type of account</td>
</tr>
<tr>
    <td>amount</td>
    <td>[LedgerAmount](#ledgeramount)</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The tendered amount</td>
</tr>
<tr>
    <td>cardNumber</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td>pattern:<i>[0-9]{6}[0-9*]{0,13}</i><br/></td>
    <td>A PCI compliant masked card number, with at least the first 6 digits in the clear. Only applicable to card based transactions</td>
</tr>
<tr>
    <td>reference</td>
    <td>string</td>
    <td>optional</td>
    <td></td>
    <td>maxLength:<i>40</i><br/></td>
    <td>A free text reference</td>
</tr>
<tr>
    <td>tenderType</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td><i>[CASH, CHEQUE, CREDIT_CARD, DEBIT_CARD, WALLET, ROUNDING, GIFT_CARD, LOYALTY_CARD, OTHER]</i></td>
    <td>The type of tender used</td>
</tr>

</table>


## ThirdPartyIdentifier



An identifier assigned by an entity which process the message. Identifiers are keyed by institution ID thereby enabling any institution to recall a transaction within the entity&#x27;s own system using the entity&#x27;s own identifier. Entity&#x27;s must not alter the identifier set by another entity. Once an identifier has been set by an entity, all other entity&#x27;s must send that identifier in subsequent messages.
<table border="1">
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Required</th>
        <th>Default</th>
        <th>Restrictions</th>
        <th>Description</th>
    </tr>
<tr>
    <td>institutionId</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The entity's institution ID.</td>
</tr>
<tr>
    <td>transactionIdentifier</td>
    <td>string</td>
    <td>required</td>
    <td></td>
    <td></td>
    <td>The identifier assigned to this transaction by the institution represented in institutionId. This value should be unique within the institution's system.</td>
</tr>

</table>


