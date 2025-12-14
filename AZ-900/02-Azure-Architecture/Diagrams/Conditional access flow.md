```ascii
Conditional Access flow (signals → decision → enforcement)

            ┌───────────────────────────────┐
            │           User sign-in        │
            └───────────────┬───────────────┘
                            │
                            v
            ┌────────────────────────────────┐
            │  Collect signals during sign-in│
            │  • User / group                │
            │  • Location (known / unknown)  │
            │  • Device (compliant / not)    │
            │  • Risk / unusual sign-in      │
            │  • App / resource              │
            └───────────────┬────────────────┘
                            │
                            v
            ┌───────────────────────────────┐
            │   Evaluate Conditional Access │
            │   policies against signals    │
            └───────────────┬───────────────┘
                            │
                            v
            ┌───────────────────────────────┐
            │            Decision           │
            │   ┌──────────────┬──────────┐ │
            │   │ Allow access  │ Block   │ │
            │   │               │ access  │ │
            │   └───────┬───────┴─────┬───┘ │
            │           │             │     │
            │           v             v     │
            │   (Optional) Challenge MFA    │
            └───────────────┬───────────────┘
                            │
                            v
            ┌───────────────────────────────┐
            │           Enforcement         │
            │ • Allow (no MFA, trusted)     │
            │ • Allow after MFA             │
            │ • Deny / Block                │
            └───────────────────────────────┘
```
