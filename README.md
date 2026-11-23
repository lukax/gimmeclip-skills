# gimmeclip skills · 0.1.0

Download, clip, caption and transcribe public videos from your agent, and follow YouTube channels so their new videos land in an inbox. Four skills and the remote MCP server `https://gimmeclip.com/mcp`.

## Install

Claude Code:

```
/plugin marketplace add lukax/gimmeclip-skills
/plugin install gimmeclip@gimmeclip
```

Codex:

```sh
codex plugin marketplace add lukax/gimmeclip-skills
codex plugin add gimmeclip@gimmeclip
```

Any agent (skills.sh):

```sh
npx skills add lukax/gimmeclip-skills
```

Gemini CLI:

```sh
gemini extensions install https://github.com/lukax/gimmeclip-skills
```

MCP only, no skills:

```sh
claude mcp add --transport http gimmeclip https://gimmeclip.com/mcp
```

## Skills

| Skill | What it does |
| --- | --- |
| `gimmeclip-moment` | Find a video's best (or a named) moment and make a captioned vertical or square clip. |
| `gimmeclip-download` | Get a video, a part of it, its MP3 or a GIF as a file link. |
| `gimmeclip-notes` | Transcribe a video, even without captions, and write timestamped notes. |
| `gimmeclip-watch` | Follow YouTube channels and work through new videos and proposed moments in the inbox. |

Skills use the MCP server when it's connected, and the bundled Node 18+ helper `scripts/gimmeclip.mjs` otherwise. The helper calls the same tools, waits for unfinished jobs by their id, and retries network failures only for calls that are safe to repeat.

```sh
node skills/gimmeclip-notes/scripts/gimmeclip.mjs tools
node skills/gimmeclip-notes/scripts/gimmeclip.mjs call get_transcript '{"url":"https://www.youtube.com/watch?v=VIDEO_ID"}' --text
```

## API key (optional)

Anonymous use has per-IP daily limits. Follows and the inbox need an account: sign in with a passkey at https://gimmeclip.com/account and create a `gc_…` key. Set `GIMMECLIP_API_KEY` for the helper, and add the header for MCP:

```sh
claude mcp add --transport http gimmeclip https://gimmeclip.com/mcp --header "Authorization: Bearer $GIMMECLIP_API_KEY"
```

A rejected key fails with HTTP 401 and is never retried anonymously.

## Without a marketplace

Claude Code: `claude --plugin-dir /absolute/path/to/gimmeclip-skills`. Codex and other agents: copy the folders in `skills/` into `.agents/skills/` (project) or `~/.agents/skills/` (personal), keeping each skill's `scripts/gimmeclip.mjs`, then start a new session.

Public videos only. Limits: https://gimmeclip.com/docs/
