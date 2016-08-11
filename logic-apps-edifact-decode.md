# Get started with Decode EDIFACT Message

Validates EDI and partner specific properties 

## Create the connection

### Prerequisites

* An Azure account; you can create a [free account](https://azure.microsoft.com/free)
* An integration account is required to use Decode EDIFACT message connector. See details on how to create an [integration account](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-enterprise-integration-accounts/), add [partners](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-enterprise-integration-partners/) and [EDIFACT agreement](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-enterprise-integration-edifact/) to it.

###Connect to Decode EDIFACT Message using the following steps:

1. Create a Logic App.  [Create a logic app](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-create-a-logic-app/) provides an example.

2. This connector does not have any triggers. Use other triggers to start the logic app, such as a Request trigger.  In the Logic App designer, add a trigger and add an action.  Select Show Microsoft managed APIs in the drop down list and then enter “edifact” in the search box.  Select Decode EDIFACT Message

3. If you haven’t previously created any connections to Integration account, you are prompted for the connection details

4. Enter the integration account details.  Properties with an asterisk are required

| Property | Details |

| ---------------------	| -------------------------------------- |

| Connection Name * | Enter any name for your connection |

| Integration Account * | Enter the integration account name; Be sure your integration account and Logic app are in the same Azure location |

Once complete, your coonection details look similar to the following

5. Select Create.

6. Notice the connection has been created.

7. Select EDIFACT flat file message to decode


## EDIFACT Decode does following

* Resolve the agreement by matching the sender qualifier and identifier, and the receiver qualifier and identifier, in the interchange header with those in the properties of an agreement.

* Splits multiple interchanges in a single message into separate.
* Validates the envelope.
* Disassembles the interchange.
* Validates EDI and partner-specific properties. This includes 
	* EDI schema validation
	* Validation of the structure of the interchange envelope.
	* Validation of the envelope against trading partner agreement 
	* Schema validation of the envelope against the control schema.
	* Schema validation of the transaction-set data elements against the message schema.
	* EDI validation performed on transaction-set data elements. This is performed if enabled in the agreement properties.
* Verifies that the interchange, group, and transaction set control numbers are not duplicates, if the checks are enabled in the EDIFACT agreement send settings. 
	* Checks the interchange control number against previously received interchanges. 
	* Checks the group control number against other group control numbers in the interchange. 
	* Checks the transaction set control number against other transaction set control numbers in that group.
* Generates an XML document for each transaction set.
* Converts the entire interchange to XML if the EDIFACT agreement receive settings configured to one of the Preserve Interchange.  
	* Split Interchange as transaction sets - suspend transaction sets on error: Parses each transaction set in an interchange into a separate XML document. If one or more transaction sets in the interchange fail validation, then EDIFACT Decode suspends only those transaction sets. 
	* Split Interchange as transaction sets - suspend interchange on error: Parses each transaction set in an interchange into a separate XML document.  If one or more transaction sets in the interchange fail validation, then EDIFACT Decode suspends the entire interchange.
	* Preserve Interchange - suspend transaction sets on error: Creates an XML document for the entire batched interchange. EDIFACT Decode suspends only those transaction sets that fail validation, while continuing to process all other transaction sets
	* Preserve Interchange - suspend interchange on error: Creates an XML document for the entire batched interchange. EDIFACT Decode suspends the entire interchange, if one or more transaction sets in the interchange fail validation
* Generates a Technical (control) and/or Functional acknowledgment (if configured).
	* A Technical Acknowledgment or the CONTRL ACK reports the results of a syntactical check of the complete received interchange.
	* A functional acknowledgment is used to acknowledge a received interchange, group, and message; accept or reject a received interchange, group, and message; and to list any syntactical errors or unsupported functionality contained in them