<properties 
	pageTitle="Monitor your Integration Account | Microsoft Azure" 
	description="How to monitor Inegration Account" 
	authors="padmavc" 
	manager="erikre" 
	editor="" 
	services="logic-apps" 
	documentationCenter=""/>

<tags
	ms.service="logic-apps"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="10/21/2016"
	ms.author="padmavc"/>

# Monitor your Integration Account

You can configure your Integration Account to use Azure Diagnostics for more rich details and debugging

1. Select **Monitor** and  click **Diagnostics logs**     
![select Monitor](./media/app-service-logic-monitor-integration-account/Pic1.png)
2. Select your **Subscription** and **Resource Group**, **Integration Account** from Resource Type and select your **Integration Account** from Resource drop down to enable diagnostics.  Click on **Turn on Diagnostics** to enable diagnostcis for the selected Integration Account               
![select Integration Account](./media/app-service-logic-monitor-integration-account/Pic2.png)
3. Select status **ON**          
![Turn diagnostics on](./media/app-service-logic-monitor-integration-account/Pic3.png) 
4. Select **Send to Log Analytics** and configure Log Analytics to emit data               
![Turn diagnostics on](./media/app-service-logic-monitor-integration-account/Pic4.png)


## Trakcing Schema

Currently supporting following track records types 

* TrackingEventDefinition
* AS2Message
* AS2MDN
* X12TransactionSet
* X12TransactionSetAcknowledgment
* X12Interchange
* X12InterchangeAcknowledgment
* X12FunctionalGroup
* X12FunctionalGroupAcknowledgment


All of them will have fixed schemas except Custom type.

## Tracking Event Definion
````java script

        {
            "sourceType": "Required - The type of the run source - Allowed values - Microsoft.Logic/workflows or custom",
            "source": { 
                // If the source type is Microsoft.Logic/workflows then the source information need to follow this schema. This schema will be a JToken if the source type is 'custom'
            "workflow": {
                "systemId": "Required - The system Id of the logic app - type string"
            },
            "runInstance": {
                "runId": "Required - The run Id of the logic app - type string"
            },
            "operation": {
                "operationName": "Required - The name of the operation (action/trigger) - type string",
                "repeatItemScopeName": "Required in case if the action is inside a foreach/until loop - type string",
                "repeatItemIndex": "Required in case if the action is inside a foreach/until loop - type int",
                "trackingId": "Optional - client can populate it to correlate. Recommened to populate the value- type string",
                "correlationId": "Optional - client can populate it to correlate. Recommened to populate the value- type string",
                "clientRequestId": "Optional - client can populate it to correlate. Recommened to populate the value- type string"
                }
            },
            "events": [
            {
                "eventLevel": "Required - The level of the event - Allowed values - Informational",
                "eventTime": "Requied - The time of the event in UTC. format - YYYY-MM-DDTHH:MM:SS.00000Z",
                "recordType": "Required - The type of the track record - Allowed values - AS2Message, AS2MDN, X12Interchange, X12FunctionalGroup, X12TransactionSet, X12InterchangeAcknowledgment, X12FunctionalGroupAcknowledgment, X12TransactionSetAcknowledgment, Custom",
                "record": {
                    // Required - For custom record type this is a JToken, for others see below.
                }
            }
         ]
      }

````


