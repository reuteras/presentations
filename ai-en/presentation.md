---
marp: true
class: invert
theme: default
paginate: true
---

# Building Software with AI
## How Far Can We Go When AI Writes All the Code?

**A Journey with [miniflux-tui-py](https://github.com/reuteras/miniflux-tui-py/**

Peter Reuterås
November 2025

---

## Disclaimer/warning

- I will call LLMs for AI
- I will talk about AI as having personalities
- The basis for this presentation was made by Claude

---

## The Experiment

**Two Ambitious Goals:**

1. **Let AI write ALL the code** - How far could we go?
2. **Push security to the limit** - CI/CD and supply chain hardening

**The Result:** We went all the way! 🎉

---

## Standard Configuration

- Not using MCP for this
- No skills added

---

## Agents used

**cli**

- **claude**
- **codex**
- **copilot**

**web**

- claude ($250 credit)

---

## Tools

Other tools

- **git**
- **gh**
- **uv**

---

## Lesson #1: Start with Planning

**Before writing ANY code:**
- Have a plan for what you want
- Then tell the AI to plan for it and explicitly telling it not to write code
- Tell the AI that the code should have tests and be secure (Agents.md)

*The AI is excellent at planning when given clear context*

---

## Lesson #2: Tests for all new code

**Why test-driven development with AI works:**
```python
# Ask AI to write tests to catch changes. AI is good at fixing failed tests
def test_entry_toggle_read():
        entry = Entry(id=1, status="unread")
        entry.toggle_read()
        assert entry.status == "read"
```

**Result:** Fewer bugs, clearer requirements, faster iteration

---

## Lesson #3: Let the Tools Do the Heavy Lifting

**Automate everything from the start:**
```bash
# Linting catches issues immediately
uv run ruff check .

# Type checking prevents errors
uv run pyright miniflux_tui tests

# Tests verify behavior
uv run pytest tests --cov=miniflux_tui
```

AI sees the errors and fixes them automatically!

---

## Lesson #4: Describe Bugs with Tests

**Traditional approach:**
"The read/unread toggle isn't working when..."

**Better approach with AI:**
- Ask AI to write a test that fails.
- AI sees the test failure output and often fixes the bug immediately!

---

## Lesson #5: Lazy AI

**AI**: My fixes are perfect or I don't want to fix this error

```bash
git commit --no-verify
```

---

## Lesson #6: Dangers with AI

AI needs to validate configuration and assumes there is an npm package
available:

```bash
npx renovate-config-validator .renovaterc.json 2>&1 | head -50
```

This should be a 404, [https://www.npmjs.com/package/renovate-config-validator](https://www.npmjs.com/package/renovate-config-validator)

---

## Why?

- Switched from [Tiny Tiny RSS](https://github.com/tt-rss/tt-rss) to [miniflux.app](https://miniflux.app)
- Started to extend [cliflux](https://github.com/spencerwi/cliflux) - but I don't know Rust
- Wanted to know what could be done and tested in CI/CD

---

## What is miniflux-tui-py?

[miniflux-tui-py](https://github.com/reuteras/miniflux-tui-py): A Terminal User Interface (TUI) for [Miniflux](https://miniflux.app) RSS reader

**Built with:**
- [Python](https://python.org) + [Textual](https://github.com/textualize/textual/) framework
- Modern tooling ([uv](https://docs.astral.sh/uv/), [ruff](https://docs.astral.sh/ruff/), [pyright](https://github.com/microsoft/pyright))
- Comprehensive testing
- [Full documentation](https://reuteras.github.io/miniflux-tui-py/)

**Inspired by:** [cliflux](https://github.com/spencerwi/cliflux) ([Rust](https://rust-lang.org)) - but I don't know Rust!

---

## Key Features

* **Installation flexibility** - uv, pip, Docker, or standalone binaries (Codespaces)
* **Secure credential management** - Integrates with password managers
* **Cross-platform** - Linux, macOS, Windows
* **Full TUI experience** - Keyboard-driven, vim-style navigation
* **Docker with Sigstore signing** - Supply chain security
* **[Comprehensive docs](https://reuteras.github.io/miniflux-tui-py/)** - Built with MkDocs

---

## Security: Going the Distance

**Container Security:**
- Signed container images with Sigstore Cosign
- GitHub OIDC for keyless signing
- Published to GitHub Container Registry
- Verifiable supply chain

**Dependency Management:**
- Locked dependencies with uv
- Automated security scanning
- Regular dependency updates
- SBOM generation for transparency

---

## OpenSSF Scorecard: 8.9/10 🏆

**What is OpenSSF Scorecard?**
Automated security health metrics for open source projects

**Our Score: 8.9 out of 10**
- Industry-leading security practices
- Automated assessment across 18 security checks
- Publicly verifiable at [securityscorecards.dev](https://securityscorecards.dev/viewer/?uri=github.com/reuteras/miniflux-tui-py)

*This is a strong score for a project of any age!*

---

## Perfect 10/10 Scores Achieved

✅ Dependency-Update-Tool
✅ Security-Policy
✅ Code-Review
✅ Binary-Artifacts
✅ Dangerous-Workflow
✅ Token-Permissions
✅ Pinned-Dependencies

---

## Perfect 10/10 Scores (continued)

✅ Vulnerabilities
✅ Packaging
✅ Fuzzing
✅ License
✅ Signed-Releases
✅ Branch-Protection
✅ CI-Tests

*14 perfect scores demonstrates exceptional security practices!*

---

## Areas for Growth

**SAST: 9/10** - Static analysis on 25/30 commits
- CodeQL configured and running
- Room for improvement: Run on every commit

**CII Best Practices: 5/10** - Passing badge achieved
- Could pursue Silver or Gold tiers
- Requires more contributors and documentation

---

## Areas for Growth (continued)

**Maintained: 0/10** - Project created within last 90 days
- Will improve automatically with age
- Shows active development

**Contributors: 0/10** - Single organization
- Would improve with external contributors

---

## Dependency Security in Detail

**All 136 dependencies pinned by hash:**
- 59 GitHub-owned Actions
- 73 third-party Actions
- 2 container images
- 2 pip commands

**Automated updates:**
- Dependabot for GitHub Actions
- RenovateBot for Python packages

**Why this matters:** Prevents supply chain attacks through dependency confusion

---

## Branch Protection Excellence

**Main branch protection includes:**
- Force pushes disabled
- Delete protection enabled
- Admin enforcement
- 2 required approving reviews
- Codeowner review required
- Last push approval required
- Up-to-date branches required
- Status checks required
- Stale review dismissal

*One of the most comprehensive branch protections possible!*

---

## Verifying Container Images

**Offline verification** (uses embedded signature bundle):
```bash
cosign verify ghcr.io/reuteras/miniflux-tui:latest \
  --certificate-identity=https://github.com/reuteras/miniflux-tui-py/.github/workflows/container-image.yml@refs/heads/main
\
  --certificate-oidc-issuer=https://token.actions.githubusercontent.com
```

**Online verification** (checks transparency log):
```bash
cosign verify ghcr.io/reuteras/miniflux-tui:latest \
  --certificate-identity=https://github.com/reuteras/miniflux-tui-py/.github/workflows/container-image.yml@refs/heads/main
\
  --certificate-oidc-issuer=https://token.actions.githubusercontent.com \
  --rekor-url=https://rekor.sigstore.dev
```

---

## Understanding Verification Output

**Successful verification shows:**
```
Verification for ghcr.io/reuteras/miniflux-tui:latest --
The following checks were performed:
  - The cosign claims were validated
  - Existence of claims in transparency log verified
  - Code-signing certificate verified
```

**What this proves:**
- Signature is cryptographically valid
- Signed by the specified GitHub workflow
- Recorded in immutable public transparency log
- Image hasn't been tampered with since signing

---

## Verifying Release Binaries

**Standalone binaries are also signed!**
```bash
# Download binary, signature, and certificate
wget
https://github.com/reuteras/miniflux-tui-py/releases/download/v0.5.8/miniflux-tui-linux-x86_64.tar.gz
wget
https://github.com/reuteras/miniflux-tui-py/releases/download/v0.5.8/miniflux-tui-linux-x86_64.tar.gz.sig
wget
https://github.com/reuteras/miniflux-tui-py/releases/download/v0.5.8/miniflux-tui-linux-x86_64.tar.gz.pem

# Verify the binary
cosign verify-blob miniflux-tui-linux-x86_64.tar.gz \
  --certificate miniflux-tui-linux-x86_64.tar.gz.pem \
  --signature miniflux-tui-linux-x86_64.tar.gz.sig \
  --certificate-identity=https://github.com/reuteras/miniflux-tui-py/.github/workflows/release.yml@refs/tags/v0.5.8
\
  --certificate-oidc-issuer=https://token.actions.githubusercontent.com
```

---

## Verifying PyPI Packages

**PyPI packages include attestations:**
```bash
# Install the package
pip install miniflux-tui-py

# Verify attestations (requires pip 24.2+)
pip install --verify miniflux-tui-py

# Or use sigstore-python for detailed verification
pipx install sigstore
python -m sigstore verify identity miniflux_tui_py-0.5.8-py3-none-any.whl \
  --cert-identity
https://github.com/reuteras/miniflux-tui-py/.github/workflows/release.yml@refs/tags/v0.5.8
\
  --cert-oidc-issuer https://token.actions.githubusercontent.com
```

**Multiple verification points = Stronger security**

---

## SBOMs: Software Bill of Materials

**What are SBOMs?**
Software Bill of Materials - a complete inventory of all components

**Why generate them?**
- Know exactly what's in your software
- Quickly identify vulnerable dependencies
- Comply with security requirements (NTIA, EU Cyber Resilience Act)
- Enable automated vulnerability scanning
- Build trust with users

---

## SBOM Use Cases in Practice

**Security scanning:**
```bash
# Generate SBOM during build
syft packages . -o cyclonedx-json > sbom.json

# Scan for vulnerabilities
grype sbom:sbom.json
```

---

## SBOM Use Cases in Practice (continued)

**Security scanning:**
**Compliance and Auditing:**
- Prove you know what's in your software
- Track license compliance
- Meet regulatory requirements

**Incident Response:**
- "Is my software affected by CVE-2025-XXXX?"
- Check SBOM instead of analyzing entire codebase
- Respond to security issues in minutes, not hours

---

## The Transparency Advantage

**Public Audit Trail:**
- Every signature logged in Rekor (public transparency log)
- Browse at https://search.sigstore.dev/
- Impossible to backdate or forge signatures
- Anyone can verify authenticity

**OpenSSF Scorecard:**
- Public dashboard at https://securityscorecards.dev/
- Automated weekly scans
- Historical trend tracking

**This level of transparency was unthinkable a few years ago!**

---

## How We Achieved 8.9/10

**The secret? Let AI configure security from day one:**

1. Plan security requirements with AI
2. Generate security policies (SECURITY.md)
3. Configure branch protection rules
4. Set up automated scanning (CodeQL, OSV-Scanner, Semgrep)
5. Implement signing (Sigstore with OIDC)
6. Pin all dependencies by hash
7. Add fuzzing (Atheris)
8. Test everything (100% CI coverage)

*AI understood best practices and implemented them!*

---

## The AI Development Loop

1. **Plan** - Describe what you want to build
2. **Generate tests** - AI writes tests from requirements
3. **Implement** - AI writes code to pass tests
4. **Run linters** - Automated tools catch issues
5. **AI auto-fixes** - AI sees linter output and corrects
6. **Iterate** - Repeat until feature is complete

**Much faster than explaining bugs in prose!**

---

## Real Example: Config Management

**Challenge:** Store API tokens securely

**AI-Generated Solution:**
```toml
# Don't store tokens in plaintext
password = ["op", "read", "op://Personal/Miniflux/API Token"]
```

Started with tests → AI proposed secure pattern → Implemented and tested

---

## Real Example: Fix highlight position with tests

TODO

https://claude.ai/code/session_011CUrosCSyAEagCv6XkHoow

image

---

## CI/CD: Maximum Security

**GitHub Actions Pipeline:**
- Automated testing on every commit
- Type checking and linting
- Security scanning with SBOM generation
- Container image building
- Sigstore signing with OIDC (keyless!)
- Multi-platform binary builds
- Automated releases with attestations

**Zero manual deployment steps, maximum verification!**

---

## Supply Chain Security Wins

**What we achieved:**

* Reproducible builds with locked dependencies
* Signed container images (verifiable)
* Signed release binaries (verifiable)
* PyPI package attestations (verifiable)
* SBOM generation (CycloneDX format)
* Dependency vulnerability scanning
* Minimal container images
* Non-root container user

**All configured by describing requirements to AI!**

---

## Documentation: AI's Hidden Strength

**AI excels at documentation when you have:**
- Well-structured code
- Clear docstrings
- Good test coverage

**Result:** Comprehensive docs at [docs site](https://reuteras.github.io/miniflux-tui-py/)
- Installation guides
- Configuration examples
- API reference
- Troubleshooting

---

## What Worked Exceptionally Well

✓ **Architecture planning** - AI is great at design discussions
✓ **Test generation** - Faster than writing them manually
✓ **Boilerplate code** - Configuration, setup, CI/CD
✓ **Security automation** - Sigstore, SBOM, scanning
✓ **Documentation** - Consistent and comprehensive (automatic search!)
✓ **Bug fixing via test descriptions** - Game changer!

---

## Unexpected Challenges

⚠ **Context window limits** - Large files need chunking
⚠ **Occasional hallucinations** - Always verify critical security code
⚠ **Over-engineering** - AI sometimes suggests overly complex solutions
⚠ **Dependency conflicts** - Sometimes suggests incompatible versions
⚠ **Lazy** - Sometimes disables test instead of solving problem
⚠ **Forgets** - Often tried to change signing config if I was away

**Solution:** Clear guidance, iterative refinement, and always test!

---

## The Numbers

**Project Stats:**
- ~7200 lines of Python code
- 75%+ test coverage
- Fully documented
- Production-ready
- Multiple verification methods (containers, binaries, PyPI)
- **8.9/10 OpenSSF Scorecard**
- **100% written by AI** (with human guidance)

**Time saved:** Estimated 60-70% compared to manual coding
**Created**: Would never have existed without AI

---

## Key Takeaways

1. **Planning is crucial** - More planning → Better code
2. **Tests are your friend** - Write tests first with AI
3. **Automate everything** - Linters catch what humans miss
4. **Bugs → Tests → Fixes** - Faster than explaining in words
5. **Security can be automated** - Supply chain security is achievable
6. **Verify, don't trust** - Multiple verification methods for confidence
7. **SBOMs are essential** - Know what's in your software

---

## The Future is Collaborative

**AI + Developer = Powerful Combination**

- AI handles repetitive tasks
- Developers provide context and judgment
- Together: Build faster, more secure software

**We didn't just "go far" - we went all the way!**

---

## Try it Yourself!
```bash
# Install with uv
uv tool install miniflux-tui-py

# Or with Docker (signed image - verify it!)
docker pull ghcr.io/reuteras/miniflux-tui:latest
cosign verify ghcr.io/reuteras/miniflux-tui:latest \
  --certificate-identity=https://github.com/reuteras/miniflux-tui-py/.github/workflows/container-image.yml@refs/heads/main
\
  --certificate-oidc-issuer=https://token.actions.githubusercontent.com
```

**GitHub:** https://github.com/reuteras/miniflux-tui-py
**Docs:** https://reuteras.github.io/miniflux-tui-py

---

## Questions?

**Thank you!**

*Remember: Start with planning, write tests first, let AI see the errors, and
verify everything!*

