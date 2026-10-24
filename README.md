# BSL_Bcrypt

**BSL_Bcrypt** is a password hashing and key derivation library designed for the **Bonezegei Scripting Language (BSL)**. It provides secure password hashing, customizable work factors, and password verification using the adaptive bcrypt algorithm in `.bzg` scripts.

## Table of Contents
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Code Examples](#code-examples)
  - [1. Salt Generation](#1-salt-generation)
  - [2. Password Hashing & Verification](#2-password-hashing--verification)
  - [3. Complete Verification Script](#3-complete-verification-script)
- [API Reference](#api-reference)
- [License & Author](#license--author)
- [Citation](#citation)

## Installation

Install `BSL_Bcrypt` using the BSL Package Manager (`bzg`):

```bash
bzg install bcrypt
```

## Getting Started

To use the library in your script, include the bcrypt module after installation and instantiate the `bcrypt` object:

```javascript
include("lib/bcrypt.bzg");

var b = bcrypt();
var hash_str = b.hash("my_secret_password");
print("Bcrypt Hash: " + hash_str);
```

## Code Examples

### 1. Salt Generation

Example generating cryptographically secure salt strings with customizable work factors (cost):

```javascript
/*
    Bcrypt Salt Generation Example
    Author: Jofel Batutay (Bonezegei)
    Date: October 24, 2026
*/

include("lib/bcrypt.bzg");

var b = bcrypt();

// Generate salt with default work factor (10)
print("Default Salt (Cost 10): " + b.gensalt());

// Generate salts with custom work factors
print("Salt (Cost 4):  " + b.gensalt(4));
print("Salt (Cost 12): " + b.gensalt(12));
```

---

### 2. Password Hashing & Verification

Example hashing plain-text passwords and verifying provided passwords against stored hashes:

```javascript
/*
    Password Hashing & Verification Example
    Author: Jofel Batutay (Bonezegei)
    Date: October 24, 2026
*/

include("lib/bcrypt.bzg");

var b = bcrypt();
var password = "SuperSecretPassword123";

// Hash password with automatic salt or custom cost
var hashed = b.hash(password, 10);
print("Hashed Output: " + hashed);

// Verify correct password
var is_valid = b.verify(password, hashed);
print("Valid Password Test: " + is_valid); // Outputs 1.0 (True)

// Verify wrong password
var is_invalid = b.verify("WrongPassword", hashed);
print("Invalid Password Test: " + is_invalid); // Outputs 0.0 (False)
```

---

### 3. Complete Verification Script

A full test suite script demonstrating output verification across bcrypt salt generation, hashing, and password validation:

```javascript
/*
    BSL_Bcrypt Test Suite
    Author: Jofel Batutay (Bonezegei)
    Date: October 24, 2026
*/

include("lib/bcrypt.bzg");

function check(label, actual, expected) {
    if (actual === expected) {
        print("PASS: " + label + "  OUTPUT: " + actual);
    } else {
        print("FAIL: " + label + "  OUTPUT: " + actual);
    }
}

var b = bcrypt();
var password = "Bonezegei_Secure_Pass_2026";

print("--- Salt Generation & Length Check ---");
var salt = b.gensalt(10);
check("Salt Length == 29", string_len(salt), 29);

print("\n--- Hashing & Password Verification ---");
var hash_output = b.hash(password, salt);
check("Hash Length == 60", string_len(hash_output), 60);
check("Verify Valid Password", b.verify(password, hash_output), 1.0);
check("Reject Invalid Password", b.verify("IncorrectPass", hash_output), 0.0);
```

## API Reference

| Function Signature | Return Value | Description |
| :--- | :--- | :--- |
| `b.gensalt([cost])` | `string` | Generates a 29-character bcrypt salt string (`$2b$<cost>$...`) with an optional work factor cost (default: 10, range: 4–31). |
| `b.hash(password, [cost_or_salt])` | `string` | Computes a 60-character bcrypt hash string for the given password using a salt string or cost factor. |
| `b.verify(password, hash)` | `number` | Verifies a plain-text password against a bcrypt hash. Returns `1.0` if matched, or `0.0` if mismatched/invalid. |

---

## License & Author

- **Author:** Jofel Batutay ([Bonezegei](https://github.com/bonezegei))
- **Website:** [bonezegei.com](https://bonezegei.com)

