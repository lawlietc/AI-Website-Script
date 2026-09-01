# AI Web Script

> 一个整合多个 AI 聊天网站增强功能的 Tampermonkey 油猴脚本，按网址自动适配。

## 支持的网站与功能

| 网站 | 匹配地址 | 快捷键 / 功能 |
|------|----------|---------------|
| DeepSeek | `chat.deepseek.com` | `Ctrl+K` ↔ `Ctrl+J` 互换（让 `Ctrl+K` 触发新建对话） |
| Gemini | `gemini.google.com/app*` | `Ctrl+K` 新建对话 |
| Grok | `grok.com` | `Ctrl+K`（Mac: `Cmd+K`）映射为新建对话 |
| Qwen | `chat.qwen.ai` | `Ctrl+K` 新建对话并自动开启网络搜索、深度思考；`Alt+1` 聚焦输入框 |
| ZAI | `chat.z.ai` | `Ctrl+K` 新建对话并切换搜索；`Alt+1` 聚焦输入框 |
| 豆包 Doubao | `*.doubao.com` | `Alt+1` 聚焦输入框；自动深色模式；侧边栏显示/隐藏；隐藏推荐内容（后三项可在油猴菜单开关） |

> **快捷键速记**：`Ctrl+K` 在多数站点用于「新建对话」；`Alt+1` 用于「聚焦输入框」。Mac 上 Grok 使用 `Cmd+K`。
> 注：Gemini 仅匹配 `/app` 路径，从首页进入时会先跳转，脚本随后生效。

## 安装

1. 浏览器安装 [Tampermonkey](https://www.tampermonkey.net/) 扩展。
2. 点击 `AI-Web-Script.user.js`，在 Tampermonkey 中确认安装。
3. 打开任意支持的网站即可生效。

## 配置

脚本顶部有一个 `ENABLED` 配置对象，可按站点开关各套功能，无需删除代码：

```js
const ENABLED = {
  deepseek: true,
  gemini:   true,
  grok:     true,
  qwen:     true,
  zai:      true,
  doubao:   true,
};
```

把某个站点设为 `false` 即可停用对应逻辑。

## 实现要点

- **按网址分流**：脚本加载后根据 `location.hostname` 调用对应的 `initXxx()`，各站点逻辑彼此隔离，快捷键不会跨站冲突。
- **运行环境**：因豆包功能依赖 `GM_registerMenuCommand` / `GM_setValue` / `GM_getValue`，整个脚本运行在油猴沙箱（isolated world）。对纯 DOM / 事件类功能无影响，脚本伪造的键盘事件仍会跨 world 送达页面原生监听器。
- **注入时机**：统一 `@run-at document-start`；DOM 依赖型代码内部做了 `DOMContentLoaded` / `readyState` 守卫，避免页面未就绪时出错。


## 更新与维护

各网站 UI 改版时，只需更新对应 `initXxx()` 内的选择器 / 逻辑，不影响其它站点。

## License

MIT
