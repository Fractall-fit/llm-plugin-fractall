# Fractall.fit for Codex

Connect Codex to your Fractall club and ask about teams and athletes in plain language.

## Before you start

- Install [Codex](https://chatgpt.com/codex).
- Use the same email you use for Fractall.
- Your Fractall account must already exist and be active.

## Install

Run these two commands in Terminal:

```sh
codex plugin marketplace add Fractall-fit/codex-plugin-fractall
codex plugin add fractall-fit@fractall-fit
```

If your team uses a private GitHub repo, replace the first command with the HTTPS URL you were given.

## Sign in

Codex opens your browser during install. Log in with Fractall and approve access.

If login fails or tools do not appear:

1. Quit and reopen Codex.
2. Run `codex plugin add fractall-fit@fractall-fit` again.
3. Retry your question in a new chat.

## Try it

Ask Codex things like:

- "List my Fractall teams"
- "Show athletes on my first team"
- "Summarize my club teams and roster sizes"

## Update later

```sh
codex plugin update fractall-fit@fractall-fit
```

## Need help?

Contact your Fractall admin if OAuth works but no teams appear. That usually means your account is not linked to a club yet.
