
# sBTC-ChainCheck - Document Verification Smart Contract

A secure and robust Clarity smart contract for **decentralized document verification** on the Stacks blockchain. This enhanced version ensures strict validation, version control, access permissions, and verifiability of digital documents via secure hash IDs.

---

## 🚀 Features

* ✅ **Immutable document records**: Uses 32-byte hash IDs to uniquely identify documents.
* 🔐 **Granular permission control**: Define viewing and verification rights per user.
* 🔄 **Document updates & versioning**: Owners can update documents while preserving audit trails.
* 👮 **Access restrictions**: Only authorized users can verify or modify records.
* 🧾 **Metadata support**: Attach and validate UTF-8 metadata (up to 256 chars).
* 📜 **Detailed error codes**: Transparent error handling for all contract operations.
* 🛡️ **Strict input validation**: Ensures security against malformed or unauthorized data.

---

## 📚 Data Structures

### Document Record (`document-records`)

Each document has:

* `document-owner`: The principal who submitted the document.
* `document-content-hash`: 32-byte hash of the content.
* `submission-timestamp`: Block height when submitted.
* `verification-status`: "PENDING" or "VERIFIED".
* `verification-authority`: Principal who verified the document.
* `document-metadata`: Descriptive metadata.
* `document-version`: Incremented on each edit.
* `verification-complete`: Boolean flag for final verification.

### Permissions Map (`document-permissions`)

Stores viewing and verification permissions:

```clarity
{ document-viewing-permission: bool, document-verification-permission: bool }
```

---

## 🔐 Access Control

| Action          | Required Permission              |
| --------------- | -------------------------------- |
| Modify document | Must be the owner                |
| Verify document | Verification permission          |
| View document   | Viewing permission (if enforced) |

---

## 📦 Contract Functions

### 🔎 Read-Only

* `get-document-details(document-hash-id)`

  * Returns the full document record if it exists.

* `get-user-permissions(document-hash-id, authorized-user)`

  * Returns the permissions of a specific user for a given document.

### ✏️ Public Functions

* `modify-existing-document(document-hash-id, updated-content-hash, updated-metadata)`

  * Allows the document owner to update an existing document.
  * Only if it's not already verified.

* `perform-document-verification(document-hash-id)`

  * Allows an authorized user to verify a document.
  * Sets the `verification-complete` flag and assigns the verifier.

---

## 🚫 Error Codes

| Error Code | Description               |
| ---------- | ------------------------- |
| `u100`     | Unauthorized access       |
| `u101`     | Duplicate document        |
| `u102`     | Document missing          |
| `u103`     | Document already verified |
| `u104`     | Invalid document hash ID  |
| `u105`     | Invalid content hash      |
| `u106`     | Invalid metadata          |
| `u107`     | Invalid authorized user   |
| `u108`     | Invalid input             |
| `u109`     | Permission denied         |

---

## ✅ Validation Utilities

* `validate-and-sanitize-hash`: Checks length of a `buff 32`.
* `validate-and-sanitize-metadata`: Ensures valid UTF-8 and non-empty.
* `validate-and-sanitize-user`: Ensures the user is not the owner (for permissions).
* `safe-get-document`: Safely retrieves a document or throws an error.

---

## 🧪 Usage Example (Clarity Console)

```clarity
;; Get details of a document
(get-document-details 0xabc123...)

;; Submit verification if authorized
(perform-document-verification 0xabc123...)

;; Update metadata as document owner
(modify-existing-document 0xabc123... 0xnewhash "Updated description")
```

---