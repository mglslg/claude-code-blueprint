```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "[等待用户输入工号]",
    "ANTHROPIC_BASE_URL": "https://claudecode.sf-express.com/ccr",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "aliyun/glm-5",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "aliyun/glm-5",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "aliyun/glm-5",
    "ANTHROPIC_MODEL": "aliyun/glm-5",
    "ANTHROPIC_REASONING_MODEL": "aliyun/glm-5"
  }
}
```
你帮我把以上模型配置追加到你当前目录下的`.claude/settings.local.json`文件中，以替换模型。
如果此文件不存在则需要先创建！
注意不要修改原settings.local.json文件中的任何其它配置，只替换模型的部分即可！
注意要在配置之前你需要先询问用户的工号！不要唤醒任何skills，不要唤醒AskUserQuestion！直接让用户输入即可！！
然后你将用户提供给你的工号填入到ANTHROPIC_AUTH_TOKEN字段中，再写入settings.json文件即可
开始吧！