# Felworld configs

Part of [Felworld](https://github.com/felworld/azerothcore) — a tech demo of
AI "players" (LLM agents + classical game AI) populating and interacting in
an MMO world. This repo versions our "best" server configs, as determined
through playtesting. It is mounted into the Felworld
containers as the `env/dist/etc` submodule, which is exactly where the
servers read their configs from — edit here, restart the server, done.

## Layout

| File | Read by |
| ---- | ------- |
| `worldserver.conf` | ac-worldserver (game rules, rates, logging — the big one) |
| `authserver.conf` | ac-authserver (login server) |
| `dbimport.conf` | ac-db-import (database populator) |
| `modules/playerbots.conf` | mod-playerbots |
| `modules/mod_llm.conf` | mod-llm |
| `modules/mod_ahbot.conf` | mod-ah-bot-plus (auction house market maker) |

## Conventions

Each config is a clean parallel pair:

- **`.conf.dist`** — the upstream template, verbatim. Never hand-edited;
  refreshed when upstream changes.
- **`.conf`** — the template plus our value overrides, and nothing else. The
  diff between the two files *is* our configuration.

When an upstream `.conf.dist` changes, regenerate the `.conf` with
`tools/regen-config.py` in felworld/azerothcore — a 3-way merge that
re-applies our value overrides onto the fresh template.

One more layer sits on top: Felworld's session-mode env files (`.env.solo` /
`.env.dumbbots` / `.env.llm`) set `AC_<OPTION>` environment variables,
which the server reads in preference to the value in these files. Options
that define a session mode (playerbots on/off, LLM bots on/off, canned
chatter) are controlled there; everything else is controlled here.

---

Felworld is a non-commercial research project, not affiliated with or
endorsed by Blizzard Entertainment — see the
[project disclaimer](https://github.com/felworld/azerothcore#license-and-disclaimer).
