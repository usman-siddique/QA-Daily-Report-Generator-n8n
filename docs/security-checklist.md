# Security and Publication Checklist

This template was adapted from an internal-style workflow export. **Do not publish the original export.** Even a sanitized derivative may require employer or client approval if the workflow's architecture, process, or code belongs to them.

Before making a repository public:

- [ ] Confirm you own the workflow and have explicit permission to disclose its implementation.
- [ ] Verify no client names, internal Jira domains, user IDs, Slack recipient IDs, webhook IDs, credential IDs, or instance metadata remain.
- [ ] Verify no secrets appear in code, node parameters, URLs, screenshots, `.env` files, test data, or **Git history**.
- [ ] Use fictional issue keys and summaries in all examples and screenshots.
- [ ] Confirm that using the external Gemini API with Jira-derived data is authorized. Remove or mask confidential content when necessary.
- [ ] Ensure the workflow is not connected to live employer systems as part of a public demonstration.
- [ ] Keep the repository **private** until all checks and required approvals are completed.

**Important:** Git's `.gitignore` does not protect information that is manually uploaded through a browser or already committed to a repository. If a secret was exposed, revoke/rotate it and follow your organization's incident response procedure.
