# Core design principles

Designing effective firewall policy requires consistent methodology and a
structured approach that can be repeated, defended, and maintained over time.
The principles in this section are not specific to any vendor or platform.
Instead, they reflect a strategic mindset that helps firewall administrators
create rulesets that are secure, scalable, and operationally sustainable.
Whether you're starting from scratch or refining an existing deployment, these
principles serve as a foundation for making informed, intentional policy
decisions.

## Understand your network

Before writing a single firewall rule, you must have a clear understanding of
the network you're defending. This includes knowing what systems exist, how they
communicate, which of those communications are critical to operations. and where
your crown jewels are.

1. Identify the systems and services that exist on your network.
2. Identify various trust zones that make up your network (such as internal
   networks, DMZs, cloud segments, partner connections, and the public internet)
3. Map out traffic flows between your trust zones. Consider not just current
   usage, but future scalability and potential risks introduced by new services.

Documenting these trust boundaries helps clarify where the firewall fits into
the larger architecture. It also reveals unintended exposures, like legacy rules
that allow overly broad access between sensitive systems. This awareness forms
the basis for enforcing least privilege, minimizing attack surface, and
designing rules that reflect intentional, necessary access.

Your firewall policy is only as good as your understanding of the environment it
protects. Make network visibility your first priority.

## Establish conventions

Conventions are essential for maintaining clarity, consistency, and long-term
maintainability in firewall policy. While the specific choices you make are less
important than the act of choosing and applying them consistently, it’s critical
that they are documented clearly and that your entire team understands and
follows them. This will prevent confusion, reduces errors, and simplify ongoing
management.

### Naming conventions

Naming conventions provide structure to your firewall configuration and make it
easier to understand, audit, and troubleshoot. Without consistent naming, even
well-designed rules and objects can become confusing or misleading over time. A
good naming convention should be descriptive, scoped, and predictable.

#### General guidelines

- Avoid abbreviations that are not widely understood. Use full words or
  well-known acronyms.
- Avoid using special characters or spaces in names, as they can cause
  unexpected compatibility issues across different systems.
- Use a consistent delimiter (like hyphens or underscores) to separate words in
  names.
- As much as possible, use names that are self-explanatory and meaningful.

> [!IMPORTANT]  
> When creating naming conventions, make sure you consider maximum name length
> limitations. For example, if your naming convention for firewall policies
> requires too much information (maybe source ip, destination ip, service, and
> ticket number), but your firewall only allows names of 32 characters, you will
> run into issues. Always check the maximum name length for your firewall
> platform and design your naming conventions accordingly.

#### Case convention

When it comes to choosing a case style, remember that consistency is more
important than choosing the "right" convention.

- camelCase (e.g., `webServerGroup`)
- PascalCase (e.g., `WebServerGroup`)
- snake_case (e.g., `web_server_group`)
- kebab-case (e.g., `web-server-group`)
- lowercase (e.g., `webservergroup`)

> [!NOTE]  
> It is generally to avoid using ALL CAPS for names, as this can become visually
> overwhelming and harder to read, especially in long lists of objects. If it is
> used at all, it should be reserved for special cases.

#### Object types that require naming conventions

Depending on your firewall platform, you may have different types of objects to
name, but common categories include:

- addresss objects
- address groups
- service objects
- service groups
- firewall policies
- security profiles
- NAT pools
- interfaces
- zones
- virtual routers

### Commenting conventions

- **Explain intent, not mechanics:** Avoid restating what the rule does (e.g.
  Allow TCP 443). Instead, expalin why the rule exists (e.g. required for
  month-end reporting).
- **Put a ticket number in the comment:** There is often too much information to
  include in a comment field (e.g. requester, justification, expiration date,
  documentation, creation date, creator, etc.). Instead, consider putting only a
  ticket number in your comment field and make sure all of the important context
  is in the ticket.
- **Use a consistent format:** Consistency is more important than the specific
  format you choose.

