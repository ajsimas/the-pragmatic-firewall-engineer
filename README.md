# The Pragmatic Firewall Engineer

## Introduction

- Purpose of this repository
- Briefly explain organization of this information
- Explain that I'll be taking a vendor neutral approach, but will use FortiGate syntax for examples.
- Talk about balance of security vs convenience

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

## Object management

- Discuss automating object distribution and how that can be accomplished

## Policy

### Creating policy

- Firewall policy is primarily based on three properties of network traffic: srcip, dstip, and dstport
- In a perfect world, every unique combination of those three properties would become its own firewall policy.
- That's not possible, so we have to group certain traffic together
- Explain how to decide how to group things together
  - sometimes we collapse traffic by broadening the srcip, dstip, or dstport
  - we should be careful of broadening more than one of the three

### Policy order

- More specific policies up to and why
- examples

### Cleaning up overly permissive rules

- Importance of being methodical
- How to triage

### Geoblocking

- This is easy and typically doesn't cause too many problems
- It doesn't solve many problems

## Staging changes

- If your firewall is capable of staging changes, then stage changes
- Explain how it works and why its useful

> [!WARNING]
> Many firewalls have an immature or incomplete implementation of change staging. Be sure you know how staging works, what the limitations are, and where the pitfalls are for your firewall vendors implementation of change staging.

### Logging

- Log everything
