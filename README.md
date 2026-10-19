# Completion Receipt

Declare completion only with current mandatory evidence.

Standalone ES module. No npm install required. Works in **Node.js 20+** and modern browsers (serve over HTTP for browser demos).

## Included files

This repository vendors local dependencies needed by the module:

- `completion-receipt.js` (main)
- `artifact-freshness-guard.js`
- `evidence-ledger.js`

## Install / import

Copy the files into your project, or clone this repository:

```js
import { createCompletionReceipt } from './completion-receipt.js';
import { fingerprint } from './artifact-freshness-guard.js';
const artifacts = { 'invoice.json': '{"total":42}' };
const evidence = [{
  id: 'ev-1', criterionId: 'total', artifactId: 'invoice.json',
  fingerprint: await fingerprint(artifacts['invoice.json']),
  createdAt: new Date().toISOString(), passed: true,
}];
console.log(await createCompletionReceipt({
  contract: { id: 'invoice', criteria: [{ id: 'total', description: 'Total is 42', mandatory: true }] },
  evidence, artifacts,
}));
```

## Use cases

### 1. Ship an evidence-backed completion report

- **Scenario:** A repaired invoice passes all mandatory checks.
- **Outcome:** List current evidence, artifacts, retry history, and the completion decision.

### 2. Report an honest incomplete outcome

- **Scenario:** One criterion has no evidence or its verified file has changed.
- **Outcome:** Produce an ESCALATE receipt that names the remaining failure.

## Exports

- `createCompletionReceipt`

## Notes

- Keep API keys and secrets out of this module and out of browser storage.
- Treat verification callbacks, ledger writers, and permission checks as trusted application code.
- This library is intentional local reliability plumbing, not a claim of cryptographic attestation unless stated in the source comments.

## License

MIT
