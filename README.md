# The Pragmatic Firewall Engineer

## Introduction

- Purpose of this repository
- Briefly explain organization of this information
- Explain that I'll be taking a vendor neutral approach, but will use FortiGate syntax for examples.

## Naming conventions

- Explain why naming conventions are important
- Provide some example of naming address objects

```
config firewall address
    edit "vmname"
      set subnet 10.1.1.0 255.255.255.0
    next
end
```

## Creating policy

- Firewall policy is primarily based on three properties of network traffic: srcip, dstip, and dstport
- In a perfect world, every unique combination of those three properties would become its own firewall policy.
- That's not possible, so we have to group certain traffic together
- Explain how to decide how to group things together
  - sometimes we collapse traffic by broadening the srcip, dstip, or dstport
  - we should be careful of broadening more than one of the three

## Staging changes

- If your firewall is capable of staging changes, then stage changes
- Explain how it works and why its useful

> [!WARNING]
> Many firewalls have an immature or incomplete implementation of change staging. Be sure you know how staging works, what the limitations are, and where the pitfalls are for your firewall vendors implementation of change staging.

> [!NOTE]  
> Highlights information that users should take into account, even when skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]  
> Crucial information necessary for users to succeed.

> [!WARNING]  
> Critical content demanding immediate user attention due to potential risks.

> [!CAUTION]
> Negative potential consequences of an action.
