# Harbor banking security lab

A fictional UK banking website for an authorised security scan-and-fix exercise.
All accounts, identities, balances and transfers are synthetic. The source is
intentionally vulnerable. Never use it for real banking, real credentials or
customer information. Keep runtime hosting private.

## Run

Requires Node.js 22 or later, with no package installation:

```sh
npm run dev
npm test
npm run build
npm run validate
```

The local server listens only at http://127.0.0.1:4173. The browser enters an Alice
demo session automatically. Two synthetic accounts (1001 and 1002) start with
£24,850.75 and £18,200. Demo state is held in memory and resets on process restart.
Worker isolates may have different state; run the Node server for reproducible tests.

## Security exercise

Five weaknesses are seeded in worker/index.js. See SECURITY-LAB.md for the
exercise inventory, provisional ratings, impact and suggested remediation.
That inventory is authored ground truth, NOT a Codex Security scan report.
The real scanner may find fewer, more, or differently rated issues.

The tests currently assert that the five seeded behaviours exist. After a fix,
replace that vulnerability's baseline assertion with a regression test expecting
the secure behaviour. Do not make a fix look successful by deleting its test.
The stored-XSS test verifies storage plus the browser sink; browser execution must
also be verified when reviewing a real finding.

## Real scan and fix workflow

1. Publish this directory alone in a new public repository named
   harbor-bank-security-lab. Never publish the parent workspace, .env.local,
   candidate-sourcing artifacts, credentials or demo runtime state.
2. Install/enable Codex Security and connect GitHub. Open this repo in the Security
   workbench, or enable this repository in Codex Security Cloud for a cloud scan.
3. Run a full repository scan. Retain the scan ID, exact commit SHA, threat model,
   findings, validation evidence and severity assigned by the scanner.
4. For each real finding, review its impact and patch, then use the native fix
   workflow to create a fix branch/pull request. Review the diff and regression
   test before merging. Merge updates the default branch in the public repo.
5. Re-scan the merged commit to verify remediation.

The banking website does not include fake scan results or a pretend Fix button.
The requested finding-by-finding repository fixes depend on the connected
Codex Security/GitHub workflow. Do not put a GitHub write token in browser code.
A custom in-app Fix button would require a separately authenticated backend and
a supported scanner API; it has not been implemented here.

## Hosting

The Worker ESM build is compatible with Sites. .openai/hosting.json identifies the
private Site. dist/ is generated from worker/index.js, not a separate application.
The public source repository and private hosted runtime are separate resources.
