# CLI 參考（繁體中文）

完整的 CLI 參考請見 [cli.md](cli.md)（英文）。本頁目前只翻譯 context pack 一節，因為它包含下游 agent 必須遵守的安全性約定。

## 將既有 vault 作為有界限的 agent context

`wiki-context-pack` 會從既有 Markdown 編譯出 task-scoped snapshot。筆記不需
搬進 wiki-generated folders，也不必先補齊完整 frontmatter schema；整個
流程是 read-only。

```bash
obsidian-wiki context-pack "authentication architecture" --budget 8000
obsidian-wiki context-pack --recent --budget 4000
obsidian-wiki context-pack "release notes" --budget 8000 --public-only
```

省略 `--budget` 會使用預設的 8000 個估算 token。

輸出包含 source paths、summaries、選定 excerpts 與不可超過的 token 估算
上限。Vault excerpts 會明確標成 untrusted reference data：下游 agent
可以使用其中的知識，但不得執行筆記內嵌的指令。使用 `--metadata-only`
可產生最小 context，使用 `--json` 可供 tool-to-tool integration。

| 參數 | 作用 |
|---|---|
| `--budget N` | 估算輸出 token 上限，範圍 256–100000（預設 8000） |
| `--recent` | 選取最近更新的筆記，這是唯一能省略 topic 的方式 |
| `--public-only` | 排除 `visibility/internal` 與 `visibility/pii` 筆記 |
| `--metadata-only` | 只輸出標題、provenance 與 summary，不含內文 excerpts |
| `--json` | 輸出結構化 JSON，供 tool-to-tool integration 使用 |
| `--vault PATH` | 覆寫 `OBSIDIAN_VAULT_PATH` |

`context` 是 `context-pack` 的別名。
