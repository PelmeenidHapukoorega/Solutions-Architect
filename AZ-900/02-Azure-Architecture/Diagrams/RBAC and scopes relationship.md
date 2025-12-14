```ascii
Azure RBAC – Roles and Scopes relationship

                     ┌───────────────────────┐
                     │   Management Group    │
                     │   (Scope level)       │
                     └───────────┬───────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
            Role: Owner                  Role: Reader
            (Full control)              (Read-only)
                    │                         │
                    v                         v
             ┌─────────────────┐     ┌──────────────────┐
             │  Subscription   │     │   Subscription   │
             │  (Scope level)  │     │   (Scope level)  │
             └────────┬────────┘     └─────────┬────────┘
                      │                        │
          ┌───────────┴───────────┐   ┌────────┴─────────┐
          │                       │   │                  │
   Role: Owner               Role: Reader           Role: Reader
   (Manage resources)        (View only)            (View only)
          │                       │
          v                       v
   ┌───────────────┐        ┌──────────────┐
   │ Resource Group│        │Resource Group│
   │ (Scope level) │        │ (Scope level)│
   └───────────────┘        └──────────────┘


Legend:
• Scope = where access applies (management group, subscription, resource group)
• Role = what actions are allowed (Owner, Reader, etc.)
• Roles are assigned *at a scope* and apply downward unless overridden
```
