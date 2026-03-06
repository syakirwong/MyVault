---
tags:
  - gdd
  - technical
  - security
---

# Account & Security System

## Design Principle

Account security in Stormbound Online is not a feature bolted onto the game — it is load-bearing infrastructure. The [[Trade System]] eliminates P2P asset transfer, which closes most RMT vectors. But account selling remains the primary residual threat. This system exists to make account selling detectable, difficult, and economically pointless.

Every decision below optimizes for **player trust and economic integrity** while minimizing friction for legitimate players.

---

## 1. Account Creation Flow

### Registration Options

| Method | Details |
|--------|---------|
| **Email + Password** | Standard registration. Email verification required before gameplay |
| **OAuth — Google** | One-click signup via Google account |
| **OAuth — Discord** | One-click signup via Discord (primary community platform) |
| **OAuth — Apple** | Required for iOS clients. Supports "Hide My Email" |

OAuth accounts still require a verified email on file for recovery purposes. If the OAuth provider does not share the email, the player is prompted to provide one during onboarding.

### Username Rules

| Rule | Specification |
|------|---------------|
| Length | 3-16 characters |
| Allowed characters | Alphanumeric, underscores, hyphens |
| Case sensitivity | Display preserves case; uniqueness check is case-insensitive |
| Profanity filter | Applied at creation. Uses blocklist + heuristic substitution detection |
| Rename | One free rename. Additional renames: $5 each. 30-day cooldown between renames |
| Reserved names | NPC names, system terms, faction names, offensive terms |

### Character Creation Sequence

1. Account created and verified
2. Player selects **Origin** (see [[Experience & Levelling]] for Origin definitions)
3. Origin determines starting region, initial stat distribution, and first gem domain access
4. Player names their character (same rules as username, but character names are not globally unique — displayed with account tag: `Kael#7291`)
5. Brief onboarding sequence introduces reactive UI, gem equipping, and first combat encounter
6. Character enters the world at their Origin's starting hub

---

## 2. Authentication

### Login Flow

```
Player → Enters credentials (email/password or OAuth)
      → Server validates credentials
      → If 2FA enabled → Prompt for 2FA code
      → If new device → Prompt for device verification (email code)
      → Session token issued (JWT)
      → Player enters game
```

### Session Management

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| Session token type | JWT (access + refresh pair) | Stateless validation at edge |
| Access token lifetime | 15 minutes | Limits window of stolen token abuse |
| Refresh token lifetime | 30 days | Avoids daily re-authentication |
| Refresh rotation | Single-use refresh tokens. Each refresh issues a new pair | Detects token theft (reuse = revoke all) |
| Idle timeout | 60 minutes of no input | Prevents unattended sessions |
| Absolute session limit | 7 days | Forces periodic re-authentication regardless of activity |

### Concurrent Session Policy

**One active game session per account.** If a new login occurs while a session is active:

1. New login receives a warning: "Another session is active. Logging in will disconnect it."
2. If confirmed, the existing session is terminated with a message: "You have been disconnected — another login was detected."
3. A 30-second cooldown prevents rapid session ping-pong (anti-grief measure if credentials are compromised)

**Exception:** The account management web portal allows a separate concurrent session for settings, marketplace browsing, and support tickets. This does not conflict with the game session.

---

## 3. Two-Factor Authentication (2FA)

### 2FA Options

| Method | Priority | Details |
|--------|----------|---------|
| **TOTP (Authenticator App)** | Primary | Google Authenticator, Authy, or any TOTP-compatible app. Standard 6-digit codes, 30-second rotation |
| **Email Code** | Fallback | 6-digit code sent to verified email. 5-minute expiry. Rate-limited to 5 requests per hour |

SMS-based 2FA is deliberately excluded. SIM-swapping is a well-documented attack vector with low cost of execution.

### 2FA Requirement Gates

