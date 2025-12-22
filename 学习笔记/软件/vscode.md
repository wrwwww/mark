## 设置vim

jk键映射到esc

```
{
    "files.autoSave": "onFocusChange",
    "git.openRepositoryInParentFolders": "never",
    "vim.useSystemClipboard": true,
    "[rust]": {
        "editor.defaultFormatter": "rust-lang.rust-analyzer"
    },
    "vim.commandLineModeKeyBindings": [

    ],
    "vim.insertModeKeyBindings": [
             {
            "before":["j","j"],
            "after":["<Esc>"]
        },
    ]
}
```