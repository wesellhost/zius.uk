# Zius API Documentation

![Zius for n8n](https://raw.githubusercontent.com/wesellhost/n8n-nodes-starter/refs/heads/master/img/58b29e35-13ac-4a65-84de-562dbb250a67.png)

[![API Documentation](https://img.shields.io/badge/API-Documentation-blue)](https://zius.uk/dashboard/docs)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE.md)
[![Dashboard](https://img.shields.io/badge/Dashboard-Login-purple)](https://zius.uk/dashboard)

> **Seamless WhatsApp and SMS Gateway Integration** - Automate your marketing, notifications, and bulk messaging with powerful APIs and automation tools.

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Getting Started](#-getting-started)
- [Authentication](#-authentication)
- [API Endpoints](#-api-endpoints)
  - [Contact Management](#contact-management-apis)
  - [WhatsApp Messaging](#whatsapp-messaging-apis)
  - [Android Devices](#android-device-apis)
- [Code Examples](#-code-examples)
- [Best Practices](#-best-practices)
- [Resources](#-resources)
- [n8n Integration](#n8n-integration)

---

## 🚀 Overview

Zius is a comprehensive WhatsApp and SMS gateway platform designed for businesses to automate their communication workflows. Our platform guarantees business success with strategic features crafted to meet the demands of ever-growing marketing businesses.

**Base API URL:** `https://zius.uk/api`

![Zius Quick Sending](https://zius.uk/templates/default/assets/images/hero-1.png)

---

## ✨ Key Features

### 📤 Quick and Bulk Sending
- Send tons of messages to different contact groups easily
- Quick messages for fast transactions
- Live dashboard notifications for sent/received messages
- **Shortcodes:** Compose messages with predefined data
- **Spintax:** Randomize words to battle spamming issues
- **Translator:** Target audiences based on their native languages

### 💬 Core Messaging Gateways
- **WhatsApp Messaging:** Full-featured WhatsApp API with built-in platform tools
- **SMS Messaging:** Use Android devices as SMS gateway (cheaper than third-party gateways)
- **Partner Program:** Earn money by using your Android devices as a gateway

### 🤖 Automated and Smart Tools

![Zius Automation](https://zius.uk/templates/default/assets/images/hero-2.png)

- **Schedule:** Time your SMS and WhatsApp chats for any date
- **Webhooks:** Automate tasks when receiving messages from both SMS and WhatsApp
- **Actions:** Create auto-responses and quick data checks for specific events
- **USSD:** Perform network-related requests (e.g., checking balance from your operator)
- **Notifications:** Use connected Android devices for automated tasks

---

## 🚀 Getting Started

### 1. Sign Up & Get Credentials
1. Visit [Zius Dashboard](https://zius.uk/dashboard) and create an account
2. Navigate to **Tools → API Keys** to generate your API secret
3. Copy and securely store your API secret key

### 2. Connect Your Gateway
Choose one or both messaging gateways:
- **WhatsApp:** [Add WhatsApp Device](https://zius.uk/dashboard/hosts/whatsapp)
- **Android SMS:** [Add Android Device](https://zius.uk/dashboard/hosts/android)

### 3. Start Sending Messages
Use the API endpoints below to integrate Zius into your application.

---

## 🔐 Authentication

All API requests require authentication using your API secret key.

```bash
# Include your API secret in every request
secret=YOUR_API_SECRET
```

**Get your API secret:** [Dashboard → Tools → API Keys](https://zius.uk/dashboard/tools/keys)

⚠️ **Security Note:** Never expose your API secret in client-side code. Always make API calls from your backend server.

---

## 📡 API Endpoints

### Contact Management APIs

#### Create Contact
Create a new contact in your account.

**Endpoint:** `POST /api/create/contact`  
**Permission:** `create_contact`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `phone` | string | ✅ | E.164 (+201001234567) or Local (01001234567) |
| `name` | string | ✅ | Contact name |
| `groups` | string | ✅ | Comma-separated group IDs |

**Example Request:**
```bash
curl -X POST "https://zius.uk/api/create/contact" \
     -H "Content-Type: multipart/form-data" \
     -F "secret=YOUR_API_SECRET" \
     -F "phone=+201001234567" \
     -F "name=John Doe" \
     -F "groups=1,2,3"
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "Contact has been created!",
  "data": false
}
```

---

#### Get Contacts
Retrieve all contacts with pagination.

**Endpoint:** `GET /api/get/contacts`  
**Permission:** `get_contacts`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `limit` | number | ❌ | Results per page (default: 10) |
| `page` | number | ❌ | Page number (default: 1) |

**Example Request:**
```bash
curl -X GET "https://zius.uk/api/get/contacts?secret=YOUR_API_SECRET&limit=10&page=1" \
     -H "Content-Type: application/json"
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "Saved Contacts",
  "data": [
    {
      "id": 2,
      "groups": ["1"],
      "phone": "+201001234567",
      "name": "Shane"
    },
    {
      "id": 3,
      "groups": ["1", "9", "10", "11"],
      "phone": "+201001234567",
      "name": "Terry Bom"
    }
  ]
}
```

---

#### Delete Contact
Delete a specific contact by ID.

**Endpoint:** `GET /api/delete/contact`  
**Permission:** `delete_contact`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `id` | number | ✅ | Contact ID to delete |

**Example Request:**
```bash
curl -X GET "https://zius.uk/api/delete/contact?secret=YOUR_API_SECRET&id=123" \
     -H "Content-Type: application/json"
```

---

### WhatsApp Messaging APIs

#### Send Single WhatsApp Message
Send a single WhatsApp message to a recipient.

**Endpoint:** `POST /api/send/whatsapp`  
**Permission:** `wa_send`

**Required Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `secret` | string | Your API secret key |
| `account` | string | WhatsApp account unique ID |
| `recipient` | string | Phone number (E.164/local) or group address |
| `message` | string | Message text or caption (supports Spintax) |

**Optional Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `type` | string | Message type: "text", "media", "document" (default: "text") |
| `priority` | number | 1 for immediate, 2 for normal (default: 2) |
| `media_file` | binary | Media file (jpg, png, gif, mp4, mp3, ogg) |
| `media_url` | string | Direct URL to media file |
| `media_type` | string | "image", "audio", or "video" (required with media_url) |
| `document_file` | binary | Document file (pdf, xml, xls, xlsx, doc, docx) |
| `document_url` | string | Direct URL to document file |
| `document_name` | string | File name with extension |
| `document_type` | string | File type (required with document_url) |
| `shortener` | number | Shortener ID for link shortening |

**Example Request (Text Message):**
```bash
curl -X POST "https://zius.uk/api/send/whatsapp" \
     -H "Content-Type: multipart/form-data" \
     -F "secret=YOUR_API_SECRET" \
     -F "account=WHATSAPP_ACCOUNT_ID" \
     -F "recipient=+201001234567" \
     -F "type=text" \
     -F "message=Hello! This is a test message."
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "WhatsApp chat has been sent!",
  "data": {
    "messageId": 123
  }
}
```

---

#### Send Bulk WhatsApp Messages
Send WhatsApp messages to multiple recipients in a campaign.

**Endpoint:** `POST /api/send/whatsapp.bulk`  
**Permission:** `wa_send`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `account` | string | ✅ | WhatsApp account unique ID |
| `campaign` | string | ✅ | Campaign name for tracking |
| `type` | string | ❌ | Message type (default: "text") |
| `message` | string | ✅ | Message text (supports Spintax) |

**Example Request:**
```bash
curl -X POST "https://zius.uk/api/send/whatsapp.bulk" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "secret=YOUR_API_SECRET" \
     -d "account=WHATSAPP_ACCOUNT_ID" \
     -d "campaign=Spring Sale 2024" \
     -d "type=text" \
     -d "message=Special offer just for you!"
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "5 chats has been queued for sending!",
  "data": {
    "campaignId": 123,
    "messageIds": [456, 457, 458, 459, 460]
  }
}
```

---

#### Validate WhatsApp Phone Number
Check if a phone number exists on WhatsApp.

**Endpoint:** `GET /api/validate/whatsapp`  
**Permission:** `validate_wa_phone`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `unique` | string | ✅ | WhatsApp account unique ID |
| `phone` | string | ✅ | E.164 formatted phone number |

**Example Request:**
```bash
curl -X GET "https://zius.uk/api/validate/whatsapp?secret=YOUR_API_SECRET&unique=WA_ID&phone=+201001234567" \
     -H "Content-Type: application/json"
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "WhatsApp phone number is valid!",
  "data": {
    "jid": "+201001234567@s.whatsapp.net",
    "phone": "+201001234567"
  }
}
```

---

#### Start WhatsApp Campaign
Start or resume a WhatsApp campaign.

**Endpoint:** `GET /api/remote/start.chats`  
**Permission:** `start_wa_campaign`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `campaign` | number | ✅ | Campaign ID to start |

**Example Request:**
```bash
curl -X GET "https://zius.uk/api/remote/start.chats?secret=YOUR_API_SECRET&campaign=123" \
     -H "Content-Type: application/json"
```

---

#### Stop WhatsApp Campaign
Stop or pause a WhatsApp campaign.

**Endpoint:** `GET /api/remote/stop.chats`  
**Permission:** `stop_wa_campaign`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `campaign` | number | ✅ | Campaign ID to stop |

**Example Request:**
```bash
curl -X GET "https://zius.uk/api/remote/stop.chats?secret=YOUR_API_SECRET&campaign=123" \
     -H "Content-Type: application/json"
```

---

### Android Device APIs

#### Get Android Devices
Retrieve all connected Android devices.

**Endpoint:** `GET /api/get/devices`  
**Permission:** `get_devices`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `limit` | number | ❌ | Results per page (default: 10) |
| `page` | number | ❌ | Page number (default: 1) |

**Example Request:**
```bash
curl -X GET "https://zius.uk/api/get/devices?secret=YOUR_API_SECRET&limit=10&page=1" \
     -H "Content-Type: application/json"
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "Android Devices",
  "data": [
    {
      "id": "49",
      "unique": "00000000-0000-0000-d57d-f30cb6a89289",
      "name": "F11 Phone",
      "version": "Android 11",
      "manufacturer": "OPPO",
      "limit_status": true,
      "limit_interval": "daily",
      "limit_number": 100,
      "created": 1636462504
    }
  ]
}
```

---

#### Send USSD Request
Send USSD codes to check balance, activate services, etc.

**Endpoint:** `POST /api/send/ussd`  
**Permission:** `ussd`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `code` | string | ✅ | MMI request code (e.g., *123#) |
| `sim` | number | ✅ | SIM slot number (1 or 2) |
| `device` | string | ✅ | Device unique ID from /get/devices |

**Example Request:**
```bash
curl -X POST "https://zius.uk/api/send/ussd" \
     -H "Content-Type: multipart/form-data" \
     -F "secret=YOUR_API_SECRET" \
     -F "code=*123#" \
     -F "sim=1" \
     -F "device=00000000-0000-0000-d57d-f30cb6a89289"
```

---

#### Get USSD Requests
Retrieve all USSD requests and their responses.

**Endpoint:** `GET /api/get/ussd`  
**Permission:** `get_ussd`

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `secret` | string | ✅ | Your API secret key |
| `limit` | number | ❌ | Results per page (default: 10) |
| `page` | number | ❌ | Page number (default: 1) |

**Example Request:**
```bash
curl -X GET "https://zius.uk/api/get/ussd?secret=YOUR_API_SECRET&limit=10&page=1" \
     -H "Content-Type: application/json"
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "USSD Requests",
  "data": [
    {
      "id": 5,
      "device": "00000000-0000-0000-d57d-f30cb6a89289",
      "sim": 1,
      "code": "*143#",
      "response": "Your balance is 14.60",
      "status": "completed",
      "created": 1645043019
    }
  ]
}
```

---

## 💻 Code Examples

### JavaScript (Node.js)

```javascript
const FormData = require('form-data');
const axios = require('axios');

// Send WhatsApp Message
const sendWhatsApp = async () => {
  const formData = new FormData();
  formData.append('secret', 'YOUR_API_SECRET');
  formData.append('account', 'YOUR_WHATSAPP_ACCOUNT_ID');
  formData.append('recipient', '+201001234567');
  formData.append('type', 'text');
  formData.append('message', 'Hello from Zius API!');

  try {
    const response = await axios.post('https://zius.uk/api/send/whatsapp', formData, {
      headers: formData.getHeaders()
    });
    console.log('Success:', response.data);
  } catch (error) {
    console.error('Error:', error.response.data);
  }
};

sendWhatsApp();
```

### Python

```python
import requests

# Send WhatsApp Message
url = 'https://zius.uk/api/send/whatsapp'
data = {
    'secret': 'YOUR_API_SECRET',
    'account': 'YOUR_WHATSAPP_ACCOUNT_ID',
    'recipient': '+201001234567',
    'type': 'text',
    'message': 'Hello from Zius API!'
}

response = requests.post(url, data=data)
print(response.json())
```

### PHP

```php
<?php
// Send WhatsApp Message
$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://zius.uk/api/send/whatsapp",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_POST => true,
  CURLOPT_POSTFIELDS => [
    'secret' => 'YOUR_API_SECRET',
    'account' => 'YOUR_WHATSAPP_ACCOUNT_ID',
    'recipient' => '+201001234567',
    'type' => 'text',
    'message' => 'Hello from Zius API!'
  ]
]);

$response = curl_exec($curl);
curl_close($curl);

echo $response;
?>
```

### cURL

```bash
# Send WhatsApp Message
curl -X POST "https://zius.uk/api/send/whatsapp" \
     -H "Content-Type: multipart/form-data" \
     -F "secret=YOUR_API_SECRET" \
     -F "account=YOUR_WHATSAPP_ACCOUNT_ID" \
     -F "recipient=+201001234567" \
     -F "type=text" \
     -F "message=Hello from Zius API!"
```

---

## 📋 Best Practices

### Security
- ✅ Keep your API secret secure and never expose it in client-side code
- ✅ Regenerate API keys periodically
- ✅ Use HTTPS for all API requests
- ✅ Implement proper error handling and logging

### Rate Limiting
- ✅ Respect API rate limits to avoid throttling
- ✅ Implement exponential backoff for failed requests
- ✅ Monitor your usage through the dashboard

### Phone Number Formatting
- ✅ Use E.164 format for international compatibility (+201001234567)
- ✅ Verify phone numbers before sending bulk messages
- ✅ Use the validation endpoint for WhatsApp numbers

### Message Optimization
- ✅ Use **Spintax** to randomize messages and avoid spam filters
- ✅ Use **Shortcodes** for personalized messages
- ✅ Schedule messages during optimal engagement times
- ✅ Monitor delivery status through the dashboard

### Bulk Messaging
- ✅ Group contacts logically for targeted campaigns
- ✅ Test messages with small groups before full campaigns
- ✅ Monitor campaign performance in real-time
- ✅ Use webhooks for automated follow-ups

---

## 🔗 Resources

### Dashboard & Tools
- **Main Dashboard:** [zius.uk/dashboard](https://zius.uk/dashboard/)
- **API Documentation:** [zius.uk/dashboard/docs](https://zius.uk/dashboard/docs)
- **API Keys:** [zius.uk/dashboard/tools/keys](https://zius.uk/dashboard/tools/keys)
- **Webhooks:** [zius.uk/dashboard/tools/webhooks](https://zius.uk/dashboard/tools/webhooks)
- **Flows:** [zius.uk/dashboard/tools/flows](https://zius.uk/dashboard/tools/flows)

### Gateway Setup
- **Add WhatsApp Device:** [zius.uk/dashboard/hosts/whatsapp](https://zius.uk/dashboard/hosts/whatsapp)
- **Add Android Device:** [zius.uk/dashboard/hosts/android](https://zius.uk/dashboard/hosts/android)

### Images & Assets
![Zius Dashboard Elements](https://zius.uk/templates/default/assets/images/mini-4.png)

---

## n8n Integration

This is the official [n8n](https://n8n.io/) community node for **Zius**. Seamlessly integrate your Zius WhatsApp and SMS gateway directly into your n8n workflows.

### Installation

To install this node in your self-hosted n8n instance:

1. Go to **Settings > Community Nodes**
2. Click **Install a community node**
3. Enter `n8n-nodes-zius-whatsapp` and click **Install**

### Credentials Setup

To use this node:
1. Grab your **API Secret** and **WhatsApp Account Unique ID** from your [Zius Dashboard](https://zius.uk/dashboard/tools/keys)
2. In n8n, add the Zius node to your canvas
3. Click **Create New Credential**
4. Securely paste your API details

---

## 📝 Response Codes

### Success Responses
- **200:** Request successful
- Response format: `{ "status": 200, "message": "Success message", "data": {...} }`

### Error Responses
Common error scenarios:
- Invalid API secret
- Missing required parameters
- Insufficient permissions
- Resource not found
- Rate limiting

Error format:
```json
{
  "status": 400,
  "message": "Error description",
  "data": false
}
```

---

## 🤝 Support

For support and questions:
- Visit the [Zius Dashboard](https://zius.uk/dashboard)
- Check the [API Documentation](https://zius.uk/dashboard/docs)
- Contact through the website

---

## 📄 License

[MIT](LICENSE.md)

---

<div align="center">
  <p>Made with ❤️ by the Zius Team</p>
  <p>
    <a href="https://zius.uk">Website</a> •
    <a href="https://zius.uk/dashboard">Dashboard</a> •
    <a href="https://zius.uk/dashboard/docs">API Docs</a>
  </p>
</div>
