# ai-marketplace

Plugin marketplace catalog for `sebmartin/ai-marketplace`.

For full documentation, see: https://code.claude.com/docs/en/plugin-marketplaces

## marketplace.json schema

### Top-level fields

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | Yes | Marketplace identifier (kebab-case). Users reference it as `plugin@name`. Reserved names (anthropic-*, claude-*) are blocked. |
| `owner` | object | Yes | Maintainer info: `name` (required), `email` (optional) |
| `plugins` | array | Yes | List of plugin entries |
| `metadata.description` | string | No | Brief marketplace description |
| `metadata.version` | string | No | Marketplace version |
| `metadata.pluginRoot` | string | No | Base directory prepended to relative plugin source paths. E.g. `"./plugins"` lets you write `"./my-plugin"` instead of `"./plugins/my-plugin"` |

### Plugin entry fields

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | Yes | Plugin identifier (kebab-case) |
| `source` | string\|object | Yes | Where to fetch the plugin (see Plugin sources below) |
| `description` | string | No | Brief plugin description |
| `version` | string | No | Plugin version (avoid setting in both marketplace.json and plugin.json — plugin.json wins silently) |
| `author` | object | No | `name` (required), `email` (optional) |
| `homepage` | string | No | Plugin homepage URL |
| `repository` | string | No | Source code repository URL |
| `license` | string | No | SPDX identifier (e.g. `MIT`) |
| `keywords` | array | No | Tags for discovery |
| `category` | string | No | Plugin category |
| `tags` | array | No | Additional searchability tags |
| `strict` | boolean | No | Default `true`. When `true`, `plugin.json` is the authority and marketplace entry supplements it. When `false`, marketplace entry is the entire definition (plugin.json must not also declare components). |
| `commands` | string\|array | No | Custom paths to command files or directories |
| `agents` | string\|array | No | Custom paths to agent files |
| `hooks` | string\|object | No | Hooks configuration or path to hooks file |
| `mcpServers` | string\|object | No | MCP server configurations or path to MCP config |
| `lspServers` | string\|object | No | LSP server configurations or path to LSP config |

### Plugin sources

| Source type | Format | Required fields | Notes |
|---|---|---|---|
| Relative path | `string` | — | Must start with `./`. With `pluginRoot` set, path is relative to pluginRoot. Bare strings without `./` fail schema validation. |
| GitHub | `{"source": "github", "repo": "owner/repo"}` | `repo` | Optional: `ref` (branch/tag), `sha` (40-char commit) |
| Git URL | `{"source": "url", "url": "https://....git"}` | `url` | URL must end in `.git`. Optional: `ref`, `sha` |
| Git subdirectory | `{"source": "git-subdir", "url": "...", "path": "subdir"}` | `url`, `path` | Sparse clone of a monorepo subdirectory. `url` also accepts `owner/repo` shorthand or SSH. Optional: `ref`, `sha` |
| npm | `{"source": "npm", "package": "@scope/name"}` | `package` | Optional: `version`, `registry` |
| pip | `{"source": "pip", "package": "name"}` | `package` | Optional: `version`, `registry` |

### Version guidance

- Co-located plugins (relative path source): set `version` in `marketplace.json`, omit from `plugin.json`
- External plugins (github/url/etc. source): set `version` in the plugin's own `plugin.json`, omit from `marketplace.json`
- If set in both, `plugin.json` wins silently — the marketplace version is ignored

### ${CLAUDE_PLUGIN_ROOT}

Use `${CLAUDE_PLUGIN_ROOT}` in hooks and MCP server configs to reference files within the plugin's installed location. Plugins are copied to a cache directory on install, so absolute paths and paths outside the plugin directory will not work.
