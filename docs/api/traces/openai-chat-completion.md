# Sending OpenAI Chat Completion Traces

This guide explains how to send OpenTelemetry-compatible traces for OpenAI chat completions to Langtrace using cURL.

## Prerequisites

- Langtrace API Key
- OpenAI API Key (for comparison with SDK usage)

## Endpoint

```
POST http://localhost:3000/api/trace  # For local development
# or
POST https://api.langtrace.ai/api/trace  # For production
```

## Headers

```
Content-Type: application/json
x-api-key: YOUR_LANGTRACE_API_KEY
User-Agent: opentelemetry-python
```

## Trace Format

The trace must follow the OpenTelemetry format and include specific attributes for OpenAI chat completions. Note that attributes must use the OpenTelemetry value format with `stringValue` or `intValue`:

```json
{
  "resourceSpans": [{
    "resource": {
      "attributes": [{
        "key": "service.name",
        "value": { "stringValue": "your-service-name" }
      }]
    },
    "scopeSpans": [{
      "scope": {
        "name": "openai",
        "version": "1.0.0"
      },
      "spans": [{
        "name": "OpenAI ChatCompletion",
        "kind": 2,  // SpanKind.CLIENT
        "startTimeUnixNano": "1234567890000000000",
        "endTimeUnixNano": "1234567891000000000",
        "traceId": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
        "spanId": "a1b2c3d4e5f6g7h8",
        "attributes": [{
          "key": "llm.vendor",
          "value": { "stringValue": "openai" }
        }, {
          "key": "llm.request.model",
          "value": { "stringValue": "gpt-3.5-turbo" }
        }, {
          "key": "llm.request.messages",
          "value": { "stringValue": "[{\"role\": \"user\", \"content\": \"Hello\"}]" }
        }, {
          "key": "llm.path",
          "value": { "stringValue": "/v1/chat/completions" }
        }, {
          "key": "llm.request.temperature",
          "value": { "intValue": 0.7 }
        }, {
          "key": "llm.request.max_tokens",
          "value": { "intValue": 150 }
        }, {
          "key": "llm.usage.prompt_tokens",
          "value": { "intValue": 10 }
        }, {
          "key": "llm.usage.completion_tokens",
          "value": { "intValue": 15 }
        }, {
          "key": "llm.usage.total_tokens",
          "value": { "intValue": 25 }
        }]
      }]
    }]
  }]
}
```

## Example cURL Command

```bash
curl -X POST "http://localhost:3000/api/trace" \
  -H "Content-Type: application/json" \
  -H "x-api-key: ${LANGTRACE_API_KEY}" \
  -H "User-Agent: opentelemetry-python" \
  -d '{
    "resourceSpans": [{
      "resource": {
        "attributes": [{
          "key": "service.name",
          "value": { "stringValue": "openai-test" }
        }]
      },
      "scopeSpans": [{
        "scope": {
          "name": "openai",
          "version": "1.0.0"
        },
        "spans": [{
          "name": "OpenAI ChatCompletion",
          "kind": 2,
          "startTimeUnixNano": "'$(date +%s%N)'",
          "endTimeUnixNano": "'$(date +%s%N)'",
          "traceId": "'$(openssl rand -hex 16)'",
          "spanId": "'$(openssl rand -hex 8)'",
          "attributes": [{
            "key": "llm.vendor",
            "value": { "stringValue": "openai" }
          }, {
            "key": "llm.request.model",
            "value": { "stringValue": "gpt-3.5-turbo" }
          }, {
            "key": "llm.request.messages",
            "value": { "stringValue": "[{\"role\": \"user\", \"content\": \"Hello\"}]" }
          }, {
            "key": "llm.path",
            "value": { "stringValue": "/v1/chat/completions" }
          }, {
            "key": "llm.request.temperature",
            "value": { "intValue": 0.7 }
          }, {
            "key": "llm.request.max_tokens",
            "value": { "intValue": 150 }
          }, {
            "key": "llm.usage.prompt_tokens",
            "value": { "intValue": 10 }
          }, {
            "key": "llm.usage.completion_tokens",
            "value": { "intValue": 15 }
          }, {
            "key": "llm.usage.total_tokens",
            "value": { "intValue": 25 }
          }]
        }]
      }]
    }]
  }'
```

## Required Attributes

| Attribute | Description | Required |
|-----------|-------------|----------|
| llm.vendor | Service provider ("openai") | Yes |
| llm.request.model | Model name (e.g., "gpt-3.5-turbo") | Yes |
| llm.request.messages | JSON string of messages array | Yes |
| llm.path | API endpoint path | Yes |
| llm.request.temperature | Temperature setting | No |
| llm.request.max_tokens | Maximum tokens | No |
| llm.usage.* | Token usage statistics | No |

## Function Calling Example

When using OpenAI function calling, include the tools in the attributes with proper value formatting:

```bash
curl -X POST "http://localhost:3000/api/trace" \
  -H "Content-Type: application/json" \
  -H "x-api-key: ${LANGTRACE_API_KEY}" \
  -H "User-Agent: opentelemetry-python" \
  -d '{
    "resourceSpans": [{
      "resource": {
        "attributes": [{
          "key": "service.name",
          "value": { "stringValue": "openai-test" }
        }]
      },
      "scopeSpans": [{
        "scope": {
          "name": "openai",
          "version": "1.0.0"
        },
        "spans": [{
          "name": "OpenAI ChatCompletion",
          "kind": 2,
          "startTimeUnixNano": "'$(date +%s%N)'",
          "endTimeUnixNano": "'$(date +%s%N)'",
          "traceId": "'$(openssl rand -hex 16)'",
          "spanId": "'$(openssl rand -hex 8)'",
          "attributes": [{
            "key": "llm.vendor",
            "value": { "stringValue": "openai" }
          }, {
            "key": "llm.request.model",
            "value": { "stringValue": "gpt-3.5-turbo" }
          }, {
            "key": "llm.request.messages",
            "value": { "stringValue": "[{\"role\": \"user\", \"content\": \"What's the weather?\"}]" }
          }, {
            "key": "llm.path",
            "value": { "stringValue": "/v1/chat/completions" }
          }, {
            "key": "llm.tools",
            "value": { "stringValue": "[{\"type\": \"function\", \"function\": {\"name\": \"get_weather\", \"arguments\": \"{\\\"location\\\": \\\"London\\\"}\"}}]" }
          }]
        }]
      }]
    }]
  }'
```

## Response Format

### Success Response
```json
{
  "message": "Traces added successfully"
}
```

### Error Response
```json
{
  "name": "Error name",
  "message": "Error message",
  "stack": "Error stack trace",
  "fullError": "Detailed error information"
}
```

## Comparison with SDK Usage

For comparison, here's how the same trace would be generated using the Python SDK:

```python
from openai import OpenAI
from langtrace_python_sdk import langtrace

# Initialize clients
client = OpenAI()
langtrace.init(api_key="YOUR_LANGTRACE_API_KEY")

# Make API call - traces are automatically sent
response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Hello"}],
    temperature=0.7,
    max_tokens=150
)
```

## Best Practices

1. Always include required attributes (vendor, model, messages, path)
2. Generate unique trace and span IDs for each request
3. Use accurate timestamps for start and end times
4. Include usage statistics when available
5. Format message arrays and tool calls as proper JSON strings
6. Use the OpenTelemetry user agent to ensure proper trace processing
7. Include resource attributes with service information
8. Use proper OpenTelemetry value format (stringValue/intValue) for attributes
