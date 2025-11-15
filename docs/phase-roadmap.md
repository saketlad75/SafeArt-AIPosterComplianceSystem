# Phase Roadmap

This document outlines the implementation phases for the Safeart project.

## Phase 0 – Project & Architecture Setup ✅

**Status**: Complete

- [x] Defined overall architecture (S3 + DynamoDB + SQS + Lambda)
- [x] Set up repo structure (backend, crawler, shared modules)
- [x] Decided deployment stack (AWS CDK)
- [x] Created documentation and diagrams

## Phase 1 – Core Pipeline (Backend) Foundations

**Status**: In Progress

### Tasks

- [x] Define job schema for poster compliance task
- [x] Design data stores (DynamoDB table, S3 buckets)
- [x] Provision core AWS resources (DynamoDB, S3, SQS, Lambda)
- [x] Implement worker Lambda skeleton
- [ ] Manually trigger flow end-to-end with test data

### Next Steps

1. Deploy infrastructure to AWS
2. Create test job manually
3. Verify end-to-end flow works
4. Test error handling and DLQ

## Phase 2 – Job Creation & Caching Logic

**Status**: Partially Complete

### Tasks

- [x] Decide job creation contract
- [x] Implement job creator component
- [x] Add cache-awareness (hash-based lookup)
- [x] Enforce idempotency at job creation
- [ ] Validate repeated requests behave consistently

### Next Steps

1. Test cache hit scenarios
2. Test idempotency with requestId
3. Add integration tests

## Phase 3 – Nova Act Crawler Integration

**Status**: Not Started

### Tasks

- [ ] Clarify crawl scope and targets
- [ ] Implement Nova Act agent
- [ ] Connect crawler output to job creator
- [ ] Verify end-to-end crawler → job creation → processing

### Requirements

- Install Nova Act SDK
- Set up browser automation
- Implement platform-specific extractors
- Add retry logic for failed extractions

## Phase 4 – Scheduling & Continuous Operation

**Status**: Not Started

### Tasks

- [ ] Wrap crawler in scheduled execution model
- [ ] Define back-pressure and rate limits
- [ ] Tune Lambda concurrency and SQS settings
- [ ] Confirm system behavior under realistic load

### Configuration

- EventBridge schedule (every 6 hours)
- Per-platform schedules if needed
- Rate limiting per platform
- Queue depth monitoring

## Phase 5 – Compliance Model & Policy Engine

**Status**: Not Started

### Tasks

- [ ] Choose model(s) for compliance checks
- [ ] Define policy schema
- [ ] Implement compliance engine layer
- [ ] Add caching hooks at compliance layer
- [ ] Document policy logic

### Model Options

- AWS Rekognition (content moderation)
- Amazon Bedrock (Claude/GPT-4 Vision)
- Custom vision models
- OCR for text analysis

## Phase 6 – Observability, Operations & Demo Polish

**Status**: Not Started

### Tasks

- [ ] Add logging and tracing
- [ ] Define metrics and dashboards
- [ ] Configure alerts
- [ ] Add minimal inspection interface
- [ ] Finalize documentation

### Observability Stack

- CloudWatch Logs (structured logging)
- CloudWatch Metrics (custom metrics)
- CloudWatch Alarms (DLQ, errors, latency)
- Optional: X-Ray for tracing

### Inspection Interface

Options:
- Internal API Gateway endpoints
- CLI tool
- Simple dashboard (React/Vue)
- AWS Console queries

## Current Focus

**Phase 1**: Complete end-to-end testing of core pipeline

## Blockers

None currently. Ready to proceed with Phase 1 testing.

