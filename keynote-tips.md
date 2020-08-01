## Highlight Code

### 1. Use `highlight` command

Click [Code highlighting for Keynote presentations](https://gist.github.com/jimbojsb/1630790) to see more detail 

### 2. Use VS Code

Make sure you have set a shortcut for `editor.action.clipboardCopyWithSyntaxHighlightingAction`，you can use `Command + K` then `Command + S` to open shortcut map quickly.

```json
{
  "command": "editor.action.clipboardCopyWithSyntaxHighlightingAction",
  "key": "shift+cmd+c"
}
```

Then when you want to copy code with highlighting, just chose your code and press `Shift + Command + C` and paste it to Keynote.

