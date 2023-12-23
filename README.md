# AWS-serverless-microservice
# High-Level Design

<img width="615" alt="Screenshot 2023-12-23 at 10 56 25 PM" src="https://github.com/harshshah8/AWS-serverless-microservice/assets/25564369/aede46ef-8963-414c-9e86-3e5efdd0fbf1">

## The lambda function does the following operations on DynamoDB.
## 1. Create

The "Create" operation is responsible for adding new items to the system. This operation is essential for populating the database with fresh data.

## 2. Update

The "Update" operation allows for modifying existing items in the system. This operation is crucial for keeping data up-to-date and reflecting changes over time.

## 3. Read

The "Read" operation retrieves information from the system. It is used to fetch data for various purposes, such as displaying it to users or performing analysis.

## 4. Delete

The "Delete" operation removes items from the system. This operation is important for managing the data lifecycle and ensuring that unnecessary or outdated information is removed.

## 5. Scan

The "Scan" operation involves a comprehensive examination of the system's data. This operation is useful for searching, auditing, or performing any other analysis that requires a thorough examination of the entire dataset.

### The following is a sample request payload for a DynamoDB create item operation:
```json
{
    "operation": "create",
    "tableName": "lambda-function-url",
    "payload": {
        "Item": {
            "id": "1",
            "name": "Harsh"
        }
    }
}
```
### The following is a sample request payload for a DynamoDB read item operation:
```json
{
    "operation": "read",
    "tableName": "lambda-function-url",
    "payload": {
        "Key": {
            "id": "1"
        }
    }
}
```

## AWS lambda function (Python 3.11)
### Lambda Handler

```python
import json
import boto3

def lambda_handler(event, context):
    # TODO implement
    print(event)
    body = json.loads(event['body'])
    print(body)
    operation = body['operation']
    print(operation)
    
    dynamo_obj = boto3.resource('dynamodb').Table(body['tableName'])
    
    operations = {
        'create': lambda x: dynamo_obj.put_item(**x),
        'read': lambda x: dynamo_obj.get_item(**x),
        'update': lambda x: dynamo_obj.update_item(**x),
        'delete': lambda x: dynamo_obj.delete_item(**x),
        'list': lambda x: dynamo_obj.scan(**x),
        'echo': lambda x: x,
        'ping': lambda x: 'pong'
    }
    
    if operation in operations:
        return operations[operation](body.get('payload'))
    else:
        raise ValueError(f"Unrecognized method {operation}")
```
### For set-up read this article: https://medium.com/@harshshah692/introduction-to-aws-lambda-function-url-d036fb805e84