## AS2 Message Tracking Schema
````java script

        {
        "agreementProperties": {
            "senderPartnerName": "Optional - types string",
            "receiverPartnerName": "Optional - type string",
            "as2To": "Required - type string",
            "as2From": "Required - type string",
            "agreementName": "Optional - type string"
        },
        "messageProperties": {
            "direction": "Required - Allowed values - Receive/Send",
            "messageId": "Optional - The AS2 message id - type string",
            "dispositionType": "Optional - The AS2 disposition type - type string",
            "fileName": "Optional - The file name header - type string",
            "isMessageFailed": "Required - type boolean",
            "isMessageSigned": "Required - type boolean",
            "isMessageEncrypted": "Required - type boolean",
            "isMessageCompressed": "Required - type boolean",
            "correlationMessageId": "Optional - the message id, used for correlation - type string",
            "incomingHeaders": { 
                // Optional
                // Dictionary of JTokens
                },
            "outgoingHeaders": { 
                // Optional
                // Dictionary of JTokens
                },
        "isNrrEnabled": "Required - type boolean - default value if not known",
        "isMdnExpected": "Required - type boolean - default value if not known",
        "mdnType": "Required - Allowed values - NotConfigured,Sync,Async"
        }
    }
````

## AS2 MDN Tracking Schema
````java script

        {
            "agreementProperties": {
                "senderPartnerName": "Optional - types string",
                "receiverPartnerName": "Optional - type string",
                "as2To": "Required - type string",
                "as2From": "Required - type string",
                "agreementName": "Optional - type string"
            },
            "mdnProperties": {
                "direction": "Required - Allowed values - Receive/Send",
                "messageId": "Optional - The AS2 message id of MDN- type string",
                "originalMessageId": "Optional - The AS2 original message id for which MDN is generated - type string",
                "dispositionType": "Optional - The AS2 disposition type - type string",
                "isMessageFailed": "Required - type boolean",
                "isMessageSigned": "Required - type boolean",
                "isNrrEnabled": "Required - type boolean - default value if not known",
                "statusCode": "Requires - Allowed values - Accepted/Rejected/AcceptedWithErrros",
                "micVerificationStatus": "Required - Allowed values - NotApplicable/Succeeded/Failed",
                "correlationMessageId": "Optional - the original message id, used for correlation - type string",
                "incomingHeaders": { 
                    // Optional
                    // Dictionary of JTokens
                },
                "outgoingHeaders": { 
                    // Optional
                    // Dictionary of JTokens
                }
            }
        }
        

````

## X12 Transaction Set Tracking Schema
````java script

       {
            "agreementProperties": {
                "senderPartnerName": "Optional - types string",
                "receiverPartnerName": "Optional - type string",
                "senderQualifier": "Required - types string",
                "senderIdentifier": "Required - type string",
                "receiverQualifier": "Required - types string",
                "receiverIdentifier": "Required - type string",
                "agreementName": "Optional - type string"
            },
            "transactionSetProperties": {
                "direction": "Required - Allowed Values - Send/Receive",
                "interchangeControlNumber": "Optional - Interchange control number - type string",
                "functionalGroupControlNumber": "Optional - Functional control number - type string",
                "transactionSetControlNumber": "Optional - Transaction set control number - type string",
                "CorrelationMessageId": "Optional - The correlation message id - type string", // TODO: provide the format
                "messageType": "Optional - The transaction set type - doc type - type string",
                "isMessageFailed": "Required - indicates whether the interchange processing failed or not - type boolean.",
                "isTechnicalAcknowledgmentExpected": "Required - Whether the technical ack is expected or not. - type boolean",
                "isFunctionalAcknowledgmentExpected": "Required - Whether the functional ack is expected or not. - type boolean",
                "needAk2LoopForValidMessages":  "Required - whether the AK2 loop is requird for valid message or not. - type boolean",
                "segmentsCount": "Optional - The no of segemnts in the x12 transaction set - type int"
            }
        }

````

