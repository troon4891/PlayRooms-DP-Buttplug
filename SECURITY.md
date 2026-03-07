# Security Policy

## Important: Physical Device Safety

This provider controls **physical devices** (vibrators, linear actuators, and other intimate hardware) via Buttplug.io and Intiface Engine. Security vulnerabilities in this software may have **physical safety implications** for users. We take security reports for this project extremely seriously.

Potential impacts of security issues include:

- Unauthorized device activation or control
- Bypassing emergency stop mechanisms
- Escalation of device intensity beyond user-set limits
- Unauthorized access to device discovery or approval workflows

## Supported Versions

| Version | Supported          |
|---------|--------------------|
| 1.x     | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

Instead, please report security issues by emailing the repository owner directly or by using GitHub's private vulnerability reporting feature:

1. Navigate to the **Security** tab of this repository
2. Click **Report a vulnerability**
3. Provide a detailed description of the issue, including steps to reproduce

We will acknowledge receipt within 48 hours and aim to provide an initial assessment within 7 days.

## Scope

The following are in scope for security reports:

- The Buttplug.io Device Provider plugin code in this repository
- Intiface Engine process management and communication
- Device discovery, approval, and command execution pathways
- Emergency stop mechanisms
- Panel schema generation and command validation

The following are out of scope:

- Vulnerabilities in the Buttplug.io library itself (report to [Nonpolynomial](https://github.com/buttplugio))
- Vulnerabilities in Intiface Engine (report to [Nonpolynomial](https://github.com/buttplugio))
- Vulnerabilities in the PlayRooms Host platform (report to the Host repository)

## Disclosure Policy

We follow coordinated disclosure. We ask that reporters give us reasonable time to address issues before public disclosure. We will credit reporters in our changelog and NOTICE.md unless they prefer to remain anonymous.
