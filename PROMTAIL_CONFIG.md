# Promtail Configuration Update

## Overview
This configuration sets up Promtail to collect and process logs from Kubernetes pods, with specific focus on HTTP/Log fields and Adapter Response fields.

## Key Components

### 1. Basic Configuration
- Log level: `info`
- Server port: `3101`
- Loki push endpoint: `http://prometheus-XXX-loki-gateway/loki/api/v1/push`

### 2. Pipeline Stages
The configuration implements a multi-stage pipeline for log processing:

#### a. Container Runtime Interface (CRI) Stage
- Initial parsing of container logs using the CRI format

#### b. JSON Stage
- Extracts `message` and `loglevel` fields from JSON-formatted logs

#### c. Log Level Filter
- Regex pattern to identify and keep logs with standard log levels:
  - info, warning, error, debug, severe, finest, audit, fine

#### d. Message Template
- Processes the extracted message field

#### e. HTTP/Log Fields Extraction
Extracts and labels the following HTTP-related fields:
- `httpMethod`: HTTP method used
- `requestURI`: Request URI
- `statusCode`: HTTP status code
- `serviceName`: Name of the service
- `responseTime`: Response time in milliseconds
- `severity`: Log severity
- `url`: Request URL

#### f. Adapter Response Fields Extraction
Extracts and labels the following adapter response fields:
- `origMethodOfPayment`: Original payment method
- `methodOfPayment`: Payment method
- `responseCode`: Response code
- `accountNo`: Account number
- `bban`: Basic Bank Account Number
- `currency`: Currency code
- `iban`: International Bank Account Number
- `residencyFlag`: Residency flag
- `coreBankIdentifier`: Core bank identifier
- `bicfi`: Bank Identifier Code
- `amount`: Transaction amount
- `account`: Account information

### 3. Kubernetes Configuration
- Uses pod discovery for log collection
- Drops logs from pods in "Succeeded" or "Failed" states

### 4. Node Selection and Tolerations
- Targets Linux nodes only
- Includes tolerations for:
  - Critical addons
  - Monitoring components

## Usage in Grafana
The extracted fields can be used in Grafana queries for:
- Filtering logs by specific fields
- Creating dashboards with relevant metrics
- Monitoring HTTP traffic and adapter responses
- Tracking payment-related information

## Example Queries
```logql
# HTTP-related queries
{statusCode="200"}
{httpMethod="POST"}
{serviceName="payment-service"}

# Adapter response queries
{responseCode="SUCCESS"}
{methodOfPayment="CARD"}
{currency="USD"}
```

## Benefits
1. Structured log processing
2. Enhanced searchability in Grafana
3. Better monitoring capabilities
4. Improved debugging experience
5. Comprehensive payment tracking 