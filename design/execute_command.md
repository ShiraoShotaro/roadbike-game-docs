---
id: design-execute-command
---

# Execute Command

執筆中だけど簡単に

json で、 `cmd` をかならず持つ

あとのキーは executor によって変わります。

```json
{
    "cmd": "<EXECUTOR SPECIFIED>",
    "key": "value", ...
}
```

このコマンドは配列になっていて、一度に複数のコマンドをキューに積むことができます。