| Action | 2FA Required? | Rationale |
|--------|---------------|-----------|
| Login from trusted device | No | Minimize friction for daily players |
| Login from new device | Yes (mandatory) | Primary account-selling detection point |
| Marketplace listing > 5,000 gold | Yes | Protect high-value economic actions |
| Marketplace purchase > 10,000 gold | Yes | Protect high-value economic actions |
| Password change | Yes | Prevent credential takeover |
| Email change | Yes + email confirmation to OLD address | Prevent silent account hijack |
| 2FA method change | Yes + 24-hour delay | Prevent attacker from removing 2FA |
| Character deletion | Yes | Irreversible action protection |
| Account deletion | Yes + 72-hour delay | GDPR compliance with safety margin |

### 2FA Incentive

Players who enable 2FA receive a permanent **"Secured" badge** on their build card and a one-time cosmetic reward (unique title suffix: "the Warded"). This is not gated gameplay content — it is a visible signal that encourages adoption.

**Target:** 70%+ of active players with 2FA enabled within 6 months of launch.

---

## 4. Device Binding

### Device Fingerprinting

Each device generates a composite fingerprint from:

| Signal | Weight | Notes |
|--------|--------|-------|
| Hardware ID (client-generated) | High | OS-level device identifier |
| Browser/client signature | Medium | User agent, screen resolution, client version |
| IP geolocation (region-level) | Low | Used for anomaly detection, not fingerprinting |

Fingerprints are hashed and stored server-side. Raw device data is never persisted (privacy compliance).

### Trusted Device Management

| Parameter | Value |
|-----------|-------|
| Maximum trusted devices | 5 per account |
| Trust duration | 90 days of inactivity. Resets on each login from the device |
| Adding a new device | Requires 2FA (if enabled) or email verification code |
| Removing a device | Available in account settings. Immediate effect |
| Exceeding device limit | Oldest inactive device is automatically removed when a 6th device is added |

### New Device Login Flow

```
New device detected →
  1. Credential validation (normal)
  2. 2FA prompt (if enabled) OR email verification code (if 2FA not enabled)
  3. Player receives notification: "New login from [Device Type] in [Region]"
     (sent to email + in-game notification on next trusted device login)
  4. Option to "Trust this device" or "One-time login"
  5. If trusted: device added to trusted list
```

### Device Change Velocity

More than 3 new device logins within 7 days triggers an automatic flag for review. This is a primary **account-selling signal** — legitimate players do not cycle through devices rapidly.

---

## 5. Account Security

### Password Requirements

| Requirement | Specification |
|-------------|---------------|
| Minimum length | 12 characters |
| Complexity | At least 1 uppercase, 1 lowercase, 1 digit, 1 special character |
| Breach check | Passwords checked against Have I Been Pwned API (k-anonymity model) at creation and each change |
| Common password block | Top 100,000 common passwords rejected |
| History | Cannot reuse last 5 passwords |
| Max age | No forced rotation (NIST 800-63B compliance). Rotation encouraged only on breach detection |

### Breach Detection

- **Credential stuffing monitoring:** Track failed login attempts per account AND per IP. Patterns indicating automated credential stuffing trigger IP-level rate limiting
- **External breach alerts:** If a third-party breach is detected involving email domains common in the player base, trigger a targeted password reset advisory (not forced — forced resets cause more harm than good unless compromise is confirmed)
- **Compromised password detection:** Periodic background check of stored password hashes against newly published breach databases (k-anonymity, no plaintext comparison)

### Suspicious Login Detection

| Signal | Threshold | Response |
|--------|-----------|----------|
| New IP address | First login from IP not seen in 30 days | Log + email notification |
| New geographic region | Login from country/region never seen for this account | 2FA challenge + email notification |
| Velocity: multiple regions | Logins from 2+ regions within 1 hour (impossible travel) | Session blocked. Email verification required |
| Failed login attempts | 5 failures in 10 minutes | Progressive delay (exponential backoff starting at 30s) |
| Failed login attempts | 15 failures in 1 hour | Account locked for 1 hour. Email notification sent |
| Failed 2FA attempts | 5 failures | 2FA locked for 30 minutes. Email notification |
| Login time anomaly | Login at time with <1% historical activity for this account | Log + silent flag (no user-facing action) |

### Brute Force Protection

