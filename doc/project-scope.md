# Project scope

## Audience

This guide is for a person with access to a functioning, substantially stock Kimai installation who has little or no previous Kimai experience.

The primary reader may be an administrator, freelancer, small-business owner, team lead, or another person responsible for turning an empty Kimai installation into a usable time-tracking and invoicing system.

## Assumptions

- Kimai is already installed and functioning.
- The reader can sign in through the web interface.
- The reader has sufficient administrative privileges for the configuration being discussed.
- Configuration described by this guide is performed through the Kimai web interface.
- The reader may not yet understand Kimai's terminology, data model, permission model, or billing model.

## Goals

- Develop the reader's mental model of Kimai before asking them to configure it.
- Explain what should be configured, in what order, and why.
- Provide decision support rather than merely enumerate fields and settings.
- Identify features that a new installation can safely ignore until a real need appears.
- Use realistic worked examples to make abstract concepts concrete.
- Preserve links to authoritative upstream Kimai documentation.
- Help the reader reach a small, understandable, production-usable configuration.

## Non-goals

This project does not attempt to document:

- installing or upgrading Kimai;
- Docker, Kubernetes, container orchestration, or hosting;
- web servers, reverse proxies, DNS, TLS, or networking;
- PHP, databases, or command-line administration;
- APIs or programmatic configuration;
- authentication infrastructure outside the Kimai web interface;
- plugin-specific installation or configuration; or
- every field, setting, permission, report, or feature available in Kimai.

The official Kimai documentation remains the authoritative reference for exhaustive feature documentation.

## Editorial contract

Every substantive statement should fit one of three categories:

1. **Kimai behavior**: a factual statement about current Kimai behavior that can be supported by authoritative documentation.
2. **Recommendation**: guidance from this project about a sensible way to begin or a tradeoff worth considering.
3. **Example**: fictional data used to demonstrate a concept or workflow.

These categories should not be blurred.  A local recommendation must not be presented as a Kimai requirement, and an example must not imply that its structure is the only correct way to use Kimai.

## First milestone

The first meaningful release should let a newcomer complete this journey through the web interface:

1. Understand Kimai's basic data model.
2. Plan a small initial structure.
3. Review important defaults.
4. Create the reader's own organization.
5. Create a customer.
6. Create a project.
7. Create useful activities.
8. Configure an appropriate rate.
9. Record and review a time entry.
10. Create and review an invoice.

Users, roles, teams, tags, lifecycle management, and more advanced features should be added around that core journey without obscuring it.
