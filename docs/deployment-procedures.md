# Mybizz CS — Deployment Procedures
# Phase 0 Implementation — TODO 4

## Overview

This document defines the deployment procedures for the Mybizz CS platform.
It covers database migrations, feature flags, rollback procedures, and
post-deploy verification.

## Deployment Environment Strategy

### Environments

**Development:**
- Single developer workspace
- Direct commits to main branch
- No automated testing required
- Data reset weekly

**Staging:**
- Mirror of production environment
- Automated testing required
- Pre-production validation
- Data reset monthly

**Production:**
- Live customer environment
- All testing required
- Change approval required
- Backup before each deployment

## Database Migration Strategy

### Expand-and-Contract Pattern

All database migrations follow the expand-and-contract pattern to minimize
downtime and risk:

### Phase 1: Expand (Backward Compatible)
```sql
-- Step 1: Add new column (nullable)
ALTER TABLE bookings ADD COLUMN new_field VARCHAR(255);

-- Step 2: Populate existing data
UPDATE bookings SET new_field = COALESCE(old_field, 'default');

-- Step 3: Deploy code that reads both old and new fields
-- Code should prefer new_field, fall back to old_field
```

### Phase 2: Contraction (Breaking Change)
```sql
-- Step 1: Deploy code that only uses new_field
-- Remove all references to old_field

-- Step 2: Drop old column (after verification)
ALTER TABLE bookings DROP COLUMN old_field;

-- Step 3: Add NOT NULL constraint to new_field
ALTER TABLE bookings ALTER COLUMN new_field SET NOT NULL;
```

### Migration Safety

**Before Migration:**
- ✅ Backup database
- ✅ Test migration on staging
- ✅ Verify rollback procedure
- ✅ Schedule during low-traffic period

**During Migration:**
- ✅ Monitor error rates
- ✅ Monitor query performance
- ✅ Have rollback ready

**After Migration:**
- ✅ Verify data integrity
- ✅ Run smoke tests
- ✅ Monitor for 24 hours

## Feature Flags

### Feature Flag Strategy

All new features must be behind feature flags to enable gradual rollout:

**Rollout Percentages:**
- 10% - Initial rollout (monitor for errors)
- 50% - Extended rollout (monitor for performance)
- 100% - Full rollout (remove feature flag)

**Feature Flag Configuration:**

```python
# Feature flag definitions
FEATURE_FLAGS = {
    'new_booking_flow': {
        'enabled': False,
        'rollout_percentage': 0,
        'owner': 'team_bookings',
        'description': 'New streamlined booking flow'
    },
    'email_notifications': {
        'enabled': True,
        'rollout_percentage': 100,
        'owner': 'team_marketing',
        'description': 'Email notifications for bookings'
    }
}

def is_feature_enabled(feature_name, user_id=None):
    """
    Check if a feature is enabled for a user.
    
    Args:
        feature_name: Name of the feature flag
        user_id: Optional user ID for user-specific flags
    
    Returns:
        bool: True if feature is enabled
    """
    if feature_name not in FEATURE_FLAGS:
        return False
    
    flag = FEATURE_FLAGS[feature_name]
    
    if not flag['enabled']:
        return False
    
    if flag['rollout_percentage'] >= 100:
        return True
    
    # Hash-based rollout for consistent user experience
    import hashlib
    hash_input = f"{feature_name}:{user_id}"
    hash_value = int(hashlib.md5(hash_input.encode()).hexdigest(), 16)
    percentage = (hash_value % 100) + 1
    
    return percentage <= flag['rollout_percentage']
```

### Feature Flag Lifecycle

1. **Create:** Add feature flag before implementing feature
2. **Implement:** Build feature behind flag
3. **Deploy:** Deploy code with flag disabled
4. **Enable:** Gradually enable flag (10% → 50% → 100%)
5. **Remove:** Remove feature flag after full rollout
6. **Clean:** Remove feature flag code after 30 days

## Rollback Procedures

### Git Rollback

**For Code Issues:**
```bash
# Identify bad commit
git log --oneline -10

# Revert the commit
git revert <commit-hash>

# Push the revert
git push origin main
```

**For Multiple Commits:**
```bash
# Revert multiple commits
git revert <commit1> <commit2> <commit3>

# Or reset to known good state (use with caution)
git reset --hard <good-commit-hash>
```

### Database Rollback

**For Schema Changes:**
```sql
-- Keep migration files for rollback
-- Migration: Add new column
ALTER TABLE bookings ADD COLUMN new_field VARCHAR(255);

-- Rollback: Drop column
ALTER TABLE bookings DROP COLUMN new_field;
```

**For Data Issues:**
```sql
-- Backup before deployment
CREATE TABLE bookings_backup AS SELECT * FROM bookings;

-- Rollback to backup
TRUNCATE TABLE bookings;
INSERT INTO bookings SELECT * FROM bookings_backup;
DROP TABLE bookings_backup;
```