| Layer | Mechanism |
|-------|-----------|
| Per-account | Exponential backoff after 5 failures. Lockout after 15. Unlock via email or 1-hour wait |
| Per-IP | Rate limit: 20 login attempts per IP per 10 minutes. Exceeded = IP blocked for 30 minutes |
| Global | If login failure rate exceeds 10x baseline across all accounts, trigger CAPTCHA challenge on all login attempts (DDoS/credential-stuffing response) |

---

## 6. Account Recovery

### Password Reset Flow

```
Player → Requests password reset
      → Email sent with single-use reset link (1-hour expiry)
      → Link opens reset form (requires new password meeting all requirements)
      → If 2FA enabled: 2FA code required to complete reset
      → All existing sessions invalidated
      → All trusted devices revoked (player must re-trust)
      → Confirmation email sent
```

### Lost 2FA Recovery

| Recovery Method | Process | Turnaround |
|-----------------|---------|------------|
| **Backup codes** (primary) | 10 single-use backup codes generated when 2FA is enabled. Each code works once. Player stores them offline | Instant |
| **Support verification** | Player contacts support with: account email, last 3 characters logged in, approximate account creation date, last purchase receipt (if any) | 24-48 hours |
| **Identity verification (escalated)** | If standard support verification fails: government ID matching account holder name (if name was provided). Used only as last resort | 3-5 business days |

**Backup code policy:** When 2 or fewer backup codes remain, the player is warned at every login. When all codes are used, the player is strongly prompted (but not forced) to regenerate a new set.

### Email Change Process

1. Player initiates email change from account settings
2. 2FA required (if enabled)
3. Confirmation sent to **current** email: "Did you request this change? Click to confirm or click to deny"
4. If confirmed: verification email sent to **new** email
5. New email confirmed: change takes effect
6. 72-hour rollback window: player can revert to old email without any verification (panic button for compromised accounts)
7. Notification sent to both old and new email addresses

### Compromised Account Procedure

If a player reports their account as compromised:

1. **Immediate:** Account locked. All sessions terminated. All trusted devices revoked
2. **Within 1 hour:** Support verifies identity (see Lost 2FA Recovery methods)
3. **Upon verification:** Password reset forced. 2FA reset forced. New backup codes issued
4. **Review:** Last 7 days of account activity audited for unauthorized marketplace transactions. Any suspicious transactions flagged for manual economic review
5. **Restoration:** If unauthorized transactions are confirmed, items/gold are restored within the limits of the [[Economy Model]] integrity rules (no duplication — restoration comes from a system reserve fund)
6. **Follow-up:** Account placed on 30-day enhanced monitoring (all logins trigger email notification regardless of device trust)

---

## 7. Anti-Account-Selling

The [[Trade System]] eliminates most RMT vectors by design (no P2P transfer). Account selling — transferring an entire account for real money — is the residual threat.

### Detection Signals

| Signal | Data Source | Weight |
|--------|------------|--------|
| **Behavioral identity shift** | [[Behavioral Identity]] system — sudden archetype change not explained by gradual drift | Critical |
| **Login region change** | New country/region login that persists (not travel) | High |
| **Device fingerprint change** | All trusted devices replaced within a short window | High |
| **Play schedule shift** | Login time distribution shifts by 6+ hours (timezone change) | Medium |
| **Combat pattern change** | Action selection patterns diverge from historical baseline (uses behavioral axes data) | Medium |
| **Social graph disruption** | Player stops interacting with established contacts, joins new social circles | Medium |
| **Credential change cluster** | Password + email + 2FA all changed within 48 hours | High |
| **Name change** | Username or character rename shortly after other signals | Low (supporting) |

### Composite Score

Each signal contributes to a **Transfer Likelihood Score (TLS)** on a 0-100 scale. Signals are weighted and time-decayed. The system looks for **clustering** — any single signal is insufficient.

```
TLS = SUM(signal_weight * signal_confidence * time_decay_factor)

Thresholds:
  TLS < 30  → No action (normal variance)
  TLS 30-50 → Silent flag. Enhanced monitoring for 30 days
  TLS 50-70 → Active investigation. Account flagged for manual review
  TLS 70-85 → Automated restriction: marketplace access suspended pending review
  TLS > 85  → Account locked pending investigation
```

