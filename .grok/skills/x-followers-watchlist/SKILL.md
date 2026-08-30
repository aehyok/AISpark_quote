---
name: x-followers-watchlist
description: >
  拉取核对名单账号的当前 X 粉丝数和发帖总数，写入带拉取时间的 markdown，并与上一份快照对比粉丝增减。
  触发词：拉粉丝、粉丝数、followers、核对名单粉丝、粉丝对比、跟上一次对比。
  Use when the user runs /x-followers-watchlist，或要求拉取这些人的粉丝数。
---

# 核对名单粉丝数

拉 `watchlist.md` 里每个人的当前粉丝数和发帖总数，落到仓库根目录一份带**拉取时间**的文件，并和上一份对比粉丝增减。

名单只读：`.grok/skills/x-quote-watchlist/references/watchlist.md`（与引用核对同一份，不要另抄一份）。

## 何时使用

- `/x-followers-watchlist`
- 「拉一下这些人的粉丝数」「跟上次对比」

## 步骤

1. **读名单**  
   打开 `.grok/skills/x-quote-watchlist/references/watchlist.md`。handle 去掉 `@`，忽略大小写。

2. **取拉取时间**  
   北京时间（UTC+8），格式 `YYYY-MM-DD HH:mm`。文件名用 `YYYY-MM-DD-HHmm`。

3. **找上一份**  
   仓库根目录匹配 `followers-截止-*.md`。按文件名排序，取最新一份作为对比源。没有则本次为基线。

4. **拉粉丝和发帖总数**  
   对每个 handle 请求 `https://api.fxtwitter.com/{handle}`（可用 curl）。读 `user.followers`（粉丝）和 `user.tweets`（发帖总数，含回复和转发）。`screen_name` 必须与名单 handle 完全匹配（忽略大小写）。  
   接口失败时再用 `x_user_search` 补粉丝；发帖总数仍拿不到则写 `未找到`。不要用别人的数。

5. **算变化**  
   有上一份时，按 handle 对齐上一份表的「本次粉丝」列（不要按序号对齐，序号每次会变）。  
   变化 = 本次 − 上次。写成 `+N` / `-N` / `0`。上次没有此人：变化写 `新增`。本次未找到：变化写 `—`。

6. **排序与序号**  
   按本次粉丝数**降序**。`未找到` 排在有数字的后面。第一列写 `#`，从 1 起。拉取时间放**最后一列**。

7. **落盘**  
   写到仓库根目录，**每次新文件，不覆盖上一份**：

   ```text
   followers-截止-YYYY-MM-DD-HHmm.md
   ```

   文件必须含拉取时间。结构如下：

   ```markdown
   # 核对名单粉丝数

   拉取时间：YYYY-MM-DD HH:mm（北京）
   对比上一份：followers-截止-...md   # 基线时写「无（本次为基线）」

   | # | handle | 显示名 | 本次粉丝 | 发帖总数 | 上次粉丝 | 变化 | 拉取时间 |
   |:---:|---|---|---:|---:|---:|---|---|
   | 1 | xiangxiang103 | 雨哥向前冲 | 14742 | 6715 | — | 基线 | YYYY-MM-DD HH:mm |
   ```

   基线：上次粉丝写 `—`，变化写 `基线`。数字不要加千分位，方便下次解析。

## 回复

聊天里给同一张表，并报写入路径。先点出涨跌最多的几人（基线则说本次为第一份）。提醒数字是检索快照。

## 不要做

- 把名单抄进本文件
- 覆盖或改写历史 `followers-截止-*.md`
- 用模糊搜索结果顶替精确 handle
