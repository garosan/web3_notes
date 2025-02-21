# Section 03. Your First Audit | PasswordStore

## Scoping: Etherscan

So in this 1st example a protocol approaches you to review their codebase and all they provide is a link to their contracts on Etherscan. Big sign that their codebase is not mature and cannot be worked with under those circumstances. The absolute minimum is to provide a Github repo where you can also see the test suite, the deployment process, etc.

Once they've provided a Github repo you can apply the:

- [Minimal Onboarding Questions](https://github.com/Cyfrin/security-and-auditing-full-course-s23/blob/main/minimal-onboarding-questions.md)

If a project gives you pushback on providing it is a big red flag and you should either offer a different consulting contract or not work with them. Remember, you're looking for signals that the team actually cares about not being hacked.

This is the repo with the audit:

https://github.com/Cyfrin/3-passwordstore-audit

From now on, on all the repos there's gonna be an audit-data branch that will contain all the final answers for the security findings.

Make sure that you're checking out the exact commit hash that you agreed to audit.

## Tools

cloc -- counts the lines of code
https://github.com/AlDanial/cloc

Once installed, you can do cloc ./src/

## The audit process with Tincho

- Download the code
- Read the documentation
- If it's a hardhat project, Tincho creates a foundry folder where he runs his own foundry tests
- Check the options for cloc, you can actually export to CSV which you can then export to Google Sheets or smt
- Check this tool too: https://github.com/Consensys/solidity-metrics
- Normally start with the smallest contracts
- Tincho wrote fuzz tests
- Timebox yourself

https://youtu.be/pUWmJ86X_do?t=10865
https://youtu.be/pUWmJ86X_do?t=11118
