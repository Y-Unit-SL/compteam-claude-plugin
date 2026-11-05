# Release the Claude bundle

## Validate a candidate

Run from the repository root:

```bash
npx --yes --package @anthropic-ai/claude-code@2.1.318 claude plugin validate --strict .claude-plugin/plugin.json
npx --yes --package @anthropic-ai/claude-code@2.1.318 claude plugin validate --strict .claude-plugin/marketplace.json
```

Install the candidate from a local marketplace or load it with `claude --plugin-dir .`. Verify public club search, schedules, prices, and branch details. Complete Compteam OAuth, then verify `connect_compteam_account`, `list_my_subjects`, `list_my_classes`, and `get_account_status` with an authorized test account. Test RSVP and trial inquiries only with a suitable test account and explicit confirmation of those changes.

The commands pin the validator used for this release; older Claude Code versions may reject newer directory metadata fields. The CLI validates structure; it does not establish that OAuth or the live tools work. Check the actual outcomes on each supported Claude surface. Do not publish a claim of completed acceptance testing based only on the validator.

## Production authentication setup

The initial public registrations, created and read back on 2026-11-05, are:

| Client | Public client ID | Exact callback |
| --- | --- | --- |
| Claude web and Desktop custom connector | `29d102c3-57e8-4817-8a0e-eeff89e5aea0` | `https://claude.ai/api/mcp/auth_callback` |
| Claude plugin bundle | `b217b550-6efe-4743-b40b-495c6c9e07d4` | `http://localhost:38761/callback` and `https://claude.ai/api/mcp/auth_callback` |

Both are public clients with token endpoint authentication `none`, no secret, and an active `marketplace_safe` Compteam client registration. The resource audience is `https://api.compteam.io/mcp`. Each user still needs to complete their own authorization-code flow with PKCE.

The bundle client is named **Compteam for Claude plugin**. Its hosted callback was added after a real Claude web test showed that the app uses the embedded bundle client ID. Registering only the loopback callback causes `invalid redirect_uri` in Claude web even when a separate custom connector has its own valid hosted client.

Before directory launch:

- The MCP HTTP 401 challenge fix was merged in [API PR #735](https://github.com/Y-Unit-SL/bjj-comp-saas/pull/735) and deployed on Render on 2026-11-05. Production initialization, public tool listing/search, protected HTTP challenges and resource metadata passed their checks. Keep anonymous public discovery available in future releases.
- The Supabase Custom Access Token hook (`public.compteam_mcp_access_token_hook`) is enabled in production as of 2026-11-05, with the selected `public` schema and function read back in the dashboard. A real Claude web OAuth flow completed consent and returned **Authentication complete** after the bundle client's callback registration was corrected.
- Sign-in, consent, token exchange, and four protected read tools were verified in a real Claude conversation on 2026-11-05: `connect_compteam_account`, `list_my_subjects`, `get_account_status`, and `list_my_classes`. The activity trace showed only these four read tools. Refresh and write workflows still need their own acceptance checks with a dedicated reviewer account; an authorization redirect alone does not establish that these work.
- Complete the supported workflows with the reviewer account and obtain the required MCP App screenshots if the trial form is included in the connector listing.
- Update the README's release status to reflect the actual validated behavior.

The wider live Registry checker reported that the public MCP documentation page lacks its expected `io.compteam/compteam` identifier. This documentation check remains separate from the passing production API checks.

Do not put service-role keys, user tokens, reviewer passwords, or confidential OAuth client secrets in this public repository. Enter reviewer credentials and any Anthropic-held confidential client credentials only in the developer portal.

## Submit to Claude's directory

Use the Claude organization that should own the listing. Connect a GitHub account with push access to this repository.

At [the developer portal](https://claude.ai/directory/manage), select **Submit new → Plugin bundle** and enter:

| Field | Value |
| --- | --- |
| Repository | `Y-Unit-SL/compteam-claude-plugin` |
| Plugin path | Leave empty; the plugin is at the repository root |
| Tracked branch | `main` |

Run portal validation, fix blocking findings, and validate again after any push. The listing comes from `.claude-plugin/plugin.json` and `README.md`. Describe data handling accurately using the README and Compteam privacy policy, review the compliance acknowledgements, and submit for review. Passing validation does not publish the plugin: request **Publish** when the version is eligible, following the review setting shown in the portal.

Also submit `https://api.compteam.io/mcp` as an **MCP connector** from the same Claude organization and pair it with this bundle. The server URL must stay identical in both configurations to avoid duplicate tools. In the live portal checked on 2026-11-05, select **Required when the server asks → Add OAuth sign-in → Anthropic-held client credentials → None (public client)** and enter the hosted client ID `29d102c3-57e8-4817-8a0e-eeff89e5aea0`. The portal explicitly supports this shared public client without a secret; a confidential client is not required for this configuration. This corrects the earlier setup note based on the written authentication guide.

## Release an update

1. Increase `version` in `.claude-plugin/plugin.json` and update the documentation for the final behavior.
2. Validate, test the changed workflows, and commit the candidate.
3. Push to `main`. Claude checks the tracked branch through the configured webhook or scheduled checks.
4. Inspect the portal's validation and scan results; use **Check for new commits** to refresh immediately.
5. Publish the passing version according to the portal's review settings. Users keep the previous published version until the update goes live.

GitHub tags and Releases are useful source snapshots; they do not submit or approve a Claude directory release. Use a prerelease label while account-linking acceptance remains outstanding.

## Official references

- [Plugin submission](https://claude.com/docs/plugins/submit)
- [Pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist)
- [Connector authentication](https://claude.com/docs/connectors/building/authentication)
- [Plugin and connector listings](https://claude.com/docs/directory/publish)
