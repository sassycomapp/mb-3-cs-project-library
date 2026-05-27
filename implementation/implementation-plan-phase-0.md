# Mybizz CS — Phase 0 Implementation Guide

**Status:** Ready for Implementation  
**Date:** 2026-05-10  
**Target App:** mb-3-cs (Anvil)
**Related ADRs:** ADR-005 (Timezone), ADR-012 (System Currency)

---

## Overview

Phase 0 addresses critical pre-implementation requirements before Phase 1 (Foundation) can begin.

**Completion Order:**
1. TODO 1: Vault Error Handling (P1, critical) — **START HERE**
2. TODO 2: Test Coverage (P1, pre-implementation)
3. TODO 3: Observability (P2, pre-implementation)
4. TODO 4: Deployment Procedures (P2, pre-implementation)

---

## TODO 1: Vault Error Handling (P1, Critical)

**Location:** `server_code/server_shared/vault_service.py`

### Implementation

**File:** `server_code/server_shared/exceptions.py`

```python
# Mybizz CS — Custom Exception Classes
# Phase 0 Implementation — TODO 1

class VaultError(Exception):
    """Base exception for Vault-related errors."""
    pass

class VaultSecretNotFoundError(VaultError):
    """Raised when a vault secret is not configured."""
    pass

class VaultDecryptionError(VaultError):
    """Raised when vault secret decryption fails."""
    pass
```

**File:** `server_code/server_shared/vault_service.py`

```python
# Mybizz CS — Vault Service
# Phase 0 Implementation — TODO 1

import anvil
from .exceptions import (
    VaultError,
    VaultSecretNotFoundError,
    VaultDecryptionError,
)
from .encryption_service import decrypt

def get_vault_secret(secret_name):
    """
    Retrieve a secret from the Vault table.
    
    Args:
        secret_name: The name of the secret (e.g., 'stripe_secret_key')
    
    Returns:
        str: The decrypted secret value
    
    Raises:
        VaultSecretNotFoundError: If the secret is not configured
        VaultDecryptionError: If decryption fails
    """
    try:
        # Query the vault table
        secret_row = anvil.app_tables.vault.find_one(name=secret_name)
        
        if not secret_row:
            # User-friendly error for missing secrets
            raise VaultSecretNotFoundError(
                f"Vault secret '{secret_name}' not configured. "
                "Please contact the business owner."
            )
        
        encrypted_value = secret_row.value
        
        # Decrypt the secret
        decrypted_value = decrypt(encrypted_value)
        return decrypted_value
        
    except VaultSecretNotFoundError:
        # Re-raise our custom errors
        raise
    except VaultDecryptionError:
        # Re-raise our custom errors
        raise
    except Exception as e:
        # Catch-all for any other errors
        import logging
        logging.error(
            "Vault secret retrieval failed",
            secret_name=secret_name,
            error_type=type(e).__name__,
            error_message=str(e)
        )
        raise VaultSecretNotFoundError(
            f"Failed to retrieve vault secret. Contact support."
        ) from e

def save_vault_secret(secret_name, secret_value, updated_by_user_id=None):
    """
    Save or update a secret in the Vault table.
    
    Args:
        secret_name: The name of the secret (unique identifier)
        secret_value: The plaintext secret value
        updated_by_user_id: User ID of the person updating the secret
    
    Returns:
        dict: The created/updated vault row
    """
    try:
        import base64
        import hashlib
        from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
        from cryptography.hazmat.backends import default_backend
        import os
        
        # Get encryption key
        key = anvil.server.secret('encryption_key')
        key_bytes = hashlib.sha256(key.encode()).digest()
        
        # Generate random IV
        iv = os.urandom(16)
        
        # Pad plaintext to 16-byte boundary
        pad_len = 16 - (len(secret_value) % 16)
        padded = secret_value + (chr(pad_len) * pad_len)
        
        # Encrypt
        cipher = Cipher(algorithms.AES(key_bytes), modes.CBC(iv), backend=default_backend())
        encryptor = cipher.encryptor()
        ciphertext = encryptor.update(padded.encode()) + encryptor.finalize()
        
        # Encode IV + ciphertext as base64
        encrypted = base64.b64encode(iv + ciphertext).decode()
        
        # Save to vault table
        existing_row = anvil.app_tables.vault.find_one(name=secret_name)
        
        if existing_row:
            # Update existing row
            row = anvil.app_tables.vault.update_one(
                existing_row,
                value=encrypted,
                updated_by=anvil.users.get_user_by_id(updated_by_user_id) if updated_by_user_id else None
            )
        else:
            # Create new row
            row = anvil.app_tables.vault.add_row(
                name=secret_name,
                value=encrypted,
                updated_by=anvil.users.get_user_by_id(updated_by_user_id) if updated_by_user_id else None
            )
        
        return row
        
    except Exception as e:
        import logging
        logging.error(
            "Failed to save vault secret",
            secret_name=secret_name,
            error_type=type(e).__name__,
            error_message=str(e)
        )
        raise VaultDecryptionError(f"Failed to save vault secret: {str(e)}") from e
```

### Completion Criteria

- ✅ `get_vault_secret()` has rescue block for `KeyNotFoundError`
- ✅ User sees "Vault secret not configured. Please contact the business owner." — not a 500 error
- ✅ Unit test for missing secret added and passing
- ✅ All Vault calls wrapped in try/except with user-friendly errors

---

## TODO 2: Test Coverage (P1, Pre-Implementation)

**Location:** `tests/` directory, `pytest.ini`

### Implementation

**File:** `pytest.ini`