## X12 Transaction Set Acknowledgment Tracking Schema
````java script

        {
            "agreementProperties": {
                "senderPartnerName": "Optional - types string",
                "receiverPartnerName": "Optional - type string",
                "senderQualifier": "Required - types string",
                "senderIdentifier": "Required - type string",
                "receiverQualifier": "Required - types string",
                "receiverIdentifier": "Required - type string",
                "agreementName": "Optional - type string"
            },
            "transactionSetAcknowledgmentProperties": {
                "direction": "Required - Allowed Values - Send/Receive",
                "interchangeControlNumber": "Optional - Interchange control number of the FA - This will get populated only in cases where it is on send side and when we receive a functaional ack, but not when we generate on receive side. - type string",
                "functionalGroupControlNumber": "Optional - functional group control number of the FA - This will get populated only in cases where it is on send side and when we receive a funcational ack, but not when we generate on receive side. - type string",
                "isaSegment": "Optional - Same as above interchange control number will get populated only in specific cases. - type string",
                "gsSegment": "Optional - Same as above functional group control number will get populated only in specific cases. - type string",
                "respondingfunctionalGroupControlNumber": "Optional - Responding  interchange control number - type string",
                "respondingFunctionalGroupId": "Optional - Maps to AK101 in the ack - functional group Id - type string",
                "respondingtransactionSetControlNumber": "Optional - responding transaction set control number - type string",
                "respondingTransactionSetId": "Optional - Maps to AK201 in the ack - transaction set Id - type string",
                "statusCode": "Required - Acknowledgment status code Allowed values - Accepted/Rejected/AccpetedWithErrros",
                "processingStatus": "Required - The processing status of the acknowledgment. Allowed values - Received/Generated/Sent",
                "CorrelationMessageId": "Optional - The correlation message id - type string", // TODO: provide the format
                "isMessageFailed": "Required - indicates whether the interchange processing failed or not - type boolean.",
                "ak2Segment": "Optional - The ak2 segemnt - type string",
                "ak3Segment": "Optional - The ak3 segemnt - type strig",
                "ak5Segment": "Optional - The ak5 segment - type string"
            }
        }       


