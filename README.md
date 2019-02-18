# My Amazon SQS samples

## Create queue

```shell
$ aws sqs create-queue --queue-name my-test-queue --attributes ReceiveMessageWaitTimeSeconds=20
{
    "QueueUrl": "https://ap-northeast-1.queue.amazonaws.com/000000000000/my-test-queue"
}
```

## List queues

```shell
$ aws sqs list-queues
{
    "QueueUrls": [
        "https://ap-northeast-1.queue.amazonaws.com/000000000000/my-test-queue"
    ]
}
```

### Get queue URL

```shell
$ QUEUE_URL=$(aws sqs get-queue-url --queue-name my-test-queue |  npx jqf --raw-string-output 'x => x.QueueUrl')
```

## Send message

```shell
$ aws sqs send-message --queue-url $QUEUE_URL --message-body '{"hello":"SQS"}'
{
    "MD5OfMessageBody": "23759ae80d00f2b3e9c5eb026b74fdd8",
    "MessageId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

## Receive message

```shell
$ aws sqs receive-message --queue-url $QUEUE_URL
{
    "Messages": [
        {
            "MessageId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ReceiptHandle": "<base 64 value>",
            "MD5OfBody": "23759ae80d00f2b3e9c5eb026b74fdd8",
            "Body": "{\"hello\":\"SQS\"}"
        }
    ]
}
```

## Delete message

```shell
$ aws sqs delete-message --receipt-handle "<base 64 value>" --queue-url $QUEUE_URL
```

## Receive message when polling

```shell
# shell A
$ QUEUE_URL=$(aws sqs get-queue-url --queue-name my-test-queue |  npx jqf --raw-string-output 'x => x.QueueUrl')
$ aws sqs receive-message --queue-url $QUEUE_URL
```

```shell
# shell B
QUEUE_URL=$(aws sqs get-queue-url --queue-name my-test-queue |  npx jqf --raw-string-output 'x => x.QueueUrl')
$ aws sqs send-message --queue-url $QUEUE_URL --message-body '{"hello":"SQS"}'
```

## Delete queue

```shell
$ aws sqs delete-queue --queue-url $QUEUE_URL
```