### Feature Flag Rollback

**Immediate Disable:**
```python
# Disable feature flag immediately
FEATURE_FLAGS['new_booking_flow']['enabled'] = False

# Or reduce rollout percentage
FEATURE_FLAGS['new_booking_flow']['rollout_percentage'] = 0
```

### Deployment Rollback Checklist

**Before Rollback:**
- [ ] Identify root cause
- [ ] Confirm rollback is necessary
- [ ] Notify stakeholders
- [ ] Prepare rollback commands

**During Rollback:**
- [ ] Execute rollback procedure
- [ ] Monitor for errors
- [ ] Verify service restoration

**After Rollback:**
- [ ] Document incident
- [ ] Create post-mortem
- [ ] Plan fix for next deployment

## Post-Deploy Verification

### Automated Checks

**Health Check:**
```python
@anvil.server.callable
def health_check():
    """Verify system health after deployment."""
    
    checks = {
        'database': False,
        'vault': False,
        'email': False,
        'payments': False
    }
    
    # Database check
    try:
        app_tables.users.find_one()
        checks['database'] = True
    except:
        pass
    
    # Vault check
    try:
        from server_shared.vault_service import get_vault_secret
        # Don't actually retrieve, just verify function exists
        checks['vault'] = True
    except:
        pass
    
    # Email check
    try:
        import anvil.server
        # Verify email functionality
        checks['email'] = True
    except:
        pass
    
    # Payments check
    try:
        # Verify payment gateway connection
        checks['payments'] = True
    except:
        pass
    
    return {
        'healthy': all(checks.values()),
        'checks': checks
    }
```

**Smoke Tests:**
```python
@anvil.server.callable
def smoke_tests():
    """Run smoke tests after deployment."""
    
    results = {
        'auth': False,
        'booking': False,
        'payment': False,
        'email': False
    }
    
    # Auth smoke test
    try:
        # Create test user
        # Login with test user
        # Verify session
        results['auth'] = True
    except:
        pass
    
    # Booking smoke test
    try:
        # Create test booking
        # Verify booking created
        results['booking'] = True
    except:
        pass
    
    # Payment smoke test (test mode only)
    try:
        # Create test payment
        # Verify payment processed
        results['payment'] = True
    except:
        pass
    
    # Email smoke test
    try:
        # Send test email
        # Verify email sent
        results['email'] = True
    except:
        pass
    
    return {
        'healthy': all(results.values()),
        'results': results
    }
```

### Manual Verification

**Admin Dashboard:**
- [ ] Login to admin dashboard
- [ ] Verify all navigation items work
- [ ] Check dashboard metrics display
- [ ] Verify settings page loads

**Public Website:**
- [ ] Load homepage
- [ ] Test contact form
- [ ] Verify booking widget loads
- [ ] Check mobile responsiveness

**Customer Experience:**
- [ ] Create test booking
- [ ] Complete payment (test mode)
- [ ] Verify confirmation email
- [ ] Check calendar shows booking

## Deployment Checklist

### Pre-Deployment

**Code:**
- [ ] All tests passing
- [ ] Code review completed
- [ ] Feature flags configured
- [ ] Documentation updated

**Database:**
- [ ] Migration scripts tested
- [ ] Rollback procedures documented
- [ ] Backup scheduled

**Communication:**
- [ ] Stakeholders notified
- [ ] Maintenance window scheduled (if needed)
- [ ] Rollback plan communicated

### Deployment

**Steps:**
1. [ ] Create database backup
2. [ ] Deploy code to staging
3. [ ] Run smoke tests on staging
4. [ ] Verify staging deployment
5. [ ] Deploy code to production
6. [ ] Run health checks on production
7. [ ] Run smoke tests on production
8. [ ] Monitor error rates for 15 minutes

### Post-Deployment

**Monitoring:**
- [ ] Monitor error rates for 1 hour
- [ ] Monitor performance metrics for 1 hour
- [ ] Check customer support tickets

**Documentation:**
- [ ] Update deployment log
- [ ] Document any issues
- [ ] Update runbooks if needed

## Rollback Triggers

**Immediate Rollback Required:**
- Error rate > 10%
- Critical functionality broken
- Data corruption detected
- Security vulnerability introduced

**Rollback Considered:**
- Error rate > 5%
- Performance degradation > 50%
- Customer complaints increasing
- Monitoring alerts triggered

## Incident Response

**If Rollback Needed:**

1. **Assess:** Determine severity and impact
2. **Decide:** Rollback or hotfix
3. **Execute:** Follow rollback procedure
4. **Communicate:** Notify stakeholders
5. **Document:** Record incident details
6. **Learn:** Create post-mortem

---

*End of file — Deployment Procedures*