`````

## X12 Interchange Tracking Schema
````java script

        {
            "agreementProperties": {
                "senderPartnerName": "Optional - types string",
                "receiverPartnerName": "Optional - type string",
                "senderQualifier": "Required - types string",
                "senderIdentifier": "Required - type string",
                "receiverQualifier": "Required - types string",
                "receiverIdentifier": "Required - type string",
                "agreementName": "Optional - type string"
            },
            "interchangeProperties": {
                "direction": "Required - Allowed Values - Send/Receive",
                "interchangeControlNumber": "Optional - Interchange control number - type string",
                "isaSegment": "Optional - ISA segment from the message - type string",
                "isTechnicalAcknowledgmentExpected": "Required - Whether the technical ack is expected or not. - type boolean",
                "isMessageFailed": "Required - indicates whether the interchange processing failed or not - type boolean.",
                "isa09": "Optional - Interchange Date - type string",
                "isa10": "Optional - Interchange Time - type string",
                "isa11": "Optional - Interchange Control Standards ID - type string",
                "isa12": "Optional - Interchange Control Version Number - type string",
                "isa14": "Optional - Field to identify whether the acknowledgment is requested or not - type string",
                "isa15": "Optional - Test/Production Indicator - type string",
                "isa16": "Optional - Sub element Separator - type string"
            }
        }

````

## X12 Interchange Acknowledgment Tracking Schema
````java script

        {
            "agreementProperties": {
                "senderPartnerName": "Optional - types string",
                "receiverPartnerName": "Optional - type string",
                "senderQualifier": "Required - types string",
                "senderIdentifier": "Required - type string",
                "receiverQualifier": "Required - types string",
                "receiverIdentifier": "Required - type string",
                "agreementName": "Optional - type string"
            },
            "technicalAcknowledgmentProperties": {
                "direction": "Required - Allowed values - Send/Receive",
                "interchangeControlNumber": "Optional - Interchange control number of the TA - This will get populated only in cases where it is on send side and when we receive a technical ack, but not when we generate on receive side - type string.",
                "isaSegment": "Optional - Same as above interchange control number will get populated only in specific cases - type string",
                "respondingInterchangeControlNumber": "Optional - Interchange control number of the responding interchange - type string", // See whether string type or long type makes sense. If string, again think about the leading zeros.
                "isMessageFailed": "Required - indicates whether the interchange acknowledgment processing failed or not - type boolean.",
                "statusCode": "Required - Interchange acknowledgment status code. - Allowed values - Accepted/Rejected/AccpetedWithErrros",
                "processingStatus": "Required - The processing status of the acknowledgment. Allowed values - Received/Generated/Sent",
                "ta102": "Optional - The responding interchange date. - type strig",
                "ta103": "Optional - The responding interchange time - type string",
                "ta105": "Optional - The interchange note code - type string"
            }
        }

````

## X12 Functional Group Tracking Schema
````java script

        {
            "agreementProperties": {
                "senderPartnerName": "Optional - types string",
                "receiverPartnerName": "Optional - type string",
                "senderQualifier": "Required - types string",
                "senderIdentifier": "Required - type string",
                "receiverQualifier": "Required - types string",
                "receiverIdentifier": "Required - type string",
                "agreementName": "Optional - type string"
            },
            "functionalGroupProperties": {
                "direction": "Required - Allowed Values - Send/Receive",
                "interchangeControlNumber": "Optional - Interchange control number - type string",
                "functionalGroupControlNumber": "Optional - Functional control number - type string",
                "gsSegment": "Optional - GS segment from the message - type string",
                "isTechnicalAcknowledgmentExpected": "Required - Whether the technical ack is expected or not. - type boolean",
                "isFunctionalAcknowledgmentExpected": "Required - Whether the functional ack is expected or not. - type boolean",
                "isMessageFailed": "Required - indicates whether the interchange processing failed or not - type boolean.",
                "gs01": "Optional - Functional id code- type string",
                "gs02": "Optional - Application senders code - type string",
                "gs03": "Optional - Application receivers code - type string",
                "gs04": "Optional - Functional group data - type string",
                "gs05": "Optional - Functional group time - type string ",
                "gs07": "Optional - Responsible agency code - type string",
                "gs08": "Optional - The version/release identifier code - type string"
            }     
        }

````


## X12 Functional Group Acknowledgement Tracking Schema
````java script 

        {
            "agreementProperties": {
                "senderPartnerName": "Optional - types string",
                "receiverPartnerName": "Optional - type string",
                "senderQualifier": "Required - types string",
                "senderIdentifier": "Required - type string",
                "receiverQualifier": "Required - types string",
                "receiverIdentifier": "Required - type string",
                "agreementName": "Optional - type string"
            },
            "functionalGroupAcknowledgmentProperties": {
                "direction": "Required - Allowed Values - Send/Receive",
                "interchangeControlNumber": "Optional - Interchange control number of the TA - This will get populated only in cases where it is on send side and when we receive a technical ack, but not when we generate on receive side. - type string",
                "functionalGroupControlNumber": "Optional - functional group control number of the TA - This will get populated only in cases where it is on send side and when we receive a technical ack, but not when we generate on receive side. - type string",
                "isaSegment": "Optional - Same as above interchange control number will get populated only in specific cases. - type string",
                "gsSegment": "Optional - Same as above functional group control number will get populated only in specific cases. - type string",
                "respondingfunctionalGroupControlNumber": "Optional - control number of the original functional group - type string",
                "respondingFunctionalGroupId": "Optional - Maps to AK101 in the ack - functional group Id - type string",
                "isMessageFailed": "Required - indicates whether the interchange processing failed or not - type boolean.",
                "statusCode": "Required - Acknowledgment status code. Allowed values Accepted/Rejected/AccpetedWithErrros",
                "processingStatus": "Required - The processing status of the acknowledgment. Allowed Values - Received/Generated/Sent",
                "ak903": "Optional - This indicates the no of transaction sets received - type string",
                "ak904": "Optional - The no of transaction sets accepted in the functional group. - type strig",
                "ak9Segment": "Optional - The ak9 segment - type string"
            }
        }

````

