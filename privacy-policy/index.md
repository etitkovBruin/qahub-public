# QA Hub - Privacy Policy

**Last updated:** March 13, 2026

## Overview

QA Hub is a self-hosted QA platform. The Chrome extension communicates only with the backend server that you deploy and control. The extension publisher does not operate a hosted QA Hub service for extension data.

For Chrome Web Store submission, publish this policy at a public HTTPS URL and keep the hosted copy aligned with this repository version. Placeholder URL: `https://your-domain.example/qahub/privacy-policy`

## Data Collection

The QA Hub extension does not collect, transmit, or store personal data on servers controlled by the extension publisher.

Data created through the extension, including test runs, test cases, recordings, API test results, alerts, and user accounts, is stored on your self-hosted backend and its MongoDB database.

## What the Extension Accesses

| Data | Purpose | Storage |
|---|---|---|
| Backend URL | Connects the extension to your self-hosted API | Chrome extension storage |
| Authentication tokens | Maintains the signed-in session | Chrome extension storage |
| Active tab URL | Supports recorder navigation and side panel workflows | Used in-memory and local extension state |
| Page DOM while recording | Captures test actions selected by the user | Sent to your backend as test steps |
| Jira page content | Extracts ticket context for AI Test Creation after user action and permission grant | Processed locally, then sent to your backend |

## Permissions Explained

| Permission | Why it's needed |
|---|---|
| `storage` | Save backend configuration, tokens, and local UI state |
| `tabs` | Read the active tab URL for recorder and review flows |
| `sidePanel` | Display the primary QA Hub UI |
| `scripting` | Inject recorder, playback, and Jira integration scripts during user-initiated actions |
| `activeTab` | Access the current page when the user starts recording, playback, or Jira refresh |
| `alarms` | Poll for alert notifications on a schedule |
| `downloads` (optional) | Download trace files only when requested by the user |
| `notifications` (optional) | Show desktop notifications for alerts only when enabled by the user |
| `declarativeNetRequestWithHostAccess` | Apply network override rules configured by the user on sites that the user has granted to QA Hub |
| Host access (optional) | Access only the specific sites the user chooses to test. Recorder and playback request exact target sites at runtime, and the user can review or remove granted sites from Profile > Site Access. Atlassian Jira access is requested separately for AI Test Creation |

Optional permissions are requested only when the related feature is used.

## Third-Party Services

The extension itself does not contact third-party services outside the backend you configure. Your self-hosted backend may optionally integrate with:

- TeamCity
- Jira
- GitHub
- AI endpoints that you configure

Those integrations are controlled entirely by your backend configuration.

## Data Retention

All QA Hub data is stored on infrastructure that you manage. Data retention is controlled by your backend deployment and MongoDB policies.

## Changes to This Policy

Updates to this policy will be published in the repository and reflected in the Chrome Web Store listing.

## Contact

For privacy questions, use your public support address in the published copy of this policy. The placeholder used in repository drafts is `support@example.com`.
