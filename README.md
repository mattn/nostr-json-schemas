# Nostr Protocol JSON Schemas

This directory contains JSON schemas for messages used in the Nostr protocol.

## Basic Schemas

### Event Structure
- **event.json** - Basic structure of a Nostr event (NIP-01)

### Client to Relay Messages
- **client-event.json** - EVENT message (publishing events)
- **client-req.json** - REQ message (requesting events and subscriptions)
- **client-close.json** - CLOSE message (stopping subscriptions)

### Relay to Client Messages
- **relay-event.json** - EVENT message (delivering events)
- **relay-ok.json** - OK message (acceptance/rejection of EVENT)
- **relay-eose.json** - EOSE message (end of stored events)
- **relay-closed.json** - CLOSED message (subscription ended)
- **relay-notice.json** - NOTICE message (notifications)

## Event Kind Schemas

### Kind 0
- **kind-0.json** - User metadata (profile information)

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

## References

- [NIP-01: Basic protocol flow description](../01.md)
- [Nostr Protocol](https://github.com/nostr-protocol/nostr)
