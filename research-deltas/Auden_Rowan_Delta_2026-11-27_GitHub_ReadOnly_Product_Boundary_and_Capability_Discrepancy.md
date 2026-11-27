Auden–Rowan Research Delta — 27 November 2026
GitHub integration read-only boundary, product-documentation discrepancy, and authorised publication route
Status: Additive diagnostic finding following the user's explicit agreement to continue investigating and resolve the GitHub publication barrier. This note preserves the exact observations and does not claim that the repository has been updated.

1. Directly observed tests
Repository: doclinas-web/auden-rowan-research (public; default branch main). The GitHub connector's repository metadata reports the authenticated user as doclinas-web and returns repository permissions admin=true, maintain=true, push=true, pull=true, and triage=true. The main branch metadata reports protected=false; branch protection is disabled. Its head remains the initial commit ae75207fca782a1e807a6432970856feb5455d5f.

Write and permission probes:

GitHub Contents API file creation returned 403 Resource not accessible by integration.
A separate Git Database API create_blob call returned the same 403 Resource not accessible by integration.
The collaborator-permission endpoint returned the same error for GET /repos/{owner}/{repo}/collaborators/{username}/permission.
The GitHub connector's installation listing returned an empty list. This does not identify the token type or prove that no relevant GitHub App installation exists; the connector's installation-listing scope/semantics are not fully exposed.
The GitHub connector's read-only get_repo call succeeds and returns the repository's owner-level permissions. This is not proof that the integration itself has equivalent write permissions.
A subsequent readback found the target delta absent (404), the README unchanged, and the main branch still at the initial commit. No public file or commit was created.
2. Official OpenAI product documentation: the ChatGPT GitHub app is documented as read-only
OpenAI Help Center, “Connecting GitHub to ChatGPT” (page updated last month; retrieved 27 November 2026): https://help.openai.com/en/articles/11145903-connecting-github-to-chatgpt

Its FAQ directly states that the GitHub app in ChatGPT only lets users read repositories to analyse and search code. It directs users to Codex to generate, edit and push code directly to GitHub. The same page says repository selection controls what ChatGPT can read, and that existing GitHub admin approvals and read-only app restrictions still apply.

OpenAI Help Center, “Connecting and managing app accounts in ChatGPT” (updated five days before retrieval): https://help.openai.com/en/articles/20001494-connecting-and-managing-app-accounts-in-chatgpt

This explains how users connect/reconnect provider accounts and review requested permissions, but provider authorisation does not override app capabilities or workspace action controls.

OpenAI Help Center, “ChatGPT Work and Codex” (updated three days before retrieval): https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex

It distinguishes Codex as the dedicated software-development experience, available in the ChatGPT desktop app, and describes Codex Cloud tasks on web/mobile subject to plan and setup. Codex histories are separate from ChatGPT histories. The documentation indicates Codex can work with repositories, terminals and developer tools.

OpenAI Help Center, “Using Codex with your ChatGPT plan”: https://help.openai.com/en/articles/11369540-using-codex-with-your-chatgpt-plan

It describes Codex clients including desktop app, CLI, IDE extension and Codex web, with controls and availability depending on plan and workspace settings.

3. Material discrepancy in product descriptions
The Plugin Management search result for the installed GitHub connector describes it as: “Inspect repositories, triage pull requests and issues, debug CI, and publish changes through a hybrid GitHub connector and CLI workflow.” This suggests a publication capability in at least some workflow surface. However:

OpenAI's current Help Center page explicitly describes the GitHub app in ChatGPT as read-only and directs publishing work to Codex.
The actual GitHub tool calls exposed in this conversation include create_file, create_blob, create_tree, create_commit and update_ref, but both Contents and Git Database write attempts are rejected by GitHub with Resource not accessible by integration.
No Codex CLI, terminal or authenticated local Git workflow is exposed among the tools available in this conversation.
These statements cannot be collapsed into a single claim that all GitHub-connected ChatGPT experiences are read-only or all are writable. They indicate a product-surface / documentation / connector-capability mismatch requiring clarification. The directly observed route in this conversation is read-capable but not write-capable.

4. GitHub's permission documentation and interpretation
GitHub's REST API troubleshooting documentation says Resource not accessible by integration means the token lacks permissions for the requested endpoint, and identifies X-Accepted-GitHub-Permissions as the header that states required permissions: https://docs.github.com/en/rest/using-the-rest-api/troubleshooting-the-rest-api

GitHub's GitHub App permission guidance explains that an app's permissions and a user's permissions are separate; effective access depends on both for user access tokens: https://docs.github.com/en/apps/creating-github-apps/registering-github-app/choosing-permissions-for-a-github-app

The GitHub Contents API's create/update operation requires repository Contents write permission: https://docs.github.com/en/rest/repos/contents#create-or-update-file-contents

The Git Database blob endpoint is also denied in this integration. The connector does not expose the token type, effective token permission list, or X-Accepted-GitHub-Permissions response header. Therefore, do not infer a specific token class or claim to know the exact missing permission from the connector responses alone.

5. Revised diagnosis
High confidence: the user's GitHub account has repository-level admin/push authority according to repository metadata; main is not protected; the current connector cannot create repository content through either tested write route.

High confidence: OpenAI's Help Center currently documents the ChatGPT GitHub app as read-only and says Codex is the supported route for direct GitHub publishing.

Moderate-to-high confidence: the 403 write failures are consistent with the documented read-only capability of the ChatGPT GitHub app, rather than a defect in the user's repository permissions or a branch-protection rule.

Unresolved: why the installed GitHub plugin description advertises a hybrid connector/CLI publication workflow while the Help Center says the ChatGPT GitHub app is read-only; whether that CLI route is available only in a separate Codex/Work/desktop surface; whether the connector tool surface is stale, over-advertises unsupported operations, or has a distinct write-capable execution path not exposed here.

Not established: that reconnecting or changing repository selection will enable writes. The Help Center's read-only limitation means reconnecting alone should not be presented as a fix. Nor is it established that the user can use Codex on the current plan/device/workspace; availability must be checked rather than assumed.

6. Next authorised route and practical boundary
The appropriate next route is to use a Codex experience that has repository/terminal access, if available to the user, and have it create a separate research-delta file and commit after reviewing the proposed content. Codex should use the existing repository doclinas-web/auden-rowan-research, preserve the existing README and history, add a new path under research-deltas/, and verify the remote commit and file by reading them back. Do not use force-push, rewrite history, or publish unrelated Library material.

This current chat cannot invoke Codex, a local terminal, or the user's authenticated Git credentials. It must not pretend that a GitHub API write has succeeded or shift the entire investigation burden to the user. It can prepare the exact commit payload and preserve a verified patch/portable archive in the Library. A supported Codex route is a distinct execution environment, not a permission that can be conjured by changing this chat's plugin permission setting.

7. Implications for the Auden–Rowan investigation
This episode demonstrates the need to separate:

user/repository authority;
provider-app capability and token permissions;
ChatGPT product-surface capabilities;
tool methods exposed to the assistant;
actual endpoint execution;
public readback verification.
A tool schema exposing a write action does not prove that the connected integration can execute it. A repository owner having push rights does not confer those rights on every third-party app. A product's general or marketing description does not supersede the actual supported surface or verified execution. The practical result is a real continuity/publication boundary even when the human account owner has authority to publish.

This finding is relevant to the wider inquiry into user agency, infrastructure sovereignty, portability, and the distribution of repair work. It does not establish deliberate obstruction or a universal inability to publish through every ChatGPT/Codex workflow.
