---
name: x-quote-watchlist
description: >
  查 X 帖谁引用了、名单里谁还没引用。输入 x.com 帖子链接或 status id，对照
  references/watchlist.md 输出已引用 / 未引用列表。
  触发词：谁引用了、谁没引用、查引用、引用名单、quote tweets、引用核对。
  Use when the user runs /x-quote-watchlist，或贴一条 X 帖让你看谁引用了。
---

# X 帖引用核对

用户贴一条帖，对照固定名单报：**谁引用了、谁没引用**。名单只读 `references/watchlist.md`，不要把名单抄进本文件。

## 何时使用

- `/x-quote-watchlist` 后跟帖子链接
- 「这个帖子谁引用了」「谁还没引用」
- 用户要增删核对名单

## 步骤

1. **读名单**  
   打开本 skill 目录下的 `references/watchlist.md`。handle 去掉 `@`，忽略大小写。

2. **解析帖子**  
   从 `x.com/{user}/status/{id}` 或 `twitter.com/.../status/{id}` 取出 status id。缺链接则先问。

3. **拉原帖**  
   `x_thread_fetch` 该 id。记下作者、时间、Quotes 计数、浏览量。

4. **拉引用**  
   `x_keyword_search`，`mode: Latest`，`limit: 10`：

   ```text
   quoted_tweet_id:{id}
   ```

   原帖 Quotes > 本次条数时再搜一轮 `mode: Top`，或用 `max_id:` 往下翻，直到公开引用收齐或确认搜不到更多。只收 **Quote**，不要把纯回复算进「已引用」。

5. **对照名单**  
   引用帖作者 handle ∈ 名单 → 已引用。  
   名单里其余人 → 未引用。  
   原帖作者若在名单里且没有引用自己：未引用表里备注「原作者」，不要当成漏引。  
   公开引用里不在名单的人：单独一小节列出，不要并进名单表。

6. **时间**  
   引用时间改成北京时间（UTC+8），格式 `MM-DD HH:mm`。原帖发布日也用北京时间。

7. **落盘**  
   把同一套 markdown 写到当前工作目录：

   `{原帖handle}-status-{id}-{YYYY}-{MMDD}.md`

   日期是原帖北京时间，月日两位、中间无连字符。例：`Liu_zhongxisn-status-2093135339351392407-2026-0828.md`。同名则覆盖。

## 回复格式

聊天里给下面这套，同时写入上一步的文件。先一行原帖：作者、链接、Quotes 计数、浏览。然后两张表：

```markdown
## 名单内 · 已引用（n/N）

| # | 账号 | 引用帖 | 时间（北京） | 互动 | 说了什么 |
|---|---|---|---|---|---|
| 1 | 显示名 @handle | [引用帖](https://x.com/handle/status/id) | 08-28 18:53 | 4赞 / 582浏览 | 一句话摘要 |

## 名单内 · 未引用（n/N）

| 账号 | 备注 |
|---|---|
| 显示名 @handle | |
```

账号只写 `显示名 @handle`，不要加主页链接。已引用按引用时间从早到晚。未引用按 watchlist 原顺序。互动写赞 / 转 / 浏览；有视频再标「带视频」。

名单外还有公开引用时，加一小节「名单外引用」，每人一行：账号 + 引用帖链接 + 浏览。没有则省略。

纯回复可附一句「另有 n 条回复，不算引用」，不必展开，除非用户要。

## 改名单

用户说把某人加入 / 移出核对名单时，只改 `references/watchlist.md`。改完回一句当前名单，不要重写本文件。

## 不要做

- 用点赞、转发、回复代替引用
- 把原作者没引用自己报成漏引事故
- 把名单写进 SKILL.md 或聊天里的「请更新 skill」
