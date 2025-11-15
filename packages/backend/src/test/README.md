# Local Testing Scripts

These scripts allow you to test Lambda functions locally without deploying to AWS.

## Prerequisites

1. **Build the project first**:
   ```bash
   npm run build
   ```

2. **Set environment variables** (choose one approach):

   ### Option A: LocalStack (Recommended)
   ```bash
   # Start LocalStack first
   localstack start
   
   # Run setup script
   npm run local:setup
   
   # Or set manually:
   export TABLE_NAME=safeart-jobs-dev
   export S3_BUCKET=safeart-posters-local
   export SQS_QUEUE_URL=http://localhost:4566/000000000000/safeart-jobs-dev
   export AWS_ENDPOINT_URL=http://localhost:4566
   export AWS_ACCESS_KEY_ID=test
   export AWS_SECRET_ACCESS_KEY=test
   export AWS_DEFAULT_REGION=us-east-1
   ```

   ### Option B: Real AWS (Integration Testing)
   ```bash
   # Deploy infrastructure first
   cd ../../infrastructure
   cdk deploy --context env=dev
   
   # Get values from stack outputs and set:
   export TABLE_NAME=safeart-jobs-dev
   export S3_BUCKET=<from-stack-output>
   export SQS_QUEUE_URL=<from-stack-output>
   export AWS_REGION=us-east-1
   ```

## Test Job Creator

Tests the job creation flow:
- Downloads poster image
- Uploads to S3
- Creates DynamoDB record
- Sends message to SQS

```bash
npm run test:local:job-creator
```

Or directly:
```bash
node dist/test/job-creator-test.js
```

## Test Worker

Tests the job processing flow:
- Reads job from DynamoDB
- Downloads image from S3
- Runs compliance check
- Updates job status

```bash
# First create a job using job-creator-test
npm run test:local:job-creator

# Then process it (use the jobId from output)
npm run test:local:worker <jobId> <s3Key> <posterHash>
```

Or with default test values:
```bash
npm run test:local:worker
```

## Example Workflow

1. **Start LocalStack**:
   ```bash
   localstack start
   ```

2. **Setup resources**:
   ```bash
   npm run local:setup
   ```

3. **Build**:
   ```bash
   npm run build
   ```

4. **Test job creation**:
   ```bash
   npm run test:local:job-creator
   ```
   
   Note the `jobId` from the output.

5. **Test worker** (use the jobId from step 4):
   ```bash
   npm run test:local:worker <jobId>
   ```

6. **Verify results**:
   ```bash
   # Check DynamoDB
   aws dynamodb get-item \
     --endpoint-url http://localhost:4566 \
     --table-name safeart-jobs-dev \
     --key '{"jobId": {"S": "<jobId>"}}'
   ```

## Troubleshooting

- **"Missing environment variables"**: Make sure you've set all required env vars
- **"Table not found"**: Run `npm run local:setup` or create resources manually
- **"Connection refused"**: Ensure LocalStack is running (`localstack start`)
- **"Access denied"**: Check AWS credentials (for real AWS) or LocalStack config

