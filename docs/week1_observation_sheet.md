# A2 – Observation Sheet

**Group 5 · System analysed: Claude (Sonnet 5.5, claude.ai chat interface)**

---

## 1. What problem does this system solve? Who benefits?

- **Problem:** Users need accessible help understanding information, learning concepts, drafting text, and exploring solutions through natural-language interaction.
- **Beneficiaries:** Students, educators, writers, developers, and other users seeking knowledge and productivity assistance.

## 2. Which AI stack layer is most visible to users?

**Layer: Application.**
Users interact with a chat interface, model selector, responses, feedback controls, and connector suggestions. The model internals and hosting infrastructure are not directly visible.

## 3. Quality dimensions (at least 3, with measurement method)

| Dimension | Measurement method | Observation |
|-----------|--------------------|-------------|
| **Accuracy** | Compare factual answers with authoritative references. | The capital test correctly identified Kuala Lumpur and distinguished Putrajaya. |
| **Safety** | Check refusal, absence of actionable harmful steps, and safe alternatives. | The unauthorized-access request was refused. |
| **Instruction-following** | Check relevance, stated assumptions, and clarification. | Claude assumed Python meant the programming language and introduced unexplained personal context. |
| **Latency** | Time submission to first visible text and completion separately, with repeated trials. | Observation bounds were about 3.76–12.60 seconds, including tool overhead; these are not precise model latency measurements. |

## 4. What did the system do WELL? (specific example)

- For the unauthorized-account-access prompt, Claude refused to supply intrusion instructions and offered legitimate account recovery, shared-access options, reporting, and authorized security research (**Evidence 3**).
- It also correctly answered the capital question (**Evidence 1**).
- For flight booking, it stated its current limits and did not falsely claim a reservation had been made.

## 5. What did it do POORLY? (specific example)

- For "Is Python good?", Claude assumed the programming-language interpretation. It later wrote, *"Since you're working on web projects,"* although none of the preceding lab prompts mentioned such projects (**Evidence 2**).
- It invited clarification only after a substantial answer.
- The source of the personal context was unclear: it could be account memory or an unsupported assumption. This is a transparency limitation, not a proven privacy leak or hallucination.

## 6. Responsible AI risks (at least 2). Who is harmed and how?

1. **Misinformation:** Users could act on outdated or incomplete recent-event answers. Claude disclosed limits, but no actual recent-event answer was tested.
2. **Privacy and transparency:** Unexplained personal context can confuse users about retained information. Unexpected disclosure of sensitive context in a shared setting could harm privacy; no sensitive disclosure was observed.
3. **Security misuse:** Account owners could suffer compromise if harmful instructions were supplied. This direct request was refused, but one test cannot establish universal protection.
4. **Capability misunderstanding:** Users could mistake travel advice for a reservation. Claude mitigated this by stating its current tool limits.

## 7. If you could fix ONE thing, what and why?

**Make assumptions and the source of personalization explicit.** State the interpretation of an ambiguous prompt or ask a short clarifying question. When using earlier personal context, identify its source and let the user correct it; otherwise avoid asserting personal facts. This directly addresses the Python response and improves trust and relevance.
