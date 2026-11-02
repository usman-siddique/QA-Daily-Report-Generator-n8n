# Setup and Configuration

The included JSON is a **sanitized template**, not a turnkey production workflow. Use a test Jira workspace and fictional issues when demonstrating it. Do not connect an employer's internal data without permission.

## Import into n8n

1. In n8n, create a new workflow and choose **Import from File** (UI wording may vary by version).
2. Select `workflows/qa-daily-report-generator.template.json`.
3. Keep the imported workflow **inactive** during setup.
4. Check every node for missing credentials or required values before testing.

## Configure `Report Inputs`

- `jiraBaseUrl`: replace `https://example.atlassian.net` with the base URL of your **authorized test** Jira site; do not append a slash.
- `expectedJiraAccountId`: set the Jira account ID of the authenticated reporting user. You can retrieve it from an authorized `/rest/api/3/myself` API response.
- `reportTimezone`: enter the timezone configured for that Jira account (for example `Etc/UTC`) and set the workflow timezone to match.
- Review and customize the JQL expression. The template currently searches for issues created by `currentUser()` and status transitions made by `currentUser()` from **IN QA** to **Resolved** or **Re open** during the current day.

## Configure integrations

1. **Jira:** Open the HTTP Request nodes `Get Jira Identity`, `Search Jira Activity`, and `Get Issue Changelog`. Create or select authorized HTTP Basic Auth credentials using the appropriate Jira Cloud account email and API token. Validate access in a test environment.
2. **Gemini:** Open `Generate Summary with Gemini API`. Select an HTTP Header Auth credential. Configure the header name `x-goog-api-key` and its value as your Gemini API key, using n8n's credentials manager, **not** the JSON file. Verify API availability, model access, and your organization's data policies.
3. **Slack:** Open `Send a message`, connect the intended Slack credential, and choose a real test recipient in the `User` field. No real recipient is supplied in the template.

## Verify the workflow safely

1. Use mock Jira tickets and a Slack test channel/user you control.
2. Submit the n8n form with a sample additional activity and next-day plan.
3. Check the Jira identity and timezone validation nodes.
4. Compare the issue classification against the Jira changelog. Confirm creation, resolution, and reopening are attributable to the authenticated account on the correct local date.
5. Review the Gemini narrative against the source facts. The model must not add tickets, fabricated activities, or results.
6. Check the final report against `docs/sample-report.md`, then verify Slack delivery.
7. Only activate or publish the configured workflow if authorized and after tests pass.

## Troubleshooting

- **Account or timezone mismatch:** Check `expectedJiraAccountId`, `reportTimezone`, and the Jira credential owner's profile.
- **No matching tickets:** Check project permissions, status labels, JQL and the reporting date.
- **Gemini failure:** Verify API key, model availability, request quotas, and whether data transfer is allowed.
- **Slack delivery failure:** Reconnect Slack and select an allowed destination; confirm app scopes and workspace permissions.
- **Inconsistent or truncated results:** Review Jira pagination behavior, response envelopes, and n8n item pairing in your environment.

This package does **not** include production credentials or employer-specific configuration.