```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts = 
    --verbose
    --strict-markers
markers =
    slow: marks tests as slow (deselect with '-m "not slow"')
    integration: marks tests as integration tests
    e2e: marks tests as end-to-end tests
```

**File:** `tests/conftest.py`

```python
import pytest
from unittest.mock import Mock, patch, MagicMock

@pytest.fixture
def mock_vault_table():
    """Mock vault table for unit tests."""
    with patch('anvil.app_tables.vault') as mock_table:
        yield mock_table

@pytest.fixture
def mock_encryption_key():
    """Mock encryption key retrieval."""
    with patch('anvil.server.secret') as mock_key:
        mock_key.return_value = 'test_encryption_key_32_chars_long!!!!'
        yield mock_key
```

### Completion Criteria

- ✅ Test framework configured (pytest)
- ✅ Pure logic tests passing (100% coverage)
- ✅ Integration tests passing (all server functions with mock data)
- ✅ E2E tests passing (critical flows: auth, booking, payment, email)
- ✅ Test data cleanup procedures defined
- ✅ Mock external services configured (Brevo, payment gateways)

---

## TODO 3: Observability (P2, Pre-Implementation)

**Location:** `server_code/server_shared/logging_config.py`, `server_code/server_shared/metrics.py`

### Implementation

**File:** `server_code/server_shared/logging_config.py`

```python
# Mybizz CS — Logging Configuration
# Phase 0 Implementation — TODO 3

import structlog
import logging

def setup_logging():
    """Configure structured logging for all server functions."""
    
    structlog.configure(
        processors=[
            structlog.stdlib.filter_by_level,
            structlog.stdlib.add_logger_name,
            structlog.stdlib.add_log_level,
            structlog.stdlib.PositionalArgumentsFormatter(),
            structlog.processors.TimeStamper(fmt="iso"),
            structlog.processors.StackInfoRenderer(),
            structlog.processors.format_exc_info,
            structlog.processors.UnicodeDecoder(),
            structlog.processors.JSONRenderer()
        ],
        context_class=dict,
        logger_factory=structlog.stdlib.LoggerFactory(),
        wrapper_class=structlog.stdlib.BoundLogger,
        cache_logger_on_first_use=True,
    )

def get_logger():
    """Get structured logger."""
    return structlog.get_logger()

# Usage in server functions:
# logger = get_logger()
# logger.info("function_entry", function="get_vault_secret", secret_name=name)
# logger.info("function_exit", function="get_vault_secret", result=secret_value)
# logger.error("function_error", function="get_vault_secret", error_type=type(e).__name__, error_message=str(e))
```

### Completion Criteria

- ✅ Observability requirements documented
- ✅ Logging framework configured (structlog)
- ✅ Metrics defined (bookings_created, payments_processed, errors, latency)
- ✅ Alerts defined (error rates, API failures, rate limits)
- ✅ Log retention documented (2 years for audit)

---

## TODO 4: Deployment Procedures (P2, Pre-Implementation)

**Location:** `docs/deployment-procedures.md`

### Implementation

See `docs/deployment-procedures.md` for complete deployment procedures including:
- Migration safety (expand-and-contract pattern)
- Feature flags (10%, 50%, 100% rollout)
- Post-deploy verification checklist
- Rollback procedures (Git, database, feature flags)

### Completion Criteria

- ✅ Deployment procedures documented
- ✅ Migration rollback procedures tested
- ✅ Feature flag strategy defined (10%, 50%, 100% rollout)
- ✅ Rollback procedure tested and verified
- ✅ Post-deploy verification checklist created

---

## Multi-Currency Implementation Notes

### ADR-012: System Currency Setting

**Reference:** `adr/012-system-currency-setting.md`

**Key Points:**
- `business_profile.system_currency` — set at onboarding, immutable after first transaction
- `services.price` — price in system_currency (default)
- `services.local_currency_price` — optional override for specific location
- `services.local_currency` — ISO 4217 (e.g., "USD") — only if local_currency_price set

**TODO:** Price conversion needs to be resolved
- Option A: Fixed exchange rates (manual update) — Recommended for V1
- Option B: API-based rates (real-time) — V2 enhancement
- Option C: No conversion (system currency only) — Simplest

### ADR-005: Timezone/UTC Storage

**Reference:** `adr/05-timezone-utc-storage-display-conversion.md`

**Key Points:**
- All datetimes stored in UTC
- `business_profile.timezone` — IANA string (e.g., "Africa/Johannesburg")
- Conversion to local timezone occurs at display time in server functions

---

## Implementation Order

**Phase 0 must complete before Phase 1 begins.**

1. **TODO 1** — Vault Error Handling (1-2 hours with CC)
2. **TODO 2** — Test Coverage (4-6 hours with CC)
3. **TODO 3** — Observability (3-4 hours with CC)
4. **TODO 4** — Deployment Procedures (2-3 hours with CC)

**Total Estimated Time:** 10-15 hours with CC (2-3 days human team)

---

## Success Criteria

Phase 0 is complete when:
- ✅ All 15 TODOs have implementation details documented
- ✅ Vault error handling prevents 500 errors
- ✅ Test framework configured and tested
- ✅ Observability logging/metrics/alerts working
- ✅ Deployment procedures documented and tested
- ✅ No blocking issues for Phase 1

---

## Next Steps

After Phase 0 completion:
1. Begin Phase 1 (Foundation) — Auth, RBAC, Vault, Navigation, Settings
2. Implement TODO 5-15 during development
3. Run `/plan-eng-review` before Phase 1 implementation
4. Run `/autoplan` for full review pipeline

---

*End of file — phase-0-implementation.md*
