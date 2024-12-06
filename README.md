# ArborChat API

## Authentication
Authentication is required via passing an API Key passed in the `x-api-key` header.

## Available APIs
- [Answer API](#answer-api) 
- [Get Quota API](#get-quota-api)

### Answer API 

Perform analysis and answer investment-related questions.

***POST [https://api.arborchat.ai/api/v1/answer](https://api.arborchat.ai/api/v1/answer)***

*Note: Max 5 requests per minute. In some complex questions that lead to multi-level analysis, we might take more than 60s to return an answer. We suggest to allow a timeout of 120s or above.* 

#### Request Body
```
{
    "query": string,
    "forceAnswer": boolean // Determines whether ArborChat should respond to non-investment related questions, even though it's not its primary focus. Default is false.
}
```

#### Response Format
```
{
    "success": boolean,
    "error": string | null, // see "Possible Errors" section
    "result": {
        "id": string,
        "query": string,
        "answer": string, // in markdown format
        "references": [{
            "title": string,
            "url": string
        }],
        "subqueries": [{
            "query" string,
            "answer": string,
            "references": [{
                "title": string,
                "url": string
            }]
        }]
    }
}
```

#### Sample Usage
*cURL*
```curl
curl --location 'https://api.arborchat.ai/api/v1/answer' \
--header 'x-api-key: <API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "query": "Who is the CEO of Tesla?"
}'
```

*Python*
```python
import requests
import json
response = requests.request("POST", "https://api.arborchat.ai/api/v1/answer", headers={
  'x-api-key: '<API_KEY>',
  'Content-Type': 'application/json'
}, data=json.dumps({
  "query": "Who is the CEO of Tesla?"
}))
```

*Javascript*
```javascript
const axios = require('axios');
await axios.request({
  method: 'POST',
  url: 'https://api.arborchat.ai/api/v1/answer',
  headers: { 
    'x-api-key: '<API_KEY>', 
    'Content-Type': 'application/json'
  },
  data: JSON.stringify({
    "query": "Who is the CEO of Tesla?"
  })
}}
```

#### Possible Errors
| Reason | Description |
| --- | --- |
| NonInvestmentRelated | The question is not Investment / Stock / Company related. |
| FailedToAnswer | We are unable to respond to the question with a qualified answer satisfying our self-evaluation checking. |
| ShouldUseScreener | The question consists of multiple stocks screening or comparisons. We suggest users to use the Arbor Stock Screener to achieve a better experience. | 
| ContentFiltered | The question is considered inappropriate usage, and is rejected by ArborChat. Please rephrase or amend the question. |

---

### Get Quota API

Get latest quota information of the account.

***GET [https://api.arborchat.ai/api/v1/quota](https://api.arborchat.ai/api/v1/quota)***

#### Response Format
```
{
    "success": boolean,
    "error": string | null,
    "result": {
        "used": integer,
        "pending": integer,
        "totalLimit": integer, 
        "isExceeded": boolean,
        "start": timestamp,
        "end": timestamp
    }
}
```

#### Sample Usage
```curl
curl --location 'https://api.arborchat.ai/api/v1/quota' \
--header 'x-api-key: <API_KEY>' \
```
