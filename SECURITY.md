# Security Policy

## Supported Versions

This project distributes only workflow JSON and docs. There is no runtime code here. Security concerns mostly relate to protecting your API credentials and webhook endpoints when deploying in n8n.

## Reporting a Vulnerability

If you discover a security issue, please open a private disclosure by emailing the maintainer or opening a GitHub Security Advisory. Do not include secrets or tokens.

## Keep Secrets Out of Git

- Do not commit API keys, access tokens, phone numbers, or webhook IDs.
- Use n8n Credentials to store WhatsApp and Gemini secrets.
- Replace any concrete IDs in exports with placeholders before publishing.

## Hardening Checklist

- Use HTTPS for all webhooks (required by WhatsApp).
- Set strong, random webhook verification tokens.
- Restrict Google Gemini API key to required APIs.
- Rotate tokens regularly and monitor usage.
- Validate payloads and handle media size limits.

## Safe Exports

When exporting a workflow for sharing:

- Remove credential blocks and webhook IDs.
- Set `active` to `false`.
- Replace real identifiers with placeholders (e.g., YOUR_PHONE_NUMBER_ID).

