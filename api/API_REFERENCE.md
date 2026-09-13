# Atomic Leap Global Company Intelligence API

The Atomic Leap API provides real-time access to corporate registry data across the UK, France, US, and EU.

## Authentication

The API uses Bearer tokens for authentication. Include your API key in the `Authorization` header of your requests.

```bash
Authorization: Bearer YOUR_API_KEY
```

## Rate Limits

The API enforces rate limits based on your pricing tier. Rate limit information is included in the response headers:

- `X-RateLimit-Limit`: The maximum number of requests you're permitted to make per hour.
- `X-RateLimit-Remaining`: The number of requests remaining in the current rate limit window.

When the rate limit is exceeded, the API returns a `429 Too Many Requests` status code.

```json
{
  "error": "rate_limit_exceeded",
  "message": "You have exceeded your API request quota. Please upgrade your plan."
}
```

## Endpoints

### Retrieve Company Details

`GET /companies/{jurisdiction}/{company_number}`

Fetches detailed information about a company from the specified jurisdiction's registry.

#### Parameters

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `jurisdiction` | path | string | Yes | The country or state jurisdiction code (e.g., 'uk', 'fr', 'us_de'). |
| `company_number` | path | string | Yes | The official company registration number. |

#### Example Request

```bash
curl -X GET "https://api.atomicleap.co/v1/companies/uk/12345678" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

#### Example Response (200 OK)

```json
{
  "company_number": "12345678",
  "jurisdiction": "uk",
  "name": "ATOMIC LEAP LTD",
  "status": "active",
  "incorporation_date": "2020-01-01",
  "registered_address": {
    "address_line_1": "123 Innovation Drive",
    "city": "London",
    "postal_code": "E1 6AN",
    "country": "United Kingdom"
  }
}
```

## Error Codes

The API uses standard HTTP status codes to indicate the success or failure of an API request.

| Code | Meaning | Description |
|---|---|---|
| `200` | OK | The request was successful. |
| `400` | Bad Request | The request was unacceptable, often due to missing a required parameter. |
| `401` | Unauthorized | No valid API key provided. |
| `404` | Not Found | The requested resource doesn't exist. |
| `422` | Unprocessable Entity | The request parameters were invalid. |
| `429` | Too Many Requests | Rate limit exceeded. |
| `502` | Bad Gateway | Upstream Registry Unavailable (e.g., Companies House is down). |

## Pricing Tiers

| Tier | Price | Features |
|---|---|---|
| Free Developer | $0/mo | 100 requests/day, Community Support |
| Starter | £29/mo | 10,000 requests/mo, Standard Support |
| Pro | £79/mo | 100,000 requests/mo, Priority Support, Dedicated Account Manager |
