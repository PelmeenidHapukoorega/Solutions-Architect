```ascii
# Azure hierarchy: Management Groups, Subscriptions, Resource Groups

Management Group
└── Management Group (optional child)
    └── Subscription
        ├── Resource Group
        │   ├── Resource (VM)
        │   ├── Resource (Storage Account)
        │   └── Resource (VNet)
        └── Resource Group
            ├── Resource (App Service)
            └── Resource (Key Vault)

# Flattened view

Tenant (Azure AD / Entra ID)
└── Management Group(s)
    └── Subscription(s)
        └── Resource Group(s)
            └── Resource(s)
```