### Response Escalation

| Stage | Trigger | Action | Player-Facing |
|-------|---------|--------|---------------|
| **Flag** | TLS 30-50 | Enhanced monitoring. All signals logged at higher fidelity | None (silent) |
| **Investigate** | TLS 50-70 | Manual review queue. Support analyst reviews signal cluster | None unless analyst initiates contact |
| **Restrict** | TLS 70-85 | Marketplace suspended. Character progression continues normally. Player notified: "Unusual activity detected — marketplace access paused for review" | Marketplace lock notification |
| **Lock** | TLS > 85 | Account locked. Player must verify identity to regain access | Full account lock notification with recovery instructions |
| **Ban** | Investigation confirms sale | Permanent ban. See Ban & Suspension System below | Ban notification with appeal instructions |

### Integration with Behavioral Identity

The [[Behavioral Identity]] system tracks 10 behavioral axes over a rolling 50-hour window. A legitimate player's axes shift gradually (~8-10 hours of consistent new behavior to move meaningfully). A new owner produces a sudden, multi-axis divergence that is statistically distinct from natural drift.

The anti-account-selling system subscribes to behavioral identity change events. When the rate of change across 3+ axes exceeds 2 standard deviations from the player's historical norm within a 24-hour window, it contributes a Critical-weight signal to the TLS calculation.

---

## 8. Ban & Suspension System

### Offense Categories

| Category | Examples | Severity |
|----------|----------|----------|
| **Exploitation** | Duplication bugs, economy exploits, client manipulation | High |
| **Account selling** | Confirmed account transfer for real money | High |
| **Botting** | Automated gameplay, macro abuse | High |
| **Harassment** | Targeted abuse, hate speech, doxxing | Medium-High |
| **Disruption** | Intentional griefing, feed-killing in PvP | Medium |
| **Minor conduct** | Spam, mild toxicity, inappropriate names | Low |

### Escalation Ladder

| Step | Duration | Conditions | Reversible? |
|------|----------|-----------|-------------|
| **Warning** | N/A | First minor offense. Logged on account | Yes (expires after 90 days clean) |
| **Mute** | 24 hours - 7 days | Chat-specific offenses (harassment, spam) | Yes |
| **Temporary ban** | 3 days | Second minor offense OR first medium offense | Yes |
| **Extended ban** | 14 days | Repeated medium offenses OR first high-severity offense (non-exploitation) | Yes |
| **Permanent ban** | Indefinite | Confirmed exploitation, account selling, or repeated high-severity offenses | Appeal possible |

### Escalation Rules

- Warnings expire after 90 days. A player with a clean 90-day record has their escalation level reduced by one step
- Multiple categories compound: harassment + botting = treated as separate escalation tracks. A player can be muted for harassment while receiving a temp ban for botting simultaneously
- Exploitation and account selling skip to Extended or Permanent ban. These do not follow the gradual ladder

### Appeal Process

1. Player submits appeal through support portal (available even when banned)
2. Appeal must include: account identifier, ban reason acknowledgment, explanation
3. Appeals reviewed by a different support agent than the one who issued the ban
4. Response within 5 business days
5. One appeal per ban. If denied, the decision is final
6. Permanent bans may be re-appealed once per year

### Data Retention During Ban

| Data Type | Retention | Reason |
|-----------|-----------|--------|
| Account credentials | Retained | Prevent re-registration with same credentials |
| Character data | Retained for 180 days (temp ban) / 365 days (permanent) | Enable restoration if ban is overturned |
| Behavioral identity data | Retained for 365 days | Evidence for appeals and pattern analysis |
| Chat logs (related to offense) | Retained for 365 days | Evidence retention |
| Economic transaction history | Retained for 365 days | Fraud analysis |
| After retention period | Anonymized and aggregated | Statistical analysis only |

---

## 9. Privacy & Compliance

### GDPR Compliance

