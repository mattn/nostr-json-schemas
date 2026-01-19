# Nostr Protocol JSON Schemas

This directory contains JSON schemas for messages used in the Nostr protocol.

## Schemas Overview

### Basic Schemas

| Schema | Description | NIP |
|--------|-------------|-----|
| [`event.json`](./event.json) | Basic structure of a Nostr event | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |

### Client to Relay Messages

| Schema | Description | NIP |
|--------|-------------|-----|
| [`client-event.json`](./client-event.json) | EVENT message (publishing events) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |
| [`client-req.json`](./client-req.json) | REQ message (requesting events and subscriptions) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md), [NIP-50](https://github.com/nostr-protocol/nips/blob/master/50.md) |
| [`client-close.json`](./client-close.json) | CLOSE message (stopping subscriptions) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |

### Relay to Client Messages

| Schema | Description | NIP |
|--------|-------------|-----|
| [`relay-event.json`](./relay-event.json) | EVENT message (delivering events) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |
| [`relay-ok.json`](./relay-ok.json) | OK message (acceptance/rejection of EVENT) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |
| [`relay-eose.json`](./relay-eose.json) | EOSE message (end of stored events) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |
| [`relay-closed.json`](./relay-closed.json) | CLOSED message (subscription ended) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |
| [`relay-notice.json`](./relay-notice.json) | NOTICE message (notifications) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md) |

### Event Kind Schemas

| Schema | Description | NIP |
|--------|-------------|-----|
| [`kind-0.json`](./kind-0.json) | User metadata (profile information) | [NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md), [NIP-05](https://github.com/nostr-protocol/nips/blob/master/05.md) |


## Usage

These schemas conform to JSON Schema Draft 7.

### Validation Example (Node.js)

```javascript
const Ajv = require('ajv');
const ajv = new Ajv();

const eventSchema = require('./event.json');
const validate = ajv.compile(eventSchema);

const event = {
  id: "a".repeat(64),
  pubkey: "b".repeat(64),
  created_at: 1234567890,
  kind: 1,
  tags: [],
  content: "Hello Nostr!",
  sig: "c".repeat(128)
};

const valid = validate(event);
if (!valid) {
  console.log(validate.errors);
}
```

### Validation Example (Python)

```python
import jsonschema
import json

with open('event.json') as f:
    schema = json.load(f)

event = {
    "id": "a" * 64,
    "pubkey": "b" * 64,
    "created_at": 1234567890,
    "kind": 1,
    "tags": [],
    "content": "Hello Nostr!",
    "sig": "c" * 128
}

try:
    jsonschema.validate(event, schema)
    print("Valid!")
except jsonschema.ValidationError as e:
    print(f"Invalid: {e}")
```

## Editor Integration

### Vim (with vim-lsp-settings)

Execute `:LspSettingsLocalEdit` and add the following configuration to associate schemas with specific file patterns. This enables validation and completion for Nostr-related JSON files.

```json
{
    "json-languageserver": {
        "disabled": false,
        "workspace_config": {
            "json": {
                "format": true,
                "schemas": [
                    {
                        "name": "Nostr Event JSON",
                        "description": "Nostr Event JSON",
                        "fileMatch": ["**/nostr-event.json"],
                        "url": "https://mattn.github.io/nostr-json-schemas/event.json"
                    },
                    {
                        "name": "Nostr Kind 0 JSON",
                        "description": "Nostr Kind 0 JSON",
                        "fileMatch": ["**/kind0.json"],
                        "url": "https://mattn.github.io/nostr-json-schemas/kind-0.json"
                    }
                    // Add more schemas as needed, e.g.:
                    // {
                    //     "name": "Nostr Client REQ",
                    //     "fileMatch": ["**/client-req.json"],
                    //     "url": "https://mattn.github.io/nostr-json-schemas/client-req.json"
                    // }
                ]
            }
        }
    }
}
```

### Visual Studio Code

VS Code has built-in support for JSON Schema validation and IntelliSense.

Add the following to your User or Workspace `settings.json` (accessible via *File > Preferences > Settings* or `Ctrl+,`, then search for "json schemas"):

```json
{
    "json.schemas": [
        {
            "fileMatch": ["**/nostr-event.json"],
            "url": "https://mattn.github.io/nostr-json-schemas/event.json"
        },
        {
            "fileMatch": ["**/kind0.json"],
            "url": "https://mattn.github.io/nostr-json-schemas/kind-0.json"
        }
        // Add more as needed
    ]
}
```

Alternatively, in individual JSON files, add a `$schema` property at the top level:

```json
{
    "$schema": "https://mattn.github.io/nostr-json-schemas/event.json",
    // ... your event data
}
```

## References

- [NIP-01: Basic protocol flow description](../01.md)
- [Nostr Protocol](https://github.com/nostr-protocol/nostr)
