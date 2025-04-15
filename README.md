# 🚀 SERVERLESS-PROJECT

A cloud-native, serverless web application leveraging modern AWS services for scalable, cost-efficient, and high-performance deployment.

## 📌 Project Overview

This project demonstrates a serverless architecture using:
- **AWS Lambda** for backend logic
- **API Gateway** for RESTful endpoints
- **DynamoDB** for NoSQL storage
- **Amazon S3** for hosting static frontend
- **Route 53** and **CloudFront** for custom domain & CDN
- **CloudWatch** for monitoring and auto-scaling

## 🧩 Features

- ✅ Fully serverless architecture
- 🔐 Secure access via IAM roles and policies
- ⚙️ REST API with CRUD functionality
- 📈 Auto-scaling based on load
- 💾 Persistent data with DynamoDB
- 🌐 Fast, global content delivery via CloudFront
- 🧑‍💻 User-friendly interface (React-based frontend)

##Lambda code
import json
import boto3
import uuid
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('busbookings')  # Make sure this is the correct table name

def lambda_handler(event, context):
    try:
        data = json.loads(event['body'])

        booking_id = str(uuid.uuid4())
        user_id = data['userId']
        bus_id = data['busId']
        booking_time = datetime.utcnow().isoformat()

        item = {
            'bookingid': booking_id,         
            'userId': user_id,
            'busId': bus_id,
            'bookingTime': booking_time
        }

        print("DEBUG - Item to put in DynamoDB:")
        print(json.dumps(item))

        response = table.put_item(Item=item)

        print("DEBUG - DynamoDB response:")
        print(response)

        return {
            'statusCode': 200,
            'body': json.dumps({
                'message': 'Booking successful',
                'bookingId': booking_id
            })
        }

    except Exception as e:
        print("ERROR:", str(e))
        return {
            'statusCode': 500,
            'body': json.dumps({
                'error': str(e)
            })
        }
bus booking code

