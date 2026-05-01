# savanto/ai-sdk

Official PHP SDK for [Savanto](https://savanto.ai) — AI-powered search, chat, recommendations, and knowledge base management.

Auto-generated from the [Savanto OpenAPI spec](https://savanto.ai/docs/api) using [OpenAPI Generator](https://openapi-generator.tech).

## Requirements

- PHP >= 8.1
- Guzzle >= 7.3
- Extensions: `curl`, `json`, `mbstring`

## Installation

```bash
composer require savanto/ai-sdk
```

## Quick Start

```php
use Savanto\AI\Api\ProductsApi;
use Savanto\AI\Api\ChatApi;
use Savanto\AI\Configuration;
use Savanto\AI\Model\ProductSearchRequest;
use GuzzleHttp\Client;

$config = Configuration::getDefaultConfiguration()
    ->setApiKey('Authorization', 'Bearer if_sk_your_secret_key');

$client = new Client();

// Search products
$api = new ProductsApi($client, $config);
$request = new ProductSearchRequest(['query' => 'warm jacket', 'limit' => 10]);
$result = $api->searchProducts($request);

// Chat
$chatApi = new ChatApi($client, $config);
$chatRequest = new \Savanto\AI\Model\ChatRequest([
    'message' => 'What shoes do you recommend?',
    'thread_id' => 'thread-1',
]);
$response = $chatApi->chat($chatRequest);
```

## Authentication

Pass your API key via the Configuration object:

```php
$config = Configuration::getDefaultConfiguration()
    ->setApiKey('Authorization', 'Bearer if_sk_...');
```

| Key Type | Prefix | Use |
|----------|--------|-----|
| **Secret** | `if_sk_` | Server-side only. Full access to all endpoints. |
| **Publishable** | `if_pk_` | Safe for client-side. Limited to search, chat, feedback, and prompts. |

## Available API Classes

| Class | Description |
|-------|-------------|
| `ProductsApi` | CRUD, search, bulk operations for products |
| `PostsApi` | CRUD, search, bulk operations for posts |
| `ChatApi` | Send chat messages (supports NDJSON streaming) |
| `PromptsApi` | Manage conversation prompts |
| `TaxonomiesApi` | Manage taxonomy terms |
| `FeedbackApi` | Submit and query user feedback |
| `ThreadsApi` | Search, export, and manage threads |
| `WebhooksApi` | Create, list, test, and manage webhooks |
| `AnalyticsApi` | Search, chat, and feedback analytics |
| `RecommendationsApi` | AI-powered product recommendations |
| `CrawlApi` | Website crawling and configuration |
| `ScrapeApi` | Single-page scraping |
| `APIKeysApi` | Create, list, rotate, and revoke API keys |
| `ProvisioningApi` | Tenant provisioning and billing |

## Examples

### Products

```php
use Savanto\AI\Api\ProductsApi;
use Savanto\AI\Model\ProductInput;
use Savanto\AI\Model\ProductSearchRequest;

$api = new ProductsApi($client, $config);

// Upsert
$product = new ProductInput([
    'id' => 'prod-1',
    'name' => 'Running Shoes',
    'price' => 99.99,
    'categories' => ['footwear'],
]);
$api->upsertProduct($product);

// Search
$request = new ProductSearchRequest(['query' => 'blue shoes', 'limit' => 10]);
$results = $api->searchProducts($request);

// Get by ID
$product = $api->getProduct('prod-1');

// Delete
$api->deleteProduct('prod-1');
```

### Feedback

```php
use Savanto\AI\Api\FeedbackApi;
use Savanto\AI\Model\FeedbackSubmission;

$api = new FeedbackApi($client, $config);

$feedback = new FeedbackSubmission([
    'thread_id' => 'thread-1',
    'message_index' => 0,
    'rating' => 5,
    'comment' => 'Very helpful!',
]);
$api->submitFeedback($feedback);
```

### Webhooks

```php
use Savanto\AI\Api\WebhooksApi;
use Savanto\AI\Model\CreateWebhookInput;

$api = new WebhooksApi($client, $config);

$webhook = new CreateWebhookInput([
    'name' => 'Inventory sync',
    'url' => 'https://example.com/hook',
    'events' => ['product.upsert'],
]);
$api->createWebhook($webhook);

// List
$webhooks = $api->listWebhooks();

// Test
$result = $api->testWebhook('webhook-1');
```

## Error Handling

```php
use Savanto\AI\ApiException;

try {
    $product = $api->getProduct('nonexistent');
} catch (ApiException $e) {
    echo 'Status: ' . $e->getCode() . "\n";
    echo 'Message: ' . $e->getMessage() . "\n";
    echo 'Body: ' . $e->getResponseBody() . "\n";
}
```

## API Reference

Full endpoint documentation with request/response schemas:

**[savanto.ai/docs/api](https://savanto.ai/docs/api)**

## Regeneration

The SDK is auto-generated from the OpenAPI spec. To regenerate after API changes:

```bash
cd cloud && npm run openapi            # extract spec to cloud/openapi.json
cd sdks/php && composer generate       # regenerate src/ via Docker
```

## License

MIT
