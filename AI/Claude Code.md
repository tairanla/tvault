# Claude Code

## 国产模型接入

配置环境变量即可。

参考：<https://juejin.cn/post/7543530064106602547>

配置环境变量：

```bash
# Claude Code
# bigmodel
export ANTHROPIC_BASE_URL=https://open.bigmodel.cn/api/anthropic
export ANTHROPIC_AUTH_TOKEN="xxxxxx"

# bailian
export ANTHROPIC_BASE_URL=https://dashscope.aliyuncs.com/api/v2/apps/claude-code-proxy
export ANTHROPIC_AUTH_TOKEN=sk-xxxxxx
```

或者配置 _~/.claude/settings.json_ 文件：

```json
{
    "env": {
        "ANTHROPIC_BASE_URL": "<https://open.bigmodel.cn/api/anthropic>",
        "ANTHROPIC_AUTH_TOKEN": "xxxxxx"
    }
}
```

## 压缩上下文

Claude Code 为了应对上下文窗口容量的限制，会自动进行“压缩”（compact）操作，即对话历史会被摘要后缩减以节省 token 用量。具体来说，当上下文使用率接近 95% 时，会触发自动压缩。

如何关闭压缩上下文？(GLM4.5 等模型设置关闭压缩上下文会好一点，因为本身自带上下文压缩能力）

```bash
# claude 进入终端后，在当前项目下执行并生效
/config
# 输入以上命令后，在交互窗口设置 Auto-compact

# 或者，全局生效
claude config set -g autoCompactEnabled false
```

## 命令

- 恢复会话

```bash
claude -r
```
