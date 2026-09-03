# CheddaBoards — Godot 3.x SDK

Online leaderboards, accounts, and achievements for Godot 3.x games. This is the Godot 3 backport of the [CheddaBoards Godot 4 SDK](https://github.com/cheddatech/cheddaboards-godot-addon).

**Using Godot 4?** You want [cheddaboards-godot-addon](https://github.com/cheddatech/cheddaboards-godot-addon) (or the [full template](https://github.com/cheddatech/cheddaboards-godot)) instead.

## Status

Community-supported backport, built and tested on Godot 3.6. Currently tracks **v2.2.5** of the Godot 4 SDK — identical signals, public methods, and response handling. It works and breaking bugs will be fixed, but new features land in the Godot 4 SDK first and may not be ported. PRs welcome.

## Quick start

1. Copy `addons/cheddaboards/CheddaBoards.gd` into your project
2. Add it in **Project Settings → AutoLoad** with the name `CheddaBoards`
3. Set your credentials from the [developer dashboard](https://cheddaboards.com):

```gdscript
CheddaBoards.set_api_key("cb_your-game_xxxxxxxxxx")
CheddaBoards.set_game_id("your-game")
```

Anonymous login (play immediately):

```gdscript
CheddaBoards.connect("login_success", self, "_on_login")
CheddaBoards.login_anonymous()
```

Submit a score:

```gdscript
CheddaBoards.submit_score(1234)
```

Social login (Google / Apple / Internet Identity) works everywhere via Device Code Auth — the player links on their phone at cheddaboards.com/link. See the header comments in `CheddaBoards.gd` for the full quick-start walkthrough.

## Docs

Full documentation, guides, and API reference: [cheddaboards.com](https://cheddaboards.com) and the [Godot 4 SDK docs](https://github.com/cheddatech/cheddaboards-godot-addon) — the API surface is identical, only the GDScript syntax differs (`yield` instead of `await`, `connect()` instead of typed signals).

## License

MIT — see [LICENSE](LICENSE).
