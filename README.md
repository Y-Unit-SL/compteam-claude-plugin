# Compteam — BJJ Classes & Memberships

Official documentation: [Compteam MCP server](https://compteam.io/mcp), [Compteam API documentation](https://compteam.io/developers/api), and [developer index](https://compteam.io/developers).

Find Brazilian jiu-jitsu (BJJ) and martial arts clubs, explore class schedules, and compare membership plans and prices. Connect your Compteam account to view upcoming classes, check membership and payment status, and manage attendance for yourself or a linked child. You can also ask a club about an eligible free trial class.

## Release status

This is a preview bundle. Public discovery, protected-tool HTTP 401 challenges, the production token hook, and Claude's OAuth consent and callback were verified on 2026-11-05. A real Claude conversation successfully called `connect_compteam_account`, `list_my_subjects`, `get_account_status`, and `list_my_classes`. Attendance changes and trial inquiries were not exercised on the owner's production account. Publishing this repository does not mean Anthropic has approved or listed the plugin.

## What is included

- A remote MCP connection to `https://api.compteam.io/mcp`.
- The `compteam-training` skill for finding classes and plans, checking linked account information, and managing attendance.
- A marketplace definition for installing the bundle directly in Claude Code.

Public club details, schedules, and membership prices can be read without signing in. Personal information requires Compteam OAuth and is restricted to the signed-in person and children linked to that account. Attendance changes and trial inquiries require confirmation. Membership purchases, payments, club administration, and athlete registration are outside this bundle's capabilities.

## Install in Claude Code

Use Claude Code 2.1.318 or later; older builds may reject the bundle's current directory metadata. Run `claude update` to update an existing installation, then:

```bash
claude plugin marketplace add Y-Unit-SL/compteam-claude-plugin
claude plugin install compteam@compteam-marketplace
```

Restart your Claude Code session after installation. Use `/compteam:compteam-training` or ask a relevant question. Open `/mcp` to inspect the connection and sign in when using personal features.

The bundle configures a public OAuth client with exact callbacks for Claude Code (`http://localhost:38761/callback`) and Claude's hosted apps (`https://claude.ai/api/mcp/auth_callback`). There is no client secret in the bundle. For Claude Code, if the local callback port is occupied, close the application using it and retry authentication. A remote or headless terminal must be able to complete the callback flow on the machine running Claude Code.

For a single local development session:

```bash
claude --plugin-dir ./compteam-claude-plugin
```

## Connect in Claude web or Desktop

Until the directory listing is published, add a custom connector in Claude's connector settings:

1. Enter `https://api.compteam.io/mcp` as the server URL.
2. Choose **Sign in now**, then **Use your own OAuth client**.
3. Enter public client ID `29d102c3-57e8-4817-8a0e-eeff89e5aea0` and leave the optional client secret blank.
4. Sign in on Compteam's consent page and approve access to your own account.

This custom-connector client uses `https://claude.ai/api/mcp/auth_callback`. The bundle's embedded client also accepts that hosted callback, so it can be used when the plugin loads in Claude web or Desktop. Each user authorizes their own account; neither public client ID grants access by itself. Directory-managed connection setup is separate from publishing this repository.

## Try asking

- Find the Compteam club named ZR Team Andorra.
- Show this club's BJJ classes next week.
- What membership plans and prices does this club offer?
- Show my upcoming Compteam classes.
- Check my child's membership status.
- Help me confirm attendance for Thursday's BJJ class.
- Ask this club about a free trial.

Club discovery searches participating clubs by name. It is not a complete geographic search of every martial arts club. Class places, prices, eligibility, and account access come from live server responses.

## Privacy and data handling

The plugin sends the arguments needed for each requested operation to the Compteam MCP server. Public searches send club names, branch identifiers, and requested weeks. After OAuth connection, personal tools use the authorized account to read limited membership, payment-status, and class information for the member or a linked child. Attendance changes send the selected class and intended RSVP. A trial inquiry sends the details the user supplies in the form to Compteam for the selected club to handle.

The bundle contains no local executable, hook, analytics collector, or credential file and creates no local application data store. Its skill sends tool requests only through the declared Compteam connector. The trial form uses Cloudflare Turnstile for a security check, which may process browser or device signals. Compteam's privacy policy describes its service providers and how inquiries reach the selected club.

Claude manages its conversation history and OAuth credentials under Anthropic's controls. Compteam processes and retains server-side account and inquiry data under its [privacy policy](https://compteam.io/privacy); this bundle does not define a separate server-side retention period. Follow that policy's contact and deletion-request instructions for Compteam data, and disconnect the connector through Claude to stop future access.

Linked-child features are for authorized parents or guardians. This training-management plugin does not provide medical or fitness-safety advice. Do not put passwords, OAuth codes, payment credentials, or identity documents in the conversation.

## Documentation and support

- [Compteam MCP guide](https://compteam.io/documentation/mcp)
- Support: [info@compteam.io](mailto:info@compteam.io)
- [Privacy policy](https://compteam.io/privacy)
- [Terms of service](https://compteam.io/terms)
- Contact: [info@compteam.io](mailto:info@compteam.io)

## License and releases

The bundle is distributed under the [Unlicense](LICENSE), consistent with the source project's existing license. Compteam service access is governed by its terms of service.

See [the release guide](docs/RELEASING.md) for validation, directory submission, and updates.