## Rule ordering strategy

The order of firewall rules directly affects both security and performance. This
guide is written for firewalls that evaluate rules in top-down order.

### General guidelines

- **Create logical groups:** Group similar rules together. For example, group
  rules by direction, site, application, service, or source/destination IP
  address. Use whatever grouping makes sense for your environment.
- **Separate user and server traffic:** With few exceptions, firewall rules
  should either be for user traffic or server traffic.
- **Place the most specific rules at the top:** Rules that match specific
  addresses, services, or applications should be placed before more general
  rules. This ensures that the most relevant rules are evaluated first.

## Logging

Well designed logging supports general network troubleshooting, security
investigations, and compliance audits. It also helps you understand how your
firewall is being used, which can inform future policy decisions.

### What to log

When in doubt, log it. Start from a position of logging everything and only
disable logging when you have to for performance or disk space limitations.

### Logging platform

There is a wide variety of logging platforms and they come in a wide variety of
usefulness. If your firewall vendor has a logging product, start there, but
don't stop there. SIEMs can also be useful for firewall logging (even for
non-security incident activities). At a minimum, your logging platform should be
able to:

- Aggregate logs from multiple firewalls
- Have robust search and filtering capabilities
- Support complex boolean logic
- Aggregate logs by one or more fields that you choose (see note below)

> [!NOTE]  
> The value that comes with robust custom log aggregation cannot be overstated.
> It allows administrators to distill millions of raw firewall events into
> actionable insights that identify top talkers, unexpected flows, misconfigured
> rules, or signs of malicious activity. Grouping logs by key dimensions like
> source IP, destination IP, and service port provides a high-level view of
> traffic behavior.

## Security profiles and layered defenses

### Application control

- Block obviously malicious applications (e.g. malware, botnets, etc.)
- Block applications that are not needed for business operations (e.g. P2P,
  proxies, etc.)
- Block legitimate applications that are often abused by threat actors (e.g. RMM
  tools, remote access tools, file transfer tools, etc.)
- Block applications on non-standard ports (e.g. DNS is the only application
  that can pass through a policy that only allows UDP/53)

### Deep packet inspection (DPI)

> [!NOTE]  
> Deep packet inspection is also known as SSL decryption, TLS decryption, SSL
> inspection, HTTPS inspection, and other names.

More than 90% of internet http traffic is encrypted using TLS. Many firewall
security features like antivirus, application control, and some IPS signatures
won't work for encrypted traffic. The solution is to perform a man-in-the-middle
(MITM) attack on yourself, through the clever use of public key infrastructure.
Traffic is encrypted at the client, sent to the firewall, decrypted on the
firewall, inspected by the firewall, re-encrypted on the firewall, and then sent
to the server. The same steps happen in reverse for return traffic. Each
firewall vendor has a unique way of implementing this functionality. Consult
your firewall documentation for specific instructions for your firewall.

> [!WARNING]  
> Many types of traffic are broken by DPI. Certificate pinning is the most
> common problem. Read more about certificate pinning here:
> [What is Certificate pinning?](https://learn.microsoft.com/en-us/azure/security/fundamentals/certificate-pinning)

> [!WARNING]  
> There may be legal implications to performing DPI on certain types of traffic.
> Consult your legal department before decrypting medical, financial, or other
> sensitive traffic.

### Intrusion prevention system (IPS)

IPS is a network security technology that monitors network and/or systems
activities for malicious activity. It can be used to detect and block malicious
traffic, as well as to alert security personnel to potential security incidents.
IPS can be deployed in a variety of ways, including as a network appliance, a
software application, or a cloud-based service.

#### General guidelines

Start by enabling all IPS signatures, but in an monitor/logging only mode. Once
enough time has passed, you should have the data you need to start strategically
setting certain IPS categories to block mode.

## Limitations and scalability

## Change management

## Microsegmentation and zero trust
