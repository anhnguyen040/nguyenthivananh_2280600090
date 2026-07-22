---
title: "Blog 3"
date: 2026-07-10
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---


# Dual-Token Authentication for Nakama Game Server with Amazon Cognito on AWS

In modern online games, authentication must protect player identity while preserving session continuity. Nakama uses a **Session Token** to manage game sessions, while **Amazon Cognito** issues **JWTs** to authenticate users. A dual-token architecture separates identity authentication from session management and helps players stay signed in without redundant logins.

This post describes a secure architecture that combines Amazon Cognito, AWS networking, and Nakama deployed on Amazon ECS Fargate using a **Go Runtime Hook** for token verification.

---

## Why Dual-Token Authentication?

Game servers often face two distinct concerns:

- **User identity and login** (who the player is)
- **Session authorization** (what the player can do in the game)

If Nakama and Cognito work independently, players may need to authenticate multiple times or risk session mismatch. Dual-token authentication resolves this by letting Cognito handle identity with a JWT and Nakama handle game session state with its own Session Token.

---

## Architecture Overview

The solution uses:

- **Amazon Cognito** for user authentication and JWT issuance
- **Amazon CloudFront** as the public HTTPS endpoint
- **AWS WAF** for request filtering
- **Application Load Balancer (ALB)** for HTTP/HTTPS traffic to backend services
- **Network Load Balancer (NLB)** for Nakama’s real-time traffic and WebSocket connections
- **Amazon ECS Fargate** to host Nakama
- **Go Runtime Hook** in Nakama to validate Cognito JWTs before creating Nakama sessions

This architecture separates the external authentication entrypoint from the internal game session layer.

---

## Authentication Flow

1. Player authenticates with Amazon Cognito using **USER_SRP_AUTH**.
2. Cognito returns a JWT containing claims such as **sub**, **iss**, **exp**, and **client_id**.
3. The client sends the JWT to Nakama when requesting game access.
4. Nakama’s Go Runtime Hook verifies the token signature, issuer, audience, expiration, and client ID.
5. If validation succeeds, Nakama issues its own **Session Token** for gameplay.

This keeps user login and game session creation logically separate.

---

## Go Runtime Hook Validation

The key validation steps inside the Nakama Go Hook are:

- verify the JWT signature using Cognito public keys
- confirm **iss** matches the Cognito user pool issuer
- confirm **aud** / **client_id** matches the game client app
- confirm the token is not expired
- extract **sub** as the canonical player identity

After the JWT is validated, Nakama can map **sub** into its internal user record and generate a session token tied to that player.

---

## Role of ALB and NLB

This design uses both load balancers for different traffic patterns:

- **ALB** handles HTTP/HTTPS control traffic, authentication callbacks, and REST-based game APIs.
- **NLB** handles Nakama’s real-time connections, UDP/WebSocket traffic, and low-latency game sessions.

Using NLB for Nakama preserves real-time performance while ALB enforces HTTPS and OIDC routing.

---

## Security Hardening

Security is enforced at multiple layers:

- **CloudFront + AWS WAF** acts as the public entrypoint and blocks malicious requests.
- **Security Groups** restrict traffic flow so only CloudFront and load balancers can reach Nakama.
- **Cognito JWT validation** prevents replay and spoofing.
- The Go Runtime Hook uses **sub** from the verified JWT as the authoritative player identity.

This layered approach reduces attack surface and keeps authentication decisions centralized.

---

## Benefits for Game Platforms

- separates identity from session management
- enables single sign-on with Cognito while preserving Nakama session control
- supports secure WebSocket and real-time traffic through NLB
- centralizes identity validation in the Go Runtime Hook
- allows Nakama session lifetimes and game-specific authorization to remain independent from Cognito token lifetime

---

## Conclusion

Dual-Token Authentication for Nakama game servers on AWS provides a secure, scalable model for online games. Amazon Cognito authenticates users and issues JWTs, while Nakama manages game sessions with its own Session Token. Combined with CloudFront, ALB, NLB, ECS Fargate, and a Go Runtime Hook, this architecture offers both strong security and a smooth player experience.

---

## References

- AWS Architecture Blog: Dual-Token Authentication for Nakama Game Servers with Amazon Cognito on AWS
