# Changelog

All notable changes to **EastClub Agent**.

## v1.6.2 — 2026-10-20
- **进入强制注册 / 登录**：通过访问密码后，必须登录或注册社区账号才能进入对话（强制，不可跳过）。注册必须填写真实姓名（2-20 位中文或字母，必填），新注册即送 20 CDY；对话与相册保存在账号下，任意设备登录同一账号均可同步。
- **代码雨锁屏**：侧边栏新增「代码雨锁屏」选项，点击后全屏播放绿色代码雨动画（Matrix 风格），点击任意处或 25 秒后自动解锁。
- **品牌更新为「十六度空间」**：社区、论坛、群聊、私信、功能介绍、版本日志六个页面品牌全部改为「十六度空间 XXX」（十六度空间社区 / 论坛 / 群聊 / 私信 / 功能介绍 / 版本日志）。
- **admin 可修改智能体根本人格 / 安全护栏 / 身份**：管理后台新增「智能体核心配置」面板，可设置身份设定、人格设定、安全护栏三个字段并保存；保存后对话的系统提示词即时生效（身份/人格补充或覆盖默认，安全护栏作为系统强制段追加），也可一键恢复默认。配置持久化存于 KV。

## v1.6.1 — 2026-10-19
- **聊天室摘要 1 → 5 条**：社区页群聊摘要由最后 1 条改为最近 5 条，逐条以虚线分隔展示。
- **外链改回横排**：页脚 GitHub / LinkedIn / RA2 Web / MC.JS 链接恢复为横向排列（上一版误改成竖排）。
- **每次进入随机外观与字体**：新增设置项「每次进入随机外观与字体」（默认开启），每次进入自动从 5 套主题、4 种字体中各随机一套；可在设置弹窗关闭，关闭后用固定的手动选择。
- **私信功能**：登录社区账号后可在独立私信页（/dm.html）向 1-56 号账号发私信、查看收件箱（标未读/点击已读、显示未读数）；未登录仅提示需登录。管理员在后台社区面板可查看全部私信与删除。
- **推荐区最新投票入口**：主界面推荐区顶部新增「最新投票」横条，实时显示最近发起的投票标题，点击跳转社区投票站。
- **更多货币机制**：签到改为「连续签到加成」（基础 10 + 连续天数 ×2，最多 +20）；发帖 +2 CDY；论坛帖子被点赞时楼主 +1 CDY；投票参与 +3 CDY；发起投票仍扣 2 CDY。防重复：同账号不可重复领投票奖励、重复投。
- **修复社区页脚本失效**：社区页摘要的群聊拉取原本写在 `<script>` 顶层、含非法顶层 `await`，导致社区页整段脚本不解析（登录/签到/投票/聊天/摘要全部失效）。已并入 async 函数，社区页所有功能与摘要恢复。

## v1.6.0 — 2026-10-19
- **Sidebar bottom options switched to vertical**: the six links at the bottom of the sidebar (Admin / Forum / Chat / Community / Features / Log) now stack vertically instead of a horizontal row, each with hover highlight; the status light + language + settings row is on its own line and the GitHub/LinkedIn/RA2/MC.JS footer links also become vertical.
- **Community entry on the welcome screen**: the main landing page now shows four glassmorphism hub cards (Community / Forum / Chat / Features) linking straight to each module.
- **Community page summaries**: the community page now shows a forum summary (latest two posts) and a chatroom summary (last message); clicking either jumps into the dedicated page.
- **Forum: images + likes + tipping**: you can now attach up to 6 images when posting (auto-compressed and stored via `/api/upload`), like posts (deduplicated per user, guests get an IP-derived ID), and tip authors with CDY coins (1–1000, balance-checked, deducted from tipper and credited to the author). Fixed the tip endpoint that failed with an internal error because `acctPut(e,a)` omitted the number argument.
- **Top bar slimmed**: apart from Deep-Thinking and Web-search toggles, all the top-bar icon links were removed — entries live in the sidebar and welcome hub now. On mobile the thinking/web toggles are visible again.
- **Pollinations drawing merged into the model list**: pick `Pollinations Flux / Turbo / JuggernautXL` in the model dropdown and sending a prompt draws instead of chatting.
- **First-visit onboarding**: new visitors get a guided intro listing every module and are encouraged to register; it appears once and won't reappear.
- **Account-level cross-device chat sync**: signing in with the same account on any device restores the same conversations; the server validates that an `acct-` session key belongs to the logged-in account.

## v1.5.0 — 2026-10-18
- **Mobile model switcher restored**: the model dropdown now lives in the composer next to the input, on the same row as the image / drawing buttons — visible on all screen sizes.
- **Hidden persona editor in Settings**: the "System prompt (CDY persona)" field is no longer shown in the settings dialog.
- **Default model is now GLM-4.5-Flash**: on first visit the conversation now starts on glm-4.5-flash; your own saved choice still wins.
- **Fixed mobile "stuck" chat scrolling**: `#scroller` and `.main` lacked `min-height:0` — a classic flexbox bug. Added `min-height:0`, `-webkit-overflow-scrolling:touch` and `overscroll-behavior:contain`. Verified: with 30 injected messages the scroller overflows (3465px > 614px) and scrolls.

