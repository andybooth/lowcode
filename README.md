# DurableCards

Exploration of using Adaptive Cards (https://adaptivecards.io/) with Durable Entities (https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-entities?tabs=csharp) for stateful micro-UI.

## API

Post an Adaptive Card Template to CreateCard. Optionally include an action using Action.Submit and a JSON Schema to validate the postback. The text response is a guid for use with RenderCard, an HTML rendering of the card template using Bootstrap CSS. On postback, all input is saved to the `attachments` array for data binding in the template, allowing simple data driven interactions, with all state stored in a Durable Entity.

*CreateCard: [POST] http://{host}/api/card*

```
{
    "definition": {
        "template": {
            "type": "AdaptiveCard",
            "version": "1.3",
            "body": [
                {
                    "type": "TextBlock",
                    "$data": "${attachments}",
                    "text": "You sent: ${text}"
                },
                {
                    "type": "Input.Text",
                    "id": "text",
                    "label": "Text"
                }
            ],
            "actions": [
                {
                    "type": "Action.Submit",
                    "title": "Send"
                }
            ]
        },
        "schema": {
            "type": "object",
            "required": [
                "text"
            ],
            "properties": {
                "text": {
                    "type": "string",
                    "minLength": 1,
                    "pattern": "^(?!\\s*$).+"
                }
            }
        }
    }
}
```

*PostCard: [POST] http://{host}/api/card/{id:guid}*

*RenderCard: [GET] http://{host}/api/card/{id:guid}*

*Welcome: [GET] http://{host}7071/api/welcome*