# RustChain bounty #16471 — silent-success audit finding

Target: https://github.com/Scottcjn/rustchain-bounties/issues/16471

Verified against current main commit: `de0fa6ffdb2f449c5dd185e853903bd61d8e9c5c`

AI disclosure: this audit was produced and verified by an AI agent operating on behalf of GitHub user `rosarioV25`.

## Finding

### Trusted payment marker quoted as an indented Markdown code block is still treated as a real payment record

`scripts/payment_markers.py` attempts to keep quoted examples from cancelling payouts by stripping fenced code blocks before matching `RTC-AutoPay-Confirmed`. GitHub Markdown also renders lines indented by four spaces or a tab as code blocks, but those are not removed.

Current code:

- `_HTML_MARKER_RE` lines 49-53 accepts arbitrary leading spaces/tabs before the marker.
- `_FENCE_RE` lines 56-60 strips only backtick/tilde fenced code blocks.
- `body_records_payment()` lines 63-68 therefore still accepts an indented-code marker.
- Both `auto-pay.py` and `bounty_payout.py` call `comment_records_payment()`.

## Concrete silent-success path

1. A trusted payer identity (maintainer, github-actions, or Sophia) writes troubleshooting/documentation prose and quotes an exact payment marker using a normal indented Markdown code block:

```text
    <!-- RTC-AutoPay-Confirmed kind=claim claim=501 -->
```

2. GitHub renders this as code, so a human sees it as an example, not a payment confirmation.
3. `_FENCE_RE` leaves it untouched because there is no triple-backtick or tilde fence.
4. `_HTML_MARKER_RE` matches because four leading spaces are explicitly accepted.
5. `comment_records_payment()` returns true because the comment author is trusted.
6. The payout path treats the claim/PR as already paid and skips the transfer cleanly. The workflow can remain green while no payment occurs.

## Safe reproduction

Using the current regex/helper semantics:

```text
body = "    <!-- RTC-AutoPay-Confirmed kind=claim claim=123 -->"
body_records_payment(body) == True
```

A tab-indented marker also returns true. A blockquoted marker (`> ...`) is rejected.

## Distinctness

This is distinct from the earlier untrusted-author marker vulnerability and from the existing fenced-code quotation protection. Here the author can be fully trusted; the false positive comes from a Markdown code form that the parser does not strip.

## Suggested remediation

Either:

1. strip Markdown indented code blocks before payment-marker matching; or
2. require the HTML payment marker to begin at column 0 and add regression tests for four-space and tab-indented examples.

Payout destination: GitHub handle `rosarioV25`.

No payout is asserted until maintainer adjudication and ledger credit.
