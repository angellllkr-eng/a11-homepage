# A11 Machine Payments

A11 treats machine payments as a controlled execution boundary rather than a replacement for owner approval.

## Protocols

- **MPP (Machine Payments Protocol):** preferred interface for machine-to-machine payments because it supports multiple payment methods and one-time, session/usage, and subscription intents.
- **x402:** compatibility path for HTTP APIs and MCP tools that expose stablecoin payment challenges.

Both protocols use HTTP `402 Payment Required` as the challenge step. The service declares the amount and accepted payment method; the client presents payment credentials; the service verifies and returns the resource with payment evidence/receipt.

## A11 control model

1. **Discover** — identify the paid resource and expected charge.
2. **Budget** — evaluate the request against an explicit owner-controlled spend limit.
3. **Challenge** — accept the service's `402` challenge without bypassing policy.
4. **Authorise** — pay only when identity, route, amount, and policy checks pass.
5. **Execute** — retry the request with the payment credential.
6. **Record** — retain redacted payment and execution evidence for the Evidence Room.
7. **Handoff** — escalate when the amount, destination, protocol, or policy falls outside the configured trust zone.

## Cloudflare implementation target

Cloudflare Workers/Agents support both MPP and x402. MPP can protect Worker routes, HTTP content, and MCP tools; x402 can protect HTTP content and MCP tools through the Agents SDK.

Production activation requires account-level credentials and payment-recipient configuration. Do not place wallet private keys, Stripe secrets, or Cloudflare API tokens in repository files.

## Current state

- A11 public surface: **UPDATED** to describe MPP/x402 and the controlled payment boundary.
- A11 machine-readable discovery: **ADDED** via `/llms.txt`.
- Cloudflare account wiring: **UNVERIFIED** from this workspace because the connected Cloudflare management API is not available here.
- Production payment settlement: **NOT ACTIVATED** until the payment recipient, Cloudflare account, and eligible Stripe/payment-method access are configured and verified.

## References

- Cloudflare Agentic Payments: https://developers.cloudflare.com/agents/tools/payments/
- Cloudflare MPP: https://developers.cloudflare.com/agents/tools/payments/mpp/
- Cloudflare x402: https://developers.cloudflare.com/agents/tools/payments/x402/
