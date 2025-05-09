# The Pragmatic Firewall Engineer

## Table of contents

1. [Introduction](#introduction)
2. [Contributing](/Contributing.md)
3. [Core design principles](/Core.md)
   - [Understand your network](/Core.md#understand-your-network)
   - [Establish conventions](/Core.md#establish-conventions)
   - [Rule ordering strategy](/Core.md#rule-ordering-strategy)
   - [Logging](/Core.md#logging)
   - [Security profiles and layered defenses](/Core.md#security-profiles-and-layered-defenses)
   - [Change management](/Core.md#change-management)

## Introduction

Firewall policy is more than just a list of access rules. It is a strategic,
living configuration that defines the trust relationships within a network.
Despite its importance, firewall policy is often created hastily and without a
clear methodology or long-term plan. This guide is written for professional
firewall administrators who want to design policy that is not only secure and
efficient but also scalable, maintainable, and audit-friendly. Rather than
focusing on technical steps or product-specific features, this guide emphasizes
the “why” behind policy decisions. This guide presents a vendor-neutral
framework that includes proven best practices, and practical wisdom. Whether
you're building from scratch or cleaning up years of legacy rules, this guide
will help you approach firewall policy with intention and clarity.