Stormbound Online processes personal data under **legitimate interest** (account security, anti-fraud) and **contractual necessity** (providing the game service). Consent is used for optional data processing (marketing, analytics beyond core functionality).

### Data Categories

| Data | Legal Basis | Retention |
|------|-------------|-----------|
| Email address | Contractual necessity | Account lifetime + 30 days post-deletion |
| Password hash | Contractual necessity | Account lifetime |
| IP addresses | Legitimate interest (security) | 90 days rolling |
| Device fingerprints (hashed) | Legitimate interest (security) | 90 days after device removed from trusted list |
| Behavioral identity data | Contractual necessity (gameplay feature) | Account lifetime. Deleted with account |
| Chat logs | Legitimate interest (moderation) | 90 days rolling (365 if flagged for offense) |
| Economic transactions | Legitimate interest (fraud prevention) | Account lifetime. Anonymized 30 days post-deletion |
| Payment information | Handled by payment processor (Stripe/equivalent). Not stored on game servers | Per processor policy |

### Data Export (Right of Access)

Players can request a full data export from account settings:

- Export generated within 72 hours
- Includes: account info, character data, behavioral identity history, economic transaction history, chat log history (last 90 days), device list, login history (last 90 days)
- Delivered as downloadable JSON + human-readable summary
- Rate limited: 1 export per 30 days

### Account Deletion (Right to be Forgotten)

1. Player requests deletion from account settings
2. 2FA required (if enabled)
3. **72-hour cooling period** — player can cancel during this window
4. After 72 hours: account enters deletion queue
5. **Within 30 days:**
   - All personal data deleted (email, password hash, device data, IP logs)
   - Character data deleted
   - Behavioral identity data deleted
   - Chat logs deleted (unless flagged for active moderation case)
   - Economic transactions **anonymized** (transaction records retained with anonymized IDs for economy integrity, but no link to the deleted account)
   - Marketplace listings canceled, escrowed items destroyed (gold value refunded to system reserve)
   - Gem provenance records anonymized (crafter signatures replaced with "[Deleted Account]")
6. Deletion is **irreversible**. The username and character names become available for reuse after 180 days

### Consent Management

| Consent Type | Default | Required? |
|-------------|---------|-----------|
| Terms of Service | Must accept | Yes (cannot play without) |
| Privacy Policy | Must accept | Yes |
| Marketing emails | Opt-out | No |
| Analytics (gameplay telemetry) | Opt-in | No (core analytics under legitimate interest; enhanced analytics require consent) |
| Third-party data sharing | Opt-in | No (no third-party sharing by default) |

Consent preferences are accessible from account settings at any time. Changes take effect within 24 hours.

### Age Verification

| Region | Minimum Age | Verification Method |
|--------|-------------|-------------------|
| Global default | 13 | Self-declared date of birth at registration |
| EU (GDPR) | 16 (or member state minimum) | Self-declared. Parental consent flow for 13-15 |
| South Korea | 14 | Self-declared + parental consent for under-14 |
| China | 18 (or with guardian consent) | Real-name verification per local regulation |

Accounts flagged as under-age are restricted: no marketplace access, no chat (safety mode), limited play hours per day (where legally required). Parental consent flow: parent email verification + consent confirmation link.

---

## 10. Character Slots

### Slots Per Account

| Slot Type | Count | Cost |
|-----------|-------|------|
| Free slots | 2 | Included with account |
| Purchasable slots | Up to 4 additional | $5 each (see [[Monetization]]) |
| Maximum total | 6 | Hard cap |

### Character Deletion Policy

| Rule | Specification |
|------|---------------|
| Deletion confirmation | 2FA required (if enabled) + type character name to confirm |
| Cooldown | 7-day grace period before deletion executes. Player can cancel during this window |
| Post-deletion cooldown | 24-hour cooldown before the freed slot can be used for a new character |
| Deleted character name | Available for reuse by the same account immediately. Available to other accounts after 90 days |
| Items and currency | All character-bound items destroyed. Account-bound items transferred to account vault (accessible by other characters) |
| Marketplace listings | Active listings canceled. Escrowed items returned to account vault |
| Behavioral identity | Deleted with the character. Does not transfer to other characters |
| Gem provenance | Crafter signatures on gems created by the deleted character persist as "[Character Name] (deleted)" |

