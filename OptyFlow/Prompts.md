Review the infrastructural approach for exposing the WebSocket service `cloudws` to clients.

Current architecture to evaluate:
- clients + tmc -> Azure Application Gateway -> cloudws virtual machines

Focus your review on these sources first:
- infra/terraform/main.tf
- docs/adr/0001-cloudws-distributed-proxy-routing.md
- relevant code files involved in cloudws listener setup, routing, connection lifecycle, auth, session affinity, proxy-awareness, health checks, and deployment assumptions

What I want from you:
1. Infer the current end-to-end exposure model for `cloudws`.
2. Evaluate whether Azure Application Gateway is a good fit for this WebSocket workload.
3. Compare the current approach against plausible alternatives, including at least:
   - exposing cloudws VMs directly behind an L3/L4 load balancer
   - any Azure-native alternatives that are materially better fits for WebSocket traffic
4. Assess tradeoffs for each option in terms of:
   - correctness and protocol compatibility for WebSockets
   - scaling characteristics
   - connection persistence / long-lived connection behavior
   - health checks and failover
   - security posture and DDoS/WAF implications
   - observability
   - cost and Terraform/manageability
5. Recommend the best target architecture.
6. Ignore the migration path. Imagine we are setting the infrastructure up from scratch.

Important instructions:
- This is an architecture/infrastructure review, not an implementation task.
- Do not make code changes.
- Be concrete and opinionated.
- Clearly distinguish observed facts from inferences.
- If a key detail is missing, say exactly what is missing and how it affects confidence.
- Prefer depth over breadth; stay focused on WebSocket exposure and routing.

Output format:
- Executive summary
- Current architecture as implemented
- Suitability of Azure Application Gateway for cloudws
- Comparison with alternatives
- Risks in current design
- Recommended architecture
- Open questions / missing evidence
