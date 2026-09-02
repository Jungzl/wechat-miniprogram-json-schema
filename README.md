# wechat-miniprogram-json-schema — 编辑器集成

本仓库提供微信小程序配置文件的 JSON Schema（app.json、页面 index.json、theme.json），并示例了如何在常用编辑器中配置 Schema 以获得自动补全和校验。

## VS Code
在项目的 .vscode/settings.json 中添加或合并以下配置：

```json
{
  "json.schemas": [
    { "fileMatch": ["src/app.json"], "url": "https://raw.githubusercontent.com/Jungzl/wechat-miniprogram-json-schema/refs/heads/main/app.schema.json" },
    { "fileMatch": ["src/pages/**/*/index.json"], "url": "https://raw.githubusercontent.com/Jungzl/wechat-miniprogram-json-schema/refs/heads/main/page.schema.json" },
    { "fileMatch": ["src/theme.json"], "url": "https://raw.githubusercontent.com/Jungzl/wechat-miniprogram-json-schema/refs/heads/main/theme.schema.json" }
  ]
}
```

提示：可将 url 定为特定的 commit 或 tag，保证 Schema 的可复现性；根据工程目录调整 fileMatch 模式。

## JetBrains
JetBrains 系列 IDE 的 Schema 映射保存在 `.idea/jsonSchemas.xml` 中，或通过 Settings → Languages & Frameworks → Schemas and DTDs 图形化界面添加。示例条目：

```xml
<entry key="app.schema">
  <value>
    <SchemaInfo>
      <option name="name" value="app.schema" />
      <option name="relativePathToSchema" value="https://raw.githubusercontent.com/Jungzl/wechat-miniprogram-json-schema/refs/heads/main/app.schema.json" />
      <option name="schemaVersion" value="JSON Schema 7" />
      <option name="patterns">
        <list>
          <Item><option name="path" value="src/app.json" /></Item>
        </list>
      </option>
    </SchemaInfo>
  </value>
</entry>
```

将相似的 entry 添加到 `.idea/jsonSchemas.xml` 或通过 Settings → Languages & Frameworks → Schemas and DTDs 图形化界面添加即可生效（工作区设置）。

## Zed
Zed 支持通过 workspace settings 配置 JSON Schema（zconfig 或 .zed/settings.json，取决于 Zed 版本与扩展）。可按下面方式为 Zed 配置：

1. 在项目根目录创建 .zed/settings.json（若使用旧版本或自定义 zconfig，请参考 Zed 文档）：

```json
{
  "lsp": {
    "json-language-server": {
      "settings": {
        "json": {
          "schemas": [
            {
              "fileMatch": ["src/app.json"],
              "url": "https://raw.githubusercontent.com/Jungzl/wechat-miniprogram-json-schema/refs/heads/main/app.schema.json"
            },
            {
              "fileMatch": ["src/pages/**/index.json"],
              "url": "https://raw.githubusercontent.com/Jungzl/wechat-miniprogram-json-schema/refs/heads/main/page.schema.json"
            },
            {
              "fileMatch": ["src/theme.json"],
              "url": "https://raw.githubusercontent.com/Jungzl/wechat-miniprogram-json-schema/refs/heads/main/theme.schema.json"
            }
          ]
        }
      }
    }
  }
}
```

2. 重启 Zed 或在设置中重新加载工作区配置，打开相应 JSON 文件即可得到 Schema 驱动的提示与校验。

说明：该结构等价于 VS Code 的 `json.schemas` 配置，Zed 会把它透传给 `json-language-server`；若使用自定义 zconfig 或旧版本，请参考 Zed 官方文档确认同样的 `lsp` → `json-language-server` → `settings` → `json` → `schemas` 层级。

## 推荐实践
- 将编辑器映射指向仓库中具体的 commit/tag（例如使用 raw.githubusercontent.com/.../commit-hash/...），保证团队一致性。 
- 将工作区设置（.vscode、.idea、.zed）加入到仓库以便其他协作者复用，但注意不要提交包含敏感信息的个人设置。

仓库内主要 Schema 文件：`app.schema.json`、`page.schema.json`、`theme.schema.json`。如需针对其他文件添加 Schema，请参照以上示例添加对应的 fileMatch/pattern 条目。
