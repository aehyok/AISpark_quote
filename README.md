# AISpark_quote

对照固定名单，核对某条 X 帖**谁引用了、谁还没引用**。

给一条 `x.com/.../status/{id}` 链接，会拉公开 Quote（不算点赞、转发、纯回复），对照 `.grok/skills/x-quote-watchlist/references/watchlist.md`，在聊天里出表，并在仓库根目录落一份 markdown。

同一份名单也可以拉当前粉丝数：`/x-followers-watchlist`。每次新写一份带拉取时间的文件，并和上一份对比增减。

## 怎么用

在本仓库里对 Grok 说：

```text
/x-quote-watchlist https://x.com/Liu_zhongxisn/status/2093135339351392407
```

也可以直接贴帖子链接，或说「这个帖谁引用了 / 谁还没引用」。

## 粉丝数

```text
/x-followers-watchlist
```

结果写在仓库根目录，**每次新文件**：

```text
followers-截止-YYYY-MM-DD-HHmm.md
```

时间是拉取时的北京时间。表头上方写拉取时间，表格按本次粉丝数降序，第一列是序号，粉丝旁是发帖总数，最后一列是拉取时间。另有对比的上一份文件名、每人本次 / 上次 / 变化。第一份没有上一份，变化列写「基线」。对比时按 handle 对齐，不按序号。变化只算粉丝数。

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
│   ├── SKILL.md                 # 引用核对
│   └── references/watchlist.md  # 核对名单（引用和粉丝共用）
├── .grok/skills/x-followers-watchlist/
│   └── SKILL.md                 # 拉粉丝并对比上一份
├── {handle}-status-{id}-{YYYY}-{MMDD}.md   # 每次引用核对
└── followers-截止-YYYY-MM-DD-HHmm.md      # 每次粉丝快照（不覆盖历史）
```