### Character Transfer Rules

**Characters cannot be transferred between accounts.** This is a deliberate anti-RMT measure. Combined with no P2P trading, this ensures that selling an account is the only way to transfer value — and the anti-account-selling system is designed specifically to detect and prevent that.

**Characters cannot be transferred between servers** at MVP. Cross-server character transfer may be evaluated post-launch if multiple server regions are deployed, subject to economic integrity review.

---

## Implementation Priority

| Feature | Priority | MVP? |
|---------|----------|------|
| Email + OAuth registration | Critical | Yes |
| Password security (breach check, requirements) | Critical | Yes |
| Session management (JWT, concurrent policy) | Critical | Yes |
| 2FA (TOTP + email fallback) | Critical | Yes |
| Device binding (basic: trusted devices, new device flow) | High | Yes |
| Suspicious login detection | High | Yes |
| Brute force protection | High | Yes |
| Account recovery (password reset, backup codes) | High | Yes |
| Ban & suspension system (basic ladder) | High | Yes |
| Character slots and deletion | High | Yes |
| GDPR compliance (deletion, export) | High | Yes (legal requirement) |
| Anti-account-selling (TLS scoring) | Medium | Post-MVP (requires behavioral identity data maturity) |
| Behavioral identity integration for anti-selling | Medium | Post-MVP |
| Age verification (regional) | Medium | Yes (legal requirement per region) |
| Compromised account procedure (full flow) | Medium | Yes (basic flow) |
| Enhanced monitoring and TLS automation | Low | Post-MVP |

---

## Architecture Decision Record

### Context

Stormbound Online's NPC-only [[Trade System]] eliminates P2P value transfer, closing the most common MMO security threats (RMT, scams, gold selling). Account selling is the residual vector. The security system must address this without creating friction that drives away legitimate players.

### Decision

Defense in depth: layered authentication (credentials + 2FA + device binding) combined with behavioral anomaly detection (via [[Behavioral Identity]] integration). Optimize for making account selling detectable and reversible rather than solely preventable.

### Trade-offs

| Gain | Sacrifice |
|------|-----------|
| Strong anti-account-selling through behavioral integration | False positives possible if a player legitimately changes playstyle rapidly |
| One-session-per-account simplifies state management | Players cannot multibox (intentional — fits the game's design) |
| No SMS 2FA eliminates SIM-swap vector | Players without authenticator apps must use email codes (slightly less secure) |
| 72-hour deletion delay prevents impulsive data loss | Frustrated players must wait to fully leave |
| Device binding limits account sharing | Legitimate device changes (new phone) require re-verification |

### Constraints

- 2FA must never be required for basic gameplay (login from trusted device, normal combat, normal progression)
- No security measure may create a pay-to-bypass scenario
- GDPR compliance is non-negotiable in all EU-serving regions
- Behavioral identity data used for anti-selling must be the same data used for gameplay (no hidden surveillance layer)

### Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Low 2FA adoption | Medium | High (account selling easier) | Cosmetic incentive + prominent UI prompts + mandatory for high-value marketplace actions |
| False positive TLS flags on legitimate players | Medium | Medium (player frustration) | Conservative thresholds. Marketplace restriction (not ban) as first automated response. Fast manual review queue |
| Credential stuffing at scale | Medium | High | Rate limiting + breach detection + CAPTCHA escalation |
| GDPR data deletion conflicts with anti-fraud retention | Low | High (legal) | Anonymization rather than deletion for economic transaction records. Legal review of retention periods |
| Device fingerprint evasion | Low | Medium | Fingerprint is one signal among many. Behavioral identity is the harder-to-fake layer |

---

## Related Documents

- [[Trade System]]
- [[Behavioral Identity]]
- [[Economy Model]]
- [[Monetization]]
- [[Experience & Levelling]]
- [[Cosmetic System]]
- [[GDD Hub]]
