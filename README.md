# docs-linkwarden

---

## linkwarden setup

---

linkwarden

https://github.com/linkwarden/linkwarden

https://github.com/linkwarden/browser-extension

https://addons.mozilla.org/en-US/firefox/addon/linkwarden/

---

```
    ports:
      - 3000:3000
```

change left port to desired host post

---

NEXTAUTH_URL
change to container's host and host port

otherwise error when log out
minor inconvenience

---

generate passwords

NEXTAUTH_SECRET
POSTGRES_PASSWORD

---

create user via webui

then lock down afterwards
NEXT_PUBLIC_DISABLE_REGISTRATION=true

---

meilisearch

MEILI_HOST
host and port of meilisearch
host:7700

MEILI_MASTER_KEY
make up your own master key
it should be at least 16 bytes (16 standard characters in UTF-8)

---

create key (WIP)
permissions are way too broad

```
# 1. Configuration
$meiliHost = "${HOSTNAME}:7700"
$masterKey = "${REDACTED}"    # Replace with your admin key

# 2. Define the new API key payload
$payload = @{
    description = "Read-only key: get indexes, search, get version"
    actions     = @(
        "*"
    )
    indexes     = @("*")           # Wildcard for all indexes
    expiresAt   = $null            # Never expires; use RFC3339 string to set expiration
}

# 3. Convert to JSON
$jsonBody = $payload | ConvertTo-Json

# 4. Invoke the POST /keys endpoint
$response = Invoke-RestMethod `
    -Method Post `
    -Uri "$meiliHost/keys" `
    -Headers @{ "Authorization" = "Bearer $masterKey" } `
    -ContentType "application/json" `
    -Body $jsonBody

# 5. Output the newly created key
$response

get the API key

---

look for existing thingys

```
curl -s \
  -H 'Authorization: Bearer ${API_KEY}' \
  "http://localhost:7700/indexes/links/settings/filterable-attributes" \
  | jq .
```

output:
```
[
  "collectionIsPublic",
  "collectionMemberIds",
  "collectionName",
  "collectionOwnerId",
  "creationTimestamp",
  "description",
  "name",
  "pinnedBy",
  "tags",
  "type",
  "url"
]
```

---

time to add "pinned" to the thingy

```
curl -X PUT \
  -H "Content-Type: application/json" \
  -H 'Authorization: Bearer ${API_KEY}' \
  "http://localhost:7700/indexes/links/settings/filterable-attributes" \
  --data '[
    "pinned",
    "collectionIsPublic",
    "collectionMemberIds",
    "collectionName",
    "collectionOwnerId",
    "creationTimestamp",
    "description",
    "name",
    "pinnedBy",
    "tags",
    "type",
    "url"
  ]'
```

---

it works now !

search in the webui for:
```
pinned:true
```

name:"Example Domain"

---

API equivalent:

curl -X POST \
  -H "Content-Type: application/json" \
  -H 'Authorization: Bearer ${API_KEY}' \
  -H 'X-Meili-API-Key: ${API_KEY}' \
  "http://localhost:7700/indexes/links/search" \
  --data '{
    "q": "",
    "filter": ["pinnedBy = 1"]
  }'

notice pinnedBy vs pinned

---

notes:

`title:` is not actually used by linkwarden
it uses `name:` instead

when you use advanced search, it seems like you have to be exact with your search terms
you can NOT do partial searches

---

now to get linkwarden web extension working

it requires linkwarden to generate an API token for the extension to use

easy enough to not require explanation

---

now to get browser syncing working
using floccus

it requires linkwarden to generate an API token for the extension to use

easy enough to not require explanation

---

todo:
javascript with monolith and playwright as pre-processor

---
