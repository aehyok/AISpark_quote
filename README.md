# AISpark_quote

对照固定名单，核对某条 X 帖**谁引用了、谁还没引用**。

给一条 `x.com/.../status/{id}` 链接，会拉公开 Quote（不算点赞、转发、纯回复），对照 `.grok/skills/x-quote-watchlist/references/watchlist.md`，在聊天里出表，并在仓库根目录落一份 markdown。

## 怎么用

在本仓库里对 Grok 说：

```text
/x-quote-watchlist https://x.com/Liu_zhongxisn/status/2093135339351392407
```

也可以直接贴帖子链接，或说「这个帖谁引用了 / 谁还没引用」。

## 结果文件

每次核对会在仓库根目录写一份。同一条帖再跑一次，用最新引用数据**整文件覆盖**该文件，不追加、不另存：

```text
{原帖handle}-status-{id}-{YYYY}-{MMDD}.md
```

日期是原帖**北京时间**，月日两位、中间无连字符。例如：

`Liu_zhongxisn-status-2093135339351392407-2026-0828.md`

文件里有：原帖一行（作者、链接、Quotes、浏览）→ 名单内已引用 → 名单内未引用。有名单外公开引用再加一小节。账号只写 `显示名 @handle`，不挂主页链接。

## 核对名单

名单只维护这一处：

`.grok/skills/x-quote-watchlist/references/watchlist.md`

对 Grok 说「把某某加入 / 移出核对名单」即可改这份文件。handle 不带 `@`，比对忽略大小写。

原作者在名单里但没引用自己时，会标「原作者」，不当成漏引。

## 目录

```text
AISpark_quote/
├── README.md
├── .grok/skills/x-quote-watchlist/
│   ├── SKILL.md                 # 核对流程
│   └── references/watchlist.md  # 核对名单
└── {handle}-status-{id}-{YYYY}-{MMDD}.md   # 每次核对的结果
```
