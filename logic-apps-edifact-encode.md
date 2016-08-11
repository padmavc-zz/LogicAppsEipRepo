# Get started with Encode EDIFACT Message

Validates EDI and partner specific properties 

## Create the connection

### Prerequisites

* An Azure account; you can create a [free account](https://azure.microsoft.com/free)
* An integration account is required to use Decode EDIFACT message connector. See details on how to create an [integration account](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-enterprise-integration-accounts/), add [partners](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-enterprise-integration-partners/) and [EDIFACT agreement](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-enterprise-integration-edifact/) to it.

### Connect to Decode EDIFACT Message using the following steps:

1. Create a Logic App.  [Create a logic app](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-create-a-logic-app/) provides an example.

2. This connector does not have any triggers. Use other triggers to start the logic app, such as a Request trigger.  In the Logic App designer, add a trigger and add an action.  Select Show Microsoft managed APIs in the drop down list and then enter “edifact” in the search box.  Select either Encode EDIFACT Message by agreement name or Encode to EDIFACT message by identities.

3. If you haven’t previously created any connections to Integration account, you are prompted for the connection details

4. Enter the integration account details.  Properties with an asterisk are required

	| Property | Details |
	| -------- | ------- |
	| Connection Name * | Enter any name for your connection |
	| Integration Account * | Enter the integration account name; Be sure your integration account and Logic app are in the same Azure location 

Once complete, your coonection details look similar to the following

5. Select Create.

6. Notice the connection has been created

#### Encode EDIFACT Message by agreement name

7.	Provide EDIFACT agreement name and xml message to encode.

#### Encode EDIFACT Message by identities

8. Provide sender identifier, sender qualifier, receiver identifier and receiver qualifier as configured in the X12 agreement.  Select xml message to encode


## X12 Encode does following

* Resolve the agreement by matching the sender qualifier and identifier, and the receiver qualifier and identifier, in the interchange header with those in the properties of an agreement.
* Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.
* Applies transaction set header and trailer segments
* Generates an interchange control number, a group control number and a transaction set control number for each outgoing interchange
* Replaces separators in the payload data
* Validates EDI and partner-specific properties. This includes
	* Schema validation of the transaction-set data elements against the message schema.
	* EDI validation performed on transaction-set data elements.
	* Extended validation performed on transaction-set data elements
* Generates an XML document for each transaction set.
* Requests a Technical and/or Functional acknowledgment (if configured).
	* As a technical acknowledgment, the CONTRL message indicates receipt of an interchange.
	* As a functional acknowledgment, the CONTRL message indicates acceptance or rejection of the received interchange, group, or message, with a list of errors or unsupported functionality