## v1.4.9 — 2026-10-18
- **Community storage rebuilt: D1 + R2 + Durable Objects, each doing what it's best at, with KV fallback.**
  - **D1 database** (`DB` binding): accounts / numbers / check-ins / CDY coins / polls / forum / announcements / public chatroom — structured data now lives in SQL tables.
  - **R2 object storage** (`IMGS` or `R2` binding): uploaded images and Pollinations drawings go straight into the object bucket.
  - **Durable Objects** (`ChatRoomDO` class): group-chat messages and online members are coordinated by a single-point Durable Object.
  - **KV** now only keeps sessions / memory / image fallback, and acts as the automatic fallback layer — smooth migration, no data loss.
  - With **no** new bindings configured, everything falls back to KV exactly as before.

## v1.4.8 — 2026-10-18
- **Fixed sidebar bottom links showing wrong labels**: `applyI18n()` was overwriting the first four sidebar links with mismatched keys — "论坛/群聊/社区" were wrongly shown as "社区/功能/日志". Fixed so all six links render correctly in both languages.
- **Chatroom now works for guests**: guests (anyone past the access password) now appear in the online list as `访客-xxxx` and can send messages immediately.

## v1.4.7 — 2026-10-18
- **Fixed forum & chatroom translation crash**: the `T()` helper indexed the `{zh,en}` object with a numeric index, always `undefined` — all toasts threw. Posting, sending and all hints work again.
- **Fixed "some devices can't use the site"**: cookie builder hard-coded `Secure`; now set only on `https://`. HTTPS stays secure, HTTP now works.

## v1.4.6 — 2026-10-18
- **Global notification system**: every page polls `/api/notifications` every 20 s and pops a toast for new chat messages, new polls, poll results and site-wide announcements.
- **Poll upgrade**: creators can set a duration (unlimited / 1h / 6h / 24h / 3d / 7d) and optionally open to guests; overdue polls close lazily, winners computed (ties included), result notification auto-published.

## v1.4.5 — 2026-10-18
- **Standalone group chat** (`/chat.html`): online member list, message bubbles, emoji picker, auto-scroll, ~1.2s incremental refresh plus presence heartbeat.
- Backend `/api/room/*`: incremental message pull, send, online users, heartbeat; messages persisted to KV (30-day TTL).

## v1.4.4 — 2026-10-18
- **Fixed Pollinations HTTP 402 for real**: free tier only answers 200 with `model=flux&private=true&nologo=true`; any added param returns 402.
- **New Forum** (`/forum.html`): logged-in visitors can post, reply, browse and open posts; persisted to KV. Bilingual UI.
- **Footer links** on chat, community and forum pages: GitHub, LinkedIn, RA2 Web, MC.JS.

## v1.4.3 — 2026-10-17
- **Fixed KV data expiring after ~6 minutes**: all persisted data used a TTL of 365 *seconds*. Changed to 365 days.
- **Fixed Pollinations image generation returning HTTP 402**: only `flux` on free tier.
- **Hardened the Settings modal**: save/render wrapped so settings can never get stuck open.
- **Mobile scrolling fallback**: `100dvh` now has a `100vh` fallback.

## v1.4.2 — 2026-10-17
- **LaTeX math rendering** in chat messages via KaTeX.
- **Official site/agent icon** replaced with the uploaded CDY artwork.

## v1.4.1 — 2026-10-17
- Fixed a login-gate frontend JS escaping bug that prevented access from being unlocked.

## v1.4.0 — 2026-10-17
- **EastClub Agent** branding, bilingual interface, liquid-glass visuals and easter eggs.
- Model dropdown & agent name upgraded to "a … CDY (powered by …)".
- Cross-model conversation context; text-only models auto-omit image history.
- Enhanced Markdown (bold, italic, strikethrough, highlight, kbd, spoiler).
- Hidden commands `/matrix /theme /font /lang /glass /motion /surprise`, Konami code, logo-tap egg.

## v1.3.0 — 2026-10-17
- Community: 1–56 numbered accounts, salted password hashing, signup/login, daily check-in, CDY-coin economy, leaderboard.
- Public chatroom & poll station; admin moderation.
- Skylake / Matrix / Midnight / Sakura / Paper themes & multi-font.
- DuckDuckGo free web search.

## v1.2.0 — 2026-10-16
- Pollinations.ai image generation (`/api/draw`), gallery & session integration.
- Memory events, user memory documents, global CDY personality doc and Dream logs.
- Worker `scheduled` entry + admin Dreaming button.
- `/about.html` and `/changelog.html`.

## v1.1.0 — 2026-10-16
- Converged to **free-only** BigModel GLM Flash / Gemini free tiers; removed all paid models & paid tools.

## v1.0.0 — 2026-10-16
- Password gate + Matrix intro, CDY persona chat, image upload, KV sessions/gallery, admin panel.