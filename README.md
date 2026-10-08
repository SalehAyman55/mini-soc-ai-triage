<div align="center">

# TriageGuard

### AI-assisted SOC triage with a human in the loop

![Status](https://img.shields.io/badge/status-in%20development-orange)
![License](https://img.shields.io/badge/license-MIT-blue)
![SIEM](https://img.shields.io/badge/SIEM-Wazuh-1f6feb)
![LLM](https://img.shields.io/badge/LLM-Ollama%20(local)-black)
![Python](https://img.shields.io/badge/python-3.11%2B-3776ab)

</div>

> Graduation project for the **Cyber Security Incident Response Analyst** track.
> Educational lab project: every attack runs in an isolated environment we own.

---

## What is TriageGuard?

**TriageGuard reads security alerts so analysts don't have to read every one from scratch.**

When an attack is detected, a local AI model reads the alert, explains what is
happening, and recommends what to do. A human analyst reviews the
recommendation and makes the final call. **Nothing is blocked or changed without approval.**

### In 30 seconds

1. Someone tries to guess an SSH password thousands of times (a brute-force attack).
2. **Wazuh** detects it and raises an alert.
3. **TriageGuard** sends the alert to a local AI model and asks: *Is this real? How serious? What should we do?*
4. The AI answers: *"Real brute-force attack, high severity, block the source IP."*
5. **You** open the dashboard, see the AI's answer together with the evidence, and click **Approve**.
6. The attacker's IP is blocked, and every step is recorded in an audit log.

<p align="center">
  <img src="triageguard-flow.svg" alt="TriageGuard flow: attack, detect, AI triage, guardrails, analyst decision, response" width="100%">
</p>

## The problem it solves

SOC analysts receive far more alerts than they can investigate. Most are noise,
but the few that matter wait in the same queue.

| Without TriageGuard | With TriageGuard |
|---|---|
| The analyst reads each raw alert and gathers context by hand | The alert arrives already analyzed, with a verdict and evidence |
| Triage is slow and varies between analysts | The AI gives a consistent first-pass opinion |
| Real attacks can wait behind noisy alerts | Alerts are classified and prioritized by severity |
| No measurement of triage quality | Accuracy and time saved are measured and reported |

## Key principles

- **The AI recommends. The human decides.** The model can never execute an action.
- **Local only.** The LLM runs on our own machines through Ollama; no alert data leaves the lab.
- **Safe by design.** Strict output schema, an allow-list of actions, protected IPs that can never be blocked, and a full audit trail.
- **Measured, not assumed.** We compare several local models on the same labeled alerts.

## Example AI verdict

```json
{
  "verdict": "true_positive",
  "severity": "high",
  "attack_technique": "T1110",
  "summary": "Repeated SSH authentication failures from a single source IP.",
  "evidence": ["rule.id=100100", "data.srcip=<attacker-ip>", "failed_attempts=8 in 120s"],
  "recommended_action": "block_ip",
  "confidence": 0.9
}
```

## Evaluation

| Metric | How we measure it | Result |
|---|---|---|
| Triage accuracy | Precision, recall and F1 against a hand-labeled alert dataset | _TBD_ |
| Model comparison | Three local models, same alerts, same prompt | _TBD_ |
| Reliability | Invalid-output and hallucination rate | _TBD_ |
| Speed | Analyst time per alert, with and without AI | _TBD_ |
| SOC KPIs | MTTD and MTTR from recorded timestamps | _TBD_ |

## Tech stack

`Wazuh` · `Sysmon` · `Atomic Red Team` · `Python` · `FastAPI` · `Pydantic` · `SQLite` · `Ollama`

## Roadmap

- [ ] **Week 1**: lab build, agents connected, baseline and resource usage
- [ ] **Week 2**: attack simulation, labeled alert dataset, custom detection rules
- [ ] **Week 3**: triage pipeline, analyst dashboard, approval and response
- [ ] **Week 4**: model benchmark, MTTD/MTTR measurements, final incident report and demo

## Team

| Name | Role | GitHub |
|---|---|---|
|  | AI / Automation Engineer |  |
|  | SIEM / Infrastructure Engineer |  |
|  | Attack / Endpoint Engineer |  |
|  | Detection Engineer |  |
|  | Incident Response and Reporting |  |

## Disclaimer

For educational use in isolated lab environments only. Do not run any attack
technique against systems you do not own.

## License

Released under the [MIT License](LICENSE).
