# Fractall.fit for AI Apps

Connect Codex or Claude Code to your Fractall club and ask about teams and athletes in plain language.

## Before You Start

- Install [Codex](https://chatgpt.com/codex) or [Claude Code](https://code.claude.com).
- Use the same email you use for Fractall.
- Your Fractall account must already exist and be active.

## Install In Codex

Run these two commands in Terminal:

```sh
codex plugin marketplace add Fractall-fit/llm-plugin-fractall
codex plugin add fractall-fit@fractall-fit
```

If your team uses a private GitHub repo, replace the first command with the HTTPS URL you were given.

## Install In Claude Code

In Claude Code, run:

```text
/plugin marketplace add Fractall-fit/llm-plugin-fractall
/plugin install fractall-fit@fractall-fit
/reload-plugins
```

If your team uses a private GitHub repo, replace the marketplace path with the HTTPS or SSH URL you were given.

## Sign In With Codex

Codex opens your browser during install. Log in with Fractall and approve access.

If login fails or tools do not appear:

1. Quit and reopen Codex.
2. Run `codex plugin add fractall-fit@fractall-fit` again.
3. Retry your question in a new chat.

## Sign In With Claude Code

Claude Code may ask you to authenticate the MCP server the first time it loads the plugin or calls a Fractall tool.

If login does not start automatically:

1. Run `/mcp` inside Claude Code.
2. Select the `fractall-fit` server.
3. Follow the browser login flow.
4. Run `/reload-plugins` if the tools still do not appear.

## Try It

Ask Codex or Claude Code things like:

- "List my Fractall teams"
- "Show athletes on my first team"
- "Summarize my club teams and roster sizes"

## Update Later

Codex:

```sh
codex plugin update fractall-fit@fractall-fit
```

Claude Code:

```text
/plugin marketplace update fractall-fit
/plugin install fractall-fit@fractall-fit
/reload-plugins
```

## Need Help?

Contact your Fractall admin if OAuth works but no teams appear. That usually means your account is not linked to a club yet.
