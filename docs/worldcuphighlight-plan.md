# worldcuphighlight.com — Kế hoạch xây dựng site World Cup 2026

## 1. Bối cảnh (Context)

- **Tài sản**: domain `worldcuphighlight.com`.
- **Sự kiện**: FIFA World Cup 2026 khai mạc **11/06/2026** tại Mỹ/Mexico/Canada — 48 đội, 104 trận, 16 thành phố chủ nhà, kéo dài tới ~19/07/2026. Hôm nay 11/05/2026 — còn **đúng 1 tháng**.
- **Mục tiêu**: thu hút traffic toàn cầu trước & trong giải để **monetize qua AdSense + affiliate** (theo lựa chọn của user).
- **Ràng buộc cốt lõi**:
  - **Bản quyền nghiêm ngặt** — không host/re-upload video, ảnh, logo FIFA; chỉ dùng nguồn miễn phí có giấy phép mở (CC, public domain, embed hợp pháp).
  - **Đa ngôn ngữ**: EN (chính), ES, PT, FR, VI — để tối đa hóa thị phần SEO.
  - **Stack**: Astro + Cloudflare Pages (cost gần $0).
- **Thách thức**: cạnh tranh với ESPN/BBC/FIFA.com — không thể thắng ở "live news", phải thắng ở **content sâu, công cụ tương tác, SEO long-tail, đa ngôn ngữ ngách**.

---

## 1.5 Quyết định đã chốt (Locked decisions)

| Quyết định | Lựa chọn |
|------------|----------|
| Audience | **Toàn cầu đa ngôn ngữ** (EN chính + ES/PT/FR/VI) |
| Stack | **Astro + Cloudflare Pages** |
| Monetization | **AdSense + affiliate** (booking, VPN, streaming, Amazon) |
| MVP strategy | **Cân bằng**: SEO foundation + 1 tính năng viral (Bracket) + Where-to-Watch v1 — chạy song song |
| Tính năng committed extras | **PWA + Web Push** & **Custom WC Sticker Album** (gốc art tự thiết kế) |
| KPI mục tiêu (19/07/2026) | **500K visits / $3K doanh thu** |

→ KPI 500K visits = ~8K/ngày trung bình, peak 30-50K/ngày trong giải. $3K = ~$5-7 RPM hỗn hợp (AdSense + affiliate). Khả thi nếu thực thi đủ 17.5K pages × 5 ngôn ngữ + Bracket viral hit + Where-to-Watch chuyển đổi tốt.

---

## 2. Định vị sản phẩm (Positioning)

**"Fan-made global encyclopedia & companion app for World Cup 2026"** — không phải news site, mà là **bách khoa toàn thư + công cụ fan**: lịch sử, dữ liệu, tương tác, dự đoán, hướng dẫn xem.

Tagline gợi ý: *"Every kick. Every legend. Every nation."*

Disclaimer footer: *"Fan-operated site. Not affiliated with, endorsed, or sponsored by FIFA. All trademarks property of their respective owners."*

---

## 3. Trụ cột nội dung (Content Pillars)

### 3.1 History Hub — "The Journey 1930 → 2026"
- Timeline cuộn ngang (scrollytelling) tất cả 22 kỳ World Cup.
- Mỗi kỳ: trang chi tiết — host, format, kết quả, top scorer, golden ball, moments biểu tượng, kit, mascot, poster.
- Iconic Moments archive (text + ảnh CC): Hand of God 1986, Zidane 2006 headbutt, Maracanazo 1950…
- Data viz: số bàn thắng/trận qua các kỳ, sự tiến hóa sơ đồ chiến thuật, GDP vs thành tích.

### 3.2 Teams 2026 — 48 quốc gia
- 48 trang đội tuyển: lịch sử dự WC, kit, sân nhà, HLV, đội hình dự kiến, ngôi sao, bảng đấu, lịch thi đấu, head-to-head.
- "Star Power Index" — xếp hạng độ nổi của cầu thủ (dựa Wikipedia pageviews + caps quốc tế).
- Squad gallery dùng ảnh Wikimedia Commons có license rõ ràng.

### 3.3 Schedule & Match Center
- Lịch 104 trận, group stage → final, có **chuyển múi giờ tự động theo IP** (huge UX win).
- Mỗi trận: trang preview pre-match (form, h2h, lineups dự đoán) → live (score, events) → post-match (recap, highlight embed YouTube chính chủ FIFA).
- "Where to Watch" — geo-aware: detect IP → hiển thị broadcaster hợp pháp (Mỹ→FOX/Telemundo, UK→BBC/ITV, VN→VTV/FPT…). **Đây là tính năng affiliate vàng** (link tới streaming services có chương trình affiliate).

### 3.4 Stadiums & Host Cities (16 venues, 3 quốc gia)
- Trang riêng cho mỗi sân: lịch sử, sức chứa, các trận diễn ra, ảnh CC.
- **Host city travel guides** — SEO long-tail cực mạnh: "things to do in Dallas during World Cup", "best bars in Mexico City to watch World Cup", "AT&T Stadium parking guide". Affiliate booking.com / hotels.com / GetYourGuide.
- Bản đồ tương tác (Leaflet + OpenStreetMap — free).

### 3.5 Video Highlights cho TỪNG TRẬN (104 trận) — kiến trúc hợp pháp đầy đủ

**Nguyên tắc cốt lõi**: KHÔNG host, KHÔNG download, KHÔNG re-upload bất kỳ video nào. Chỉ **nhúng** (iframe embed) từ kênh chính chủ — điều này được YouTube/Twitter/TikTok ToS **cho phép tường minh**.

#### 3.5.1 Multi-source video aggregation (đảm bảo 100% coverage)

Mỗi trang trận có nhiều "tầng" video, fallback xuống dần:

```
Tier 1: YouTube embed — kênh broadcaster chính chủ THEO REGION user
  ├─ US English  → FOX Sports YouTube (channel: UCEgdi0XIXXZ-qJOFPf4JSKw)
  ├─ US Spanish  → Telemundo Deportes YouTube
  ├─ UK          → BBC Sport YouTube (UC11qXSWImSx5kgPm0RxhJiQ) hoặc ITV Football
  ├─ Brazil      → Globo Esporte / SporTV YouTube
  ├─ Argentina   → TyC Sports YouTube
  ├─ Mexico      → Televisa Deportes YouTube
  ├─ Spain       → RTVE Deportes YouTube
  ├─ Germany     → Sportschau (ZDF/ARD) YouTube
  ├─ France      → beIN Sports / TF1 Sport YouTube
  ├─ India       → Sony Sports YouTube
  └─ Default     → FIFA official YouTube (UCpcTrCXblq78GZrTUTLWeBw)

Tier 2: FIFA official YouTube (fallback nếu Tier 1 chưa có)

Tier 3: Twitter/X official broadcaster post embed (iframe Twitter embed)
  └─ FOX/BBC/Telemundo thường post clip ngắn 30-60s ngay sau bàn thắng

Tier 4: TikTok / Instagram Reels official embed (cho mobile-first audience)

Tier 5: Deep links "Watch on..." (button row, không embed — chỉ outbound):
  ├─ FIFA+ (fifa.com/fifaplus — official streaming, free trong WC 2026)
  ├─ FOX Sports / FoxSports.com → affiliate FuboTV nếu region-locked
  ├─ BBC iPlayer
  ├─ Telemundo Deportes
  └─ + suggest NordVPN/Surfshark affiliate nếu user ngoài region

Tier 6: "Highlights coming soon" UI + interactive text recap + SVG goal timeline
  └─ Hiển thị khi 0/5 tầng trên chưa có (60 phút đầu sau trận)
```

→ Trong số 6 tầng, **đảm bảo mọi trận luôn có ít nhất 1 tầng hiển thị** trong vòng 60 phút sau full-time.

#### 3.5.2 Tự động phát hiện video — quota-free architecture

YouTube Data API có giới hạn 10K units/ngày → không đủ search cho 104 trận × nhiều broadcaster. Giải pháp:

**Pipeline (chạy trên Cloudflare Worker)**:

1. **RSS feed polling (free, không cần API key, không quota)**:
   - Mỗi broadcaster YouTube channel có RSS public: `https://www.youtube.com/feeds/videos.xml?channel_id={ID}`
   - Worker chạy cron mỗi **5 phút khi có trận đang/vừa diễn ra**, mỗi **30 phút khi off-peak**.
   - Fetch RSS của ~15 broadcaster channels (~15 HTTP calls/lần).
   - Parse XML → lấy video mới nhất → fuzzy match title vs tên trận (vd "Argentina vs Brazil", "ARG-BRA", "Argentina Brazil highlights").

2. **Fuzzy matching algorithm**:
   - Normalize team names (alias dictionary: "USA" = "United States" = "US Men's National Team").
   - Levenshtein + token overlap.
   - Confidence score ≥ 0.7 → save to D1.

3. **Lưu D1**:
   ```sql
   CREATE TABLE match_videos (
     id INTEGER PRIMARY KEY,
     match_id TEXT,                -- '2026-06-11-MEX-vs-X'
     source TEXT,                  -- 'youtube' | 'twitter' | 'tiktok'
     channel_id TEXT,              -- 'UCpcTrCXblq78GZrTUTLWeBw'
     channel_name TEXT,            -- 'FIFA'
     video_id TEXT,                -- YouTube ID
     region TEXT,                  -- 'GLOBAL' | 'US' | 'UK' | ...
     language TEXT,                -- 'en' | 'es' | ...
     duration_seconds INTEGER,
     title TEXT,
     published_at TIMESTAMP,
     confidence REAL,
     verified BOOLEAN DEFAULT 0    -- manual override
   );
   ```

4. **Match page render**:
   - Server-side: query D1 cho match_id, ORDER BY (region match user) > (confidence) > (published_at desc).
   - Embed iframe với attributes responsive + lazy-load + privacy-enhanced mode (`youtube-nocookie.com`).
   - Multi-tab UI: "Full Highlights" / "All Goals" / "Reactions" / "Extended" — nhóm theo duration & title keywords.

5. **Manual override panel** (admin):
   - Trang `/admin/video-curation` — biên tập viên có thể chỉnh confidence thấp, pin video, hide video chất lượng kém.
   - 1 người làm 104 trận × 5 phút = ~9 giờ cho toàn giải. Có thể outsource freelancer $50-150.

#### 3.5.3 Goal-level video chunking (nâng cao)

YouTube hỗ trợ `?start=120` để jump tới giây thứ 120. Kết hợp với:

- **Event data từ API-Football** (free tier): mỗi bàn thắng có timestamp trong trận.
- **Manual timestamp curation** cho video full-highlight: 1 lần thiết lập, mỗi bàn thắng map vào giây trong video.

→ UI: timeline 90 phút với marker từng goal → click → embed jumps tới đúng đoạn đó.

**Đây là feature ESPN/BBC không có** vì họ chia mỗi clip thành video riêng. Bạn cho **một trải nghiệm "navigate trận đấu" mượt** trên cùng một player.

#### 3.5.4 Interactive Goal Timeline (luôn có, không phụ thuộc video)

Khi video chưa available (60 phút đầu sau trận) HOẶC nếu YouTube takedown:

- SVG pitch hiển thị tất cả bàn thắng với vị trí (từ API-Football events), tên cầu thủ, phút.
- Animate trajectory của bóng (data-driven, KHÔNG copy footage — đây là viz gốc của bạn → IP của bạn).
- Mini chart xG progression nếu API cung cấp.
- Text recap 300-500 từ (AI draft Claude/GPT + biên tập 5 phút).

→ **Mọi match page LUÔN có content meaningful**, video chỉ là bonus layer.

#### 3.5.5 Compliance & risk

| Hành động | Hợp pháp? | Ghi chú |
|-----------|-----------|---------|
| YouTube iframe embed | ✅ Có | YouTube ToS điều 6.A cho phép embed tường minh |
| Twitter/X iframe embed | ✅ Có | Twitter Developer Agreement cho phép |
| TikTok official embed | ✅ Có | TikTok Embed API documented |
| Linking ra fifa.com / fox.com | ✅ Có | Linking không phải vi phạm |
| Cache thumbnail của YouTube video | ⚠️ Grey | An toàn nếu link `i.ytimg.com` trực tiếp |
| Download video + tự host | ❌ Không | Vi phạm trắng trợn |
| Embed từ Streamable/random Reddit | ❌ Không | Thường là re-upload không phép → đồng phạm |
| Tự cắt GIF từ video | ❌ Không | Derivative work, copyright |
| Tự vẽ pitch viz từ data | ✅ Có | Data = facts không bản quyền; viz là IP của bạn |
| AI generate "moment card" với text "MESSI GOAL 23'" trên nền graphics gốc | ✅ Có | Original artwork |

**Nếu YouTube video bị takedown**: iframe sẽ tự hiển thị "Video unavailable" — UI của bạn detect (qua YouTube IFrame Player API event `onError`) → tự động fallback xuống tầng tiếp theo. Không có pháp lý ảnh hưởng tới bạn.

**Nếu FIFA gửi C&D về việc bạn embed**: rất hiếm xảy ra vì YouTube embed đã được FIFA cho phép (họ UPLOAD lên YouTube với embed enabled). Nhưng nếu có: gỡ embed, giữ text recap + goal timeline + outbound link. Site vẫn hoạt động.

#### 3.5.6 Bổ trợ: text recap, photo, ảnh đại diện trận

- **Text recap** 300-500 từ: AI Claude/GPT viết draft từ events JSON + biên tập 5 phút/trận. Cost ~$0.01/trận × 104 = $1 total.
- **Photo gallery**: Wikimedia Commons (post-match nếu có photographer CC upload) + Unsplash generic. **KHÔNG** dùng Getty.
- **Match cover image**: tự generate qua Satori — composite ảnh cờ 2 đội + biểu tượng sân + tỷ số. Original artwork → IP của bạn → vô tư share social.

### 3.6 Predictions & Bracket Challenge
- User account (Cloudflare D1 + Auth.js / Lucia).
- Bracket builder kéo-thả — chia sẻ qua social → viral growth loop.
- Mini-league: invite friends, leaderboard.
- Daily score prediction game — streak rewards (badge ảo).

### 3.7 Quizzes & Games (viral lever)
- Daily WC trivia (10 câu).
- "Guess the player from silhouette/career path".
- "Six degrees of World Cup" — kết nối 2 cầu thủ qua đồng đội (đồ thị từ data Wikidata).
- "Build your all-time XI".
- **Tiềm năng viral cao** — quiz format dễ share Twitter/Facebook.

### 3.8 "On This Day in World Cup History"
- Daily auto-post từ dataset OpenFootball — bàn thắng/kết quả ngày này trong các kỳ trước.
- Tự sinh trang SEO + push lên newsletter & Twitter bot.

### 3.9 Player Database
- Top ~500 cầu thủ tham dự 2026 — trang riêng từng người: tiểu sử (Wikipedia), CLB, sự nghiệp, stats WC.
- SEO long-tail mạnh: "[Player name] World Cup 2026 stats".

### 3.10 Sticker Album (Panini-inspired, digital, original art)
- Bộ sưu tập số: thu thập "sticker" (icon tự thiết kế, KHÔNG copy Panini) qua hoạt động (quiz, predict, share).
- **Gamification cực mạnh** — giữ user quay lại hằng ngày.

---

## 4. Tính năng khác biệt hóa (bổ sung so với ý user)

| # | Idea | Vì sao đáng làm |
|---|------|-----------------|
| 1 | **Where-to-Watch geo-aware** | UX vàng + affiliate revenue (streaming services) |
| 2 | **Bracket Challenge** với mini-league | Viral loop — kéo thêm user mới qua friend invites |
| 3 | **Multi-language với SEO ngách** | EN cạnh tranh khốc liệt; ES/PT/FR/VI có cửa rank top |
| 4 | **On This Day bot** (Twitter + site) | Free organic reach, daily evergreen content |
| 5 | **Host City Travel Guides** | SEO + affiliate booking/tours — high-intent traffic |
| 6 | **Sticker Album gamification** | Retention — fan quay lại mỗi ngày |
| 7 | **Six Degrees of WC Players** | Viral curiosity, unique angle không ai làm |
| 8 | **Anthem player** (PD anthems) | Engagement + share, license an toàn |
| 9 | **Data scrollytelling** (D3/Observable Plot) | Backlink magnet — báo chí trích dẫn |
| 10 | **Live match-thread chat** | Tăng time-on-site lúc cao điểm |
| 11 | **WC Predictor (AI/ELO)** | Content + backlink — viết blog "our model predicts…" |
| 12 | **Newsletter "Daily Kickoff"** | Owned audience — bảo hiểm khi SEO/social tụt |

---

## 5. Nguồn dữ liệu FREE & hợp pháp

### Dữ liệu trận đấu & đội tuyển
| Nguồn | License | Dùng cho | Lưu ý |
|-------|---------|----------|-------|
| **OpenFootball / football.json** ([github.com/openfootball](https://github.com/openfootball)) | Public Domain | Lịch sử WC, lịch 2026, đội hình | Build-time JSON, miễn phí hoàn toàn |
| **football-data.org** | Free tier (10 req/min) | Live scores, standings | Đăng ký free, attribution nhẹ |
| **API-Football** (RapidAPI) | Free 100 req/ngày | Live events, lineups, stats | Cache aggressive để không vượt quota |
| **TheSportsDB** | Free + tip | Logos, kit, sân vận động, ảnh đội | Community-edited, chất lượng khá |
| **Wikidata SPARQL** | CC0 | Player metadata, mối quan hệ | Truy vấn graph mạnh |
| **Wikipedia REST API** | CC-BY-SA | Tiểu sử cầu thủ, sự kiện | Phải attribute & share-alike |
| **FBref / StatsBomb open data** | CC-BY-NC (StatsBomb) | Stats nâng cao | NC = chỉ phi thương mại — **CẢNH BÁO**: vì site có ads nên StatsBomb không xài; FBref cho phép scraping vừa phải với attribution |

### Media (ảnh, audio)
| Nguồn | License | Dùng cho |
|-------|---------|----------|
| **Wikimedia Commons** | CC-BY-SA / CC-BY / PD | Ảnh cầu thủ, sân, lịch sử |
| **Flickr Commons** | PD / CC | Ảnh lịch sử (FIFA archive Flickr cẩn thận license từng ảnh) |
| **Unsplash / Pexels** | CC0 | Ảnh generic (cỏ sân, cờ, fan) |
| **flagcdn.com / country-flags GitHub** | Free | Cờ quốc gia SVG |
| **Wikimedia anthems** | PD | National anthems (đa số PD vì cũ) |
| **YouTube FIFA official channel** | Embed-allowed | Highlights chính chủ |
| **OpenStreetMap + Leaflet** | ODbL | Bản đồ host cities |

### Quy tắc bắt buộc
- **Mỗi ảnh CC-BY/SA phải có credit + link tới license + link tới nguồn gốc** (component `<MediaCredit>` tái sử dụng).
- **KHÔNG dùng** ảnh Getty / AP / Reuters / EPA / team official sites.
- **KHÔNG download + re-upload** YouTube — chỉ dùng `<iframe>` embed.
- **KHÔNG dùng** logo FIFA, mascot 2026 ("Maple, Zayu, Clutch"), poster chính thức.

---

## 6. Tuân thủ bản quyền & trademark (CRITICAL)

| Hạng mục | An toàn | Rủi ro | Cấm |
|----------|---------|--------|-----|
| Cụm "World Cup" trong domain/text | ✅ descriptive fair use | — | Không gắn ™ giả |
| "FIFA" trong tên | — | ⚠️ tránh trong brand chính | Không dùng "FIFA" trong logo/URL chính |
| Logo FIFA / 2026 emblem | — | — | ❌ Tuyệt đối không |
| Mascot 2026 | — | — | ❌ |
| Highlight video | YouTube embed chính chủ | — | ❌ Re-upload |
| Squad photos | Wikimedia CC | Getty | ❌ Scrape team sites |
| Stats / fixtures | ✅ Fact — không bản quyền | — | — |
| National anthems | PD recordings | — | Không bản thu thương mại |

**Action items**:
- Tự thiết kế logo (không liên quan FIFA branding).
- Footer: disclaimer rõ ràng + privacy + DMCA contact.
- Setup mailbox `dmca@worldcuphighlight.com` để xử lý takedown nhanh.
- Mọi page có ảnh → đính kèm `<MediaCredit>` block tự động.

---

## 7. Kiến trúc kỹ thuật (Astro + Cloudflare)

```
Astro 5 (App + content collections)
├─ Cloudflare Pages (hosting, free)
├─ Cloudflare D1 (SQLite — user accounts, predictions, brackets)
├─ Cloudflare KV (cache live API responses)
├─ Cloudflare Workers (scheduled cron — refresh scores, "on this day")
├─ Cloudflare R2 (lưu ảnh đã optimize, free 10GB)
├─ Cloudflare Images / Astro <Image> (responsive, AVIF/WebP)
└─ Cloudflare Turnstile (anti-bot cho forms)

Content layer
├─ Static (build-time): history 1930-2022, teams, players, stadiums  → MDX trong content collections
├─ ISR-like (Worker scheduled rebuild every 5-15 min): scores, standings
└─ Client-side (fetch in browser): live match minute-by-minute

Auth: Lucia + D1 (email + OAuth Google/Facebook)
i18n: Astro built-in (astro:i18n) — routes /en /es /pt /fr /vi
Analytics: Cloudflare Web Analytics (free, privacy-friendly) + GA4
Ads: Google AdSense (sau khi đạt traffic ngưỡng) + Ezoic (alt) + affiliate (Awin, Impact, Booking)
Newsletter: Buttondown (free 100 subs) hoặc Resend + own list
Search: Pagefind (static, free, fast)
```

**Vì sao Astro phù hợp**:
- Sinh static HTML siêu nhanh → SEO + LCP tốt → AdSense duyệt dễ.
- Islands architecture → JS chỉ load cho bracket builder, quiz, live score.
- Built-in i18n + image optimization.
- Cloudflare Pages free unlimited bandwidth — không sợ viral burst.

---

## 8. Cấu trúc URL & SEO đa ngôn ngữ

```
worldcuphighlight.com/                 → English (default, no prefix)
worldcuphighlight.com/es/              → Spanish
worldcuphighlight.com/pt/              → Portuguese
worldcuphighlight.com/fr/              → French
worldcuphighlight.com/vi/              → Vietnamese

/history/                              → Hub
/history/1950-brazil/                  → Each edition
/teams/brazil/                         → Team page
/teams/brazil/squad-2026/
/players/lionel-messi/
/matches/2026-06-11-mexico-vs-?/
/stadiums/azteca/
/cities/mexico-city/                   → Travel guide
/where-to-watch/usa/                   → Geo guides
/predict/                              → Bracket builder
/quiz/daily/
/on-this-day/                          → Daily evergreen
```

**SEO tactics**:
- `hreflang` đầy đủ giữa 5 ngôn ngữ.
- Schema.org: `SportsEvent`, `SportsTeam`, `Person`, `Place`, `FAQPage`, `BreadcrumbList`.
- OpenGraph + Twitter cards tự sinh ảnh (Satori + Cloudflare Worker).
- Sitemap XML phân theo locale.
- **Long-tail target**: "[Team] World Cup 2026 squad", "[Stadium] capacity", "where to watch [match] in [country]", "World Cup 1986 final summary".

---

## 9. Chiến lược Monetization

| Kênh | Khởi động khi | Ước tính RPM |
|------|---------------|--------------|
| **Google AdSense** | Sau ~50 trang content + 100 visits/ngày (~tuần 2) | $3-8 (global), $15-25 (US/UK trang đông) |
| **Ezoic** (alt nếu AdSense reject) | Cần 10K visits/tháng | $10-20 |
| **Affiliate booking** (Booking.com, Hotels.com, GetYourGuide) | Ngay khi có travel guides | 4-25% commission |
| **Affiliate streaming** (FuboTV, Sling, DAZN) | Where-to-Watch pages | $5-25/conversion |
| **Affiliate merchandise** (Amazon Associates — jerseys, balls) | Team pages, history pages | 1-4% |
| **Newsletter sponsorship** | Khi có 5K+ subs | $50-500/issue |

**Anti-pattern cần tránh**:
- Spam interstitial ads — AdSense ban + giết LCP.
- Auto-play video ads — chậm site, ảnh hưởng SEO.
- Quá nhiều ad slots trên mobile — fail Core Web Vitals.

---

## 10. Roadmap (4 tuần trước + trong giải + sau giải)

### Tuần 1 (11-17/05): Balanced foundation — SEO + Bracket + W2W cùng lúc
**SEO track**
- [ ] Khởi tạo repo `worldcuphighlight` (Astro 5 + TS + Tailwind + PWA plugin từ ngày 1).
- [ ] Setup Cloudflare Pages, D1, KV, R2, Workers, custom domain + SSL.
- [ ] i18n skeleton 5 ngôn ngữ + design system (logo gốc, palette, typography).
- [ ] Import OpenFootball JSON → content collections (lịch sử 1930-2022, lịch 2026).
- [ ] Trang chủ EN + 22 trang history skeleton.
- [ ] 1 cornerstone article "FIFA World Cup 2026: Complete Guide" (5000 từ, EN, viết tay).

**Viral track**
- [ ] Bracket builder UI v0 (chưa cần auth — local storage MVP).
- [ ] OG image generator (Satori + Worker) cho bracket share.

**Revenue track**
- [ ] Where-to-Watch hub stub cho 10 quốc gia top (US, MX, CA, BR, AR, UK, ES, FR, DE, VN).
- [ ] Đăng ký affiliate: Booking, NordVPN, Amazon, GetYourGuide (mất 2-7 ngày approve).

### Tuần 2 (18-24/05): Scale content + auth
**SEO track**
- [ ] 48 trang đội tuyển (auto-gen từ data, có wrapper MDX intro biên tập tay).
- [ ] 16 trang stadium + 5 host city guides ưu tiên (NYC, LA, Mexico City, Toronto, Dallas).
- [ ] Schedule page với chuyển múi giờ tự động.
- [ ] Submit AdSense (đã có ~80 indexable pages chất lượng).

**Viral track**
- [ ] Bracket → migrate qua D1 + Lucia auth (email + Google OAuth).
- [ ] Mini-league: invite link + leaderboard.
- [ ] **Sticker Album v0**: thiết kế 10 sticker đầu tiên (art style gốc, không copy Panini). Engine collect/display.

**Revenue track**
- [ ] Where-to-Watch hoàn thiện 10 quốc gia (geo-detect IP + đề xuất broadcaster + VPN affiliate).
- [ ] Affiliate link inserter component cho host city pages.

### Tuần 3 (25-31/05): H2H scale + retention + PWA
**SEO track**
- [ ] **Generator H2H pages**: build 48×47 = 2,256 trang "[Country] vs [Country] World Cup history" từ OpenFootball. 5 ngôn ngữ = 11,280 indexable pages.
- [ ] On This Day auto-post (CF Worker cron 00:00 UTC).
- [ ] Twitter/X bot On This Day.
- [ ] ICS calendar export + Print Bracket PDF endpoint.

**Viral track**
- [ ] Daily Quiz engine + 30 ngày content sẵn.
- [ ] Sticker Album: 30 sticker, unlock rules (quiz, predict, share).

**Retention track**
- [ ] **PWA hoàn thiện**: manifest, service worker, offline standings, installable.
- [ ] **Web Push subscription**: opt-in "Goal alerts cho team yêu thích". CF Worker gửi push qua VAPID.
- [ ] Newsletter setup (Buttondown) + 3 issues drafted.

### Tuần 4 (01-10/06): Live infra + Video pipeline + polish + launch
**Video highlight pipeline (CRITICAL — cho 104 trận)**:
- [ ] D1 schema `match_videos` + admin curation UI tại `/admin/video-curation`.
- [ ] CF Worker `youtube-rss-poller.ts`: poll 15 broadcaster channels (FIFA + FOX + Telemundo + BBC + ITV + Globo + SporTV + TyC + Televisa + RTVE + Sportschau + beIN + TF1 + Sony + OneFootball), cron mỗi 5 phút khi có trận / 30 phút off-peak.
- [ ] Fuzzy match algorithm (team alias dictionary + Levenshtein + token overlap) → save với confidence score.
- [ ] CF Worker `twitter-broadcaster-poller.ts` (nếu budget Twitter API có) — fallback dùng public nitter mirror nếu không.
- [ ] Match page video component: multi-tier fallback, region-aware (Cloudflare `request.cf.country`), lazy-load, `youtube-nocookie.com`, IFrame Player API error handling.
- [ ] Interactive SVG goal timeline component: data từ API-Football events → render pitch + goal markers, hoạt động luôn cả khi video chưa có.
- [ ] Goal-level timestamp linking: YouTube `?start=` param từ admin-curated mapping.
- [ ] Test fallback chain trên 5 trận mock data.

**Live score & match pages**:
- [ ] Live score infrastructure: Worker poll API-Football mỗi 60s khi có trận; KV cache; 3-tier API fallback.
- [ ] Match preview pages (104 trận, auto-gen) + lineups stub.
- [ ] Player database top 200 với Wikidata.
- [ ] **Multi-language sweep**: DeepL translate 11K pages → top 50 cornerstone biên tập tay 5 ngôn ngữ.
- [ ] Performance audit: Lighthouse 95+ mobile.
- [ ] Schema markup (SportsEvent, FAQPage, BreadcrumbList) + sitemap per locale + Search Console + Bing.
- [ ] Push notification test end-to-end iOS + Android.
- [ ] Soft launch: Reddit (r/worldcup r/soccer — contribute trước 2 tuần), Hacker News (Show HN: Bracket Builder), Product Hunt, Pinterest seed.
- [ ] Press kit PDF "WC 2026 by the Numbers" → outreach 50 nhà báo/blogger.

### Trong giải (11/06 – 19/07): Live mode
- Daily: text recap mỗi trận trong 2h sau full-time (AI draft + biên tập), update standings, embed highlight YouTube khi FIFA upload, daily quiz, bracket leaderboard update.
- Push tin nóng qua newsletter mỗi sáng "Daily Kickoff" (lịch hôm nay, kết quả hôm qua).
- Monitor RPM + tối ưu ad placements weekly.

### Hậu giải (sau 19/07)
- Final recap mega-article (link bait).
- "Champions of 2026" tribute page.
- Pivot dần sang Copa America 2027 / Euro 2028 / WC 2030 build-up — tận dụng authority đã có.

---

## 11. Cấu trúc thư mục đề xuất

```
worldcuphighlight/
├── astro.config.mjs
├── src/
│   ├── content/
│   │   ├── editions/        # 22 kỳ World Cup (MDX)
│   │   ├── teams/           # 48 đội (YAML + MDX intro)
│   │   ├── stadiums/        # 16 sân
│   │   ├── cities/          # 16 thành phố
│   │   ├── players/         # top players
│   │   ├── blog/            # long-form articles
│   │   └── on-this-day/     # daily auto entries
│   ├── data/
│   │   ├── openfootball/    # imported JSON
│   │   └── wikidata-cache/  # SPARQL results
│   ├── i18n/
│   │   ├── en.json   es.json   pt.json   fr.json   vi.json
│   ├── components/
│   │   ├── BracketBuilder.tsx       # island
│   │   ├── LiveScore.tsx            # island
│   │   ├── Quiz.tsx                 # island
│   │   ├── MediaCredit.astro        # MANDATORY for every image
│   │   ├── WhereToWatch.astro
│   │   └── TimezoneSwitcher.tsx
│   ├── layouts/
│   ├── pages/
│   │   ├── index.astro
│   │   ├── history/[edition].astro
│   │   ├── teams/[team].astro
│   │   ├── stadiums/[stadium].astro
│   │   ├── cities/[city].astro
│   │   ├── matches/[match].astro
│   │   ├── players/[player].astro
│   │   ├── where-to-watch/[country].astro
│   │   ├── predict/index.astro
│   │   ├── quiz/daily.astro
│   │   ├── on-this-day/index.astro
│   │   └── [lang]/...                # mirrored locale routes
│   └── workers/
│       ├── refresh-scores.ts         # CF Worker cron
│       ├── on-this-day-publish.ts
│       └── og-image.ts               # Satori OG generator
├── public/
│   ├── flags/                        # SVG cờ
│   ├── og-default.png
│   └── robots.txt
└── scripts/
    ├── ingest-openfootball.ts
    ├── wikidata-sync.ts
    └── lighthouse-ci.ts
```

---

## 12. Verification & launch checklist

**Trước public launch (cuối tuần 4)**:
- [ ] Lighthouse mobile: Performance ≥ 90, SEO 100, Accessibility ≥ 95.
- [ ] Core Web Vitals: LCP < 2.5s, INP < 200ms, CLS < 0.1.
- [ ] Tất cả ảnh có `<MediaCredit>` + alt text.
- [ ] hreflang valid trên 5 locale (test bằng Search Console).
- [ ] Schema markup pass Google Rich Results Test cho: SportsEvent, SportsTeam, FAQ.
- [ ] Sitemap submit + indexing trên Search Console + Bing Webmaster.
- [ ] AdSense approved & test ads served (không CLS).
- [ ] Backup D1 daily (CF cron + R2 dump).
- [ ] DMCA process + privacy policy + cookie consent (GDPR/CCPA).
- [ ] Load test: 10K req/min sustained không lỗi (k6 hoặc Artillery).
- [ ] Cloudflare Web Analytics & GA4 firing.
- [ ] Newsletter double opt-in flow tested.
- [ ] Bracket builder: tạo, save, share OG image, mini-league join — end-to-end test trên 3 trình duyệt.
- [ ] Where-to-Watch: VPN test 5 quốc gia → đúng broadcaster.
- [ ] **Video pipeline E2E test**: simulate trận kết thúc → RSS poller chạy → D1 có record → match page hiển thị embed đúng region → fallback chain hoạt động khi xóa video Tier 1 → goal timeline SVG render đúng từ data → admin UI pin/hide hoạt động.
- [ ] Test embed YouTube với `youtube-nocookie.com` không gây CLS hoặc giảm LCP > 2.5s.
- [ ] Test 10 trận lịch sử (vd WC 2022) end-to-end để verify pipeline trước khi giải bắt đầu.
- [ ] Mobile UX manual test trên iOS Safari + Android Chrome (đây là 70%+ traffic).

**Mỗi ngày trong giải**:
- Monitoring: Cloudflare alerts > 5% error rate; API quota dashboard; YouTube RSS poller success rate.
- **Video curation**: sau mỗi trận, mở `/admin/video-curation` → review video auto-matched, pin highlight tốt nhất, hide trùng lặp/chất lượng kém (5 phút/trận).
- Recap text mỗi trận xuất bản trong 90' sau full-time.
- Goal timeline SVG verify hiển thị đúng cho mọi trận (auto từ API events).
- Daily quiz publish 06:00 UTC.

**KPI tracking dashboard (Cloudflare Web Analytics + GA4)**:
- Daily/weekly visits vs mục tiêu 500K cumulative.
- AdSense + affiliate revenue vs $3K target.
- Bracket signups (proxy for viral lift).
- Push notification opt-in rate (proxy for retention).
- Top 20 pages contributing traffic — double down weekly.

**Milestone checkpoints**:
| Mốc | Visits cumulative | Revenue cumulative | Hành động nếu lệch |
|-----|-------------------|--------------------|--------------------|
| 11/06 (khai mạc) | 30K | $50 | Nếu < 10K: tăng cường outreach Reddit/HN, mua $200 promote tweet |
| 25/06 (cuối vòng bảng) | 150K | $700 | Nếu < 80K: ưu tiên Pinterest + IG visual content |
| 09/07 (bán kết) | 350K | $2K | Nếu < 200K: tăng email frequency, A/B test ad placements |
| 19/07 (chung kết) | 500K | $3K | — |

---

## 13. Rủi ro & mitigations

| Rủi ro | Mức | Mitigation |
|--------|-----|------------|
| FIFA gửi cease & desist vì domain | Trung bình | Disclaimer mạnh, không dùng logo/mascot, sẵn sàng đổi tagline; tham vấn luật sư nếu site lớn nhanh |
| AdSense reject vì content "thin"/copy | Trung bình | Viết tay 30+ bài cornerstone trước khi apply; tránh AI-generated thuần túy |
| API live score vượt quota lúc cao điểm | Cao | KV cache aggressive (60s); fallback 2-3 API; circuit breaker |
| Server quá tải khi viral | Thấp (CF Pages auto-scale) | Static-first, edge cache, R2 cho ảnh |
| StatsBomb NC license vi phạm | Trung bình | KHÔNG dùng StatsBomb open data trên site có ads; chỉ dùng OpenFootball + FBref (attribute) |
| Wikimedia ảnh bị attribute sai | Trung bình | Component `<MediaCredit>` bắt buộc; script CI verify metadata |
| Newsletter spam GDPR | Trung bình | Double opt-in, unsubscribe 1-click, lưu IP+timestamp |
| Bracket builder bị abuse (spam account) | Thấp | Turnstile + rate limit + email verify |

---

## 14. Quick wins ngay tuần đầu (nếu bạn cần kết quả nhanh)

1. **Cornerstone article** "FIFA World Cup 2026: Complete Guide (Schedule, Teams, Cities, How to Watch)" — 5000 từ, target keyword chính, internal link hub.
2. **22 trang history** auto-gen từ OpenFootball — instant 22 indexable pages.
3. **48 trang đội tuyển** stub — instant 48 pages × 5 ngôn ngữ = 240 pages.
4. **Twitter/X bot "On This Day"** — free organic reach, 1 post/ngày sẽ tích lũy follow.
5. **Reddit + Discord seed** — share bracket builder ở r/worldcup khi gần ngày khai mạc (tránh self-promo rule bằng cách contribute thật trước).

---

## 15. Bước tiếp theo (sau khi plan được duyệt)

1. Quyết định: dựng site **trong repo `cronboard` này** (branch `claude/worldcup-website-planning-YfIIY`) hay tạo **repo mới** `worldcuphighlight` (khuyến nghị tạo repo mới vì Cronboard là Python tool không liên quan).
2. Scaffold Astro project + Cloudflare Pages.
3. Mua/cấu hình email cho `@worldcuphighlight.com` (DMCA + newsletter sender).
4. Apply AdSense ngay khi có 30+ pages.
5. Bắt đầu Tuần 1 todos.

---

## 15.5 Tiến độ thực thi (Implementation progress) — 11/05/2026

Site được scaffold ở thư mục riêng `/home/user/worldcuphighlight/` (chưa init git để user tự `gh repo create`). Build verified: **1,367 static pages**.

| Hạng mục | Trạng thái | Ghi chú |
|----------|------------|---------|
| Astro 5 + Tailwind 3 + MDX skeleton | ✅ Done | TypeScript strict + `~/*` alias |
| i18n 5 ngôn ngữ (en/es/pt/fr/vi) | ✅ Done | `astro:i18n` + hreflang đầy đủ |
| BaseLayout (canonical, OG, JSON-LD) | ✅ Done | Default OG = original SVG art |
| Header / Footer / MediaCredit / TimezoneKickoff / NewsletterSignup | ✅ Done | Footer mở rộng 4 cột với newsletter inline |
| 22 trang lịch sử (1930–2022) | ✅ Done | Auto-gen từ `scripts/ingest-openfootball.ts` |
| Cornerstone article 2026 Complete Guide | ✅ Done | 3,400+ từ |
| Golden Boot cluster (22 + index) | ✅ Done | `Person`/award JSON-LD |
| Where-to-Watch (20 quốc gia) | ✅ Done | `FAQPage` schema + affiliate hooks |
| **48 trang đội tuyển** | ✅ Done | `src/data/teams.ts` + `SportsTeam` schema |
| **16 trang stadium** | ✅ Done | `src/data/stadiums.ts` + `StadiumOrArena` schema |
| **Schedule page + timezone switcher** | ✅ Done | 12 groups + headline kickoffs auto local-time |
| **Host city travel guides (đủ 16 thành phố)** | ✅ Done | NYC, LA, MC, Toronto, Dallas, Atl, Phi, Mia, Bos, Hou, KC, SF, Sea, Van, GDL, MTY |
| About / Privacy / DMCA / Contact / 404 | ✅ Done | AdSense gate unblocked |
| robots.txt + sitemap-index | ✅ Done | |
| **H2H pages generator (1,128 trang)** | ✅ Done | Canonical slug alphabet hoá, `SportsEvent` JSON-LD, marquee hub tại `/h2h/` |
| **Bracket Builder v0** | ✅ Done | 12 groups → R32 → Final, localStorage + share URL hash |
| **PWA (manifest + icons + service worker)** | ✅ Done | Network-first navigation + SWR `/_astro/` + `/offline/` fallback |
| **Daily Quiz engine + 20 ngày × 10 questions** | ✅ Done | `Quiz` JSON-LD, click-to-reveal, sticker reward khi đạt 8/10+ |
| **On This Day generator** | ✅ Done | 52 dates × ~70 events từ 1930 → 2022 |
| **Sticker Album v0 (22 sticker)** | ✅ Done | Sticker gốc (inline SVG), collect engine từ quiz/bracket/city visits |
| **Newsletter signup (Buttondown embed)** | ✅ Done | Footer + About; placeholder username, thay khi mở account |
| **ICS calendar export (`.ics` endpoint)** | ✅ Done | `/calendar/` + `/calendar/headlines.ics` cho Apple/Google/Outlook |
| **Print-Your-Bracket** | ✅ Done | `/predict/print/` — single-page A4 landscape printable |
| **Press kit "WC 2026 by the Numbers"** | ✅ Done | `/press/by-the-numbers/` — free data pack cho nhà báo |
| **RSS feed (blog + quizzes + On This Day)** | ✅ Done | `/rss.xml` — top 80 items |
| **Social share component** | ✅ Done | X/FB/WhatsApp/Reddit/LinkedIn/Copy trên team/edition/blog pages |
| AdSense application | ⏳ Pending | 1,367 pages — apply ngay |
| On This Day scale-out (~310 ngày còn lại) | ⏳ Pending | Engine ready, chỉ thiếu data |
| Daily Quiz scale-out (10+ ngày tiếp) | ⏳ Pending | Engine ready, chỉ thiếu data |
| Web Push opt-in (goal alerts) | ⏳ Pending | Cần VAPID + Worker (sau khi switch CF adapter) |
| Bracket → D1 + Lucia auth | ⏳ Pending | Cần CF adapter + SSR mode |
| Video pipeline + live score | ⏳ Pending | Tuần 4 |

---

# APPENDIX A — Mở rộng brainstorm (creative arsenal)

## A1. Tính năng "đào vàng" SEO ít cạnh tranh

| Ý tưởng | Vì sao mạnh | Volume keyword (ước) |
|---------|-------------|------------------------|
| **"Country vs Country" H2H pages** | 48 đội × 47 đối thủ = 2,256 pages tự động. Mỗi page tổng hợp toàn bộ lịch sử đối đầu (data có sẵn trong OpenFootball). | "argentina vs brazil world cup history" ~ 5K/tháng — nhân lên 2K pages = đào vàng |
| **"[Player name] World Cup goals" pages** | Top 500 cầu thủ — mỗi người 1 page liệt kê toàn bộ bàn thắng + video embed | Long-tail vô tận |
| **"World Cup [Year] [Team]" pages** | 22 kỳ × ~16-48 đội tham gia = ~600 pages | "world cup 1986 argentina squad" ~ 2K/tháng |
| **"Top scorer World Cup [Year]"** | 22 pages | Evergreen |
| **"World Cup final [Year] highlights"** | 21 pages (chưa có 2026) | High intent |
| **"Goal of the tournament [Year]"** | 22 pages | Niche nhưng dễ rank |
| **"World Cup kits [Year] [Team]"** | Visual gallery, Wikimedia ảnh | Pinterest traffic |
| **"World Cup mascot [Year]"** | Lịch sử mascot từ 1966 | Quirky, share well |
| **"Stadium [Name] World Cup matches"** | 16 venues 2026 + lịch sử | Local SEO |

→ **Tổng cộng có thể tự sinh ~3,500 indexable pages chất lượng** chỉ từ OpenFootball + Wikidata. Mỗi page bring 5-50 visit/tháng = tens of thousands traffic ngay khi index xong.

## A2. Tính năng viral & UGC

1. **"World Cup of Anything" polls** — dùng bracket UI để vote: best kit ever, best goal, best song, best mascot, best stadium. Mỗi tuần 1 chủ đề. Twitter/X share format gọn.
2. **Goal of the Day voting** — community vote, kết quả công bố sáng hôm sau → ngày hôm sau viết bài "Goal of the Day: …" = daily evergreen.
3. **Print-Your-Bracket PDF generator** — high search volume "world cup bracket printable", super dễ làm bằng PDFKit + Astro endpoint. Funnel xuống Bracket Challenge online.
4. **ICS calendar export** — `/calendar/team-brazil.ics`, `/calendar/all-matches.ics`. Fan thêm vào Google/Apple Calendar = sticky.
5. **"Six Degrees of World Cup Players"** — graph game trên Wikidata. Viral curiosity.
6. **Anthem 5-second quiz** — audio quiz, super shareable.
7. **AI Manager Game** — pick formation, AI simulates match. Lightweight.
8. **Live Hashtag Pulse widget** — real-time fan sentiment per team — broadcasters có thể nhúng = backlink xịn.
9. **Mascot Museum** — 1 page tổng hợp 14 mascot từ 1966. Visual content viral Pinterest/IG.
10. **"My Bracket vs AI"** — head-to-head với ELO predictor model. Engagement loop.

## A3. Công cụ "hữu dụng" (utility = retention)

1. **Group Stage Scenarios Calculator** — input current standings, hiển thị mọi kịch bản advance (tiebreaker rules: points → GD → goals → H2H). Search spike khi gần cuối vòng bảng.
2. **Knockout Simulator** — drag your predicted winners → see who you face in Final.
3. **Where-to-Watch wizard** — chọn country + match → broadcaster + streaming link + đề xuất VPN affiliate nếu region-locked.
4. **Watch Party Finder map** — crowdsource bar/cafe có chiếu trận, đặt bàn qua OpenTable/Resy affiliate. Có thể seed bằng OSM places.
5. **WhatsApp/Telegram score bot** — fan subscribe → push notify khi đội yêu thích vào sân/ghi bàn. Owned channel cực sticky ở LATAM/SEA.
6. **Time zone converter** — đặc biệt cho fan VN/Á/Phi: hiển thị giờ địa phương cho mỗi trận, có quick share "trận này 02:00 sáng giờ VN".
7. **PWA installable** — push notification, offline standings. Manifest + Service Worker đơn giản với Astro.

## A4. Content angles độc đáo

1. **Underdog stories** long-form — Costa Rica 2014, Morocco 2022, đội bóng "Cinderella" 2026. Backlink magnet vì báo lớn cũng xài.
2. **"What if?" historical simulators** — re-run 1950 Brazil vs Uruguay với analytics hiện đại. Geek content.
3. **Tactical evolution timeline** — WM → 4-2-4 → catenaccio → tiki-taka → gegenpressing. Data viz + ảnh minh họa.
4. **Coaches profiles** — ít người làm, dễ rank.
5. **Referees database** — VAR controversies, trang phục, lịch sử. Niche.
6. **Iconic kit redesign** — designer hire $50-200/kit để tạo concept reimagining classic kits → Pinterest gold.
7. **Trophy lore** — Jules Rimet → Coupe du Monde → lịch sử ăn trộm/bị mất.
8. **WC Songs archive** — Waka Waka, La Copa de la Vida — embed YouTube + lyrics + bối cảnh sáng tác.
9. **"On this day, X years ago"** — daily evergreen, Twitter bot.
10. **Refugee/diaspora players** — tay vợt câu chuyện cá nhân (Modric chiến tranh Croatia, Mbappe gốc Cameroon-Algeria, etc.).

## A5. Distribution & growth hack

| Kênh | Chiến thuật | Cost |
|------|-------------|------|
| **Twitter/X bot** | On This Day + Goal recap auto-post + bracket leaderboard updates | $0 |
| **Pinterest** | Kit galleries, mascot, infographic — Pinterest còn drive nhiều traffic | $0 |
| **TikTok/Reels** | Auto-render bằng Remotion (React video) — daily highlight summary text+stills | $0-20/m |
| **YouTube Shorts** | Auto-gen video từ data viz | $0 |
| **Reddit** | Contribute real ở r/worldcup r/soccer 2-3 tuần trước rồi share công cụ | $0 |
| **Hacker News** | "Show HN: I built a bracket builder for World Cup 2026" | $0 |
| **Product Hunt** | Launch predictor / bracket tool | $0 |
| **Wikipedia citations** | Tạo data viz CC-licensed → editor Wikipedia có thể cite | $0 |
| **Press kit** | Free "WC 2026 by the numbers" PDF cho nhà báo → backlinks | $0 |
| **Google News inclusion** | Cần 100+ original articles, About page, named authors, RSS | $0 |
| **Email outreach** | Football blog/podcast nhỏ — guest post trao đổi backlink | Time |
| **Discord** | Tạo Discord cho bracket league — community moat | $0 |
| **Influencer micro-deal** | Fan YouTuber 5-50K sub: custom dashboard cho video của họ → mention | $0-100 |

## A6. Monetization stack chi tiết

### Affiliate program đề xuất (đăng ký ngay)

| Vertical | Program | Commission | Approval khó? |
|----------|---------|-----------|--------------|
| Hotels | Booking.com Partner | 25-40% of Booking's commission (~4% gross) | Dễ |
| Hotels | Hotels.com (qua Awin) | 3-6% | Trung bình |
| Tours/Tickets | GetYourGuide Partner | 8% | Dễ |
| Tours | Viator (qua Awin) | 6-8% | Trung bình |
| Streaming US | FuboTV (qua Impact) | ~$30/conv | Trung bình |
| Streaming US | Sling TV (qua Impact) | $15/conv | Trung bình |
| VPN | NordVPN | 40% first / 30% renew | Dễ |
| VPN | Surfshark | $20-40/conv | Dễ |
| Merch | Amazon Associates | 1-4.5% | Dễ |
| Merch retro | Classic Football Shirts | 5-7% | Dễ |
| Custom merch | Printful + own store | 30-50% margin | Dễ |
| Travel insurance | World Nomads | 10% | Dễ |
| Flights | Skyscanner / WayAway | 50% of their commission | Dễ |
| Sports books | bet365 / DraftKings | $25-100/conv | Hạn chế khu vực, cần KYC |

### Display ads

- **AdSense** — RPM toàn cầu $3-8, US/UK $15-25. Apply ngay khi 30+ pages chất lượng.
- **Ezoic** — RPM $10-20, không cần traffic tối thiểu (nay), AI optimize.
- **Mediavine** — RPM $25-40 nhưng cần 50K sessions/tháng → mục tiêu phase 2.
- **AdThrive (Raptive)** — RPM $30-50 nhưng 100K sessions/tháng → mục tiêu.

### Premium / B2B

- **Ad-free $4.99/m** — chấp nhận thấp nhưng signals product-market fit.
- **Custom company bracket pools** — $99-499/pool, B2B email outreach.
- **Newsletter sponsorship** — $50-500/issue khi 5K+ subs.
- **Sponsored "host city guides"** — local breweries/bars $100-300.

## A7. Solutions kỹ thuật cụ thể cho vấn đề thực tế

| Vấn đề | Giải pháp |
|--------|----------|
| Dịch 5 ngôn ngữ với budget = 0 | **DeepL API free** (500K chars/month) + **content collections hooks** auto-translate ở build time; biên tập tay top 50 cornerstone pages |
| Bandwidth ảnh | Cloudflare Polish (auto AVIF/WebP) + R2 storage ($0/10GB) + Astro `<Image>` |
| Live score quota giới hạn | **Cascade 3 APIs**: football-data.org (primary) → API-Football fallback → TheSportsDB → scrape FBref. KV cache 60s. Circuit breaker khi 429 |
| OG image cho 3500+ pages | **Satori + @vercel/og** trên CF Worker — render PNG dynamic từ JSX template per slug |
| Search trong site | **Pagefind** — static index build-time, ~1MB, zero infra |
| Auth + DB ở edge | **Lucia + Cloudflare D1** (SQLite ở edge, free 5GB) |
| Newsletter free tier | **Buttondown** 100 subs free → **Resend** $20/m khi vượt |
| Cron jobs | **CF Worker scheduled triggers** (free 1000/day) — refresh scores, publish "on this day" |
| Time zone | **luxon** + Cloudflare `request.cf.timezone` để detect, prompt user override |
| Anti-bot cho bracket form | **Cloudflare Turnstile** (free, không CAPTCHA UX tệ) |
| Performance budget | Astro static + islands + zero-JS pages mặc định; chỉ hydrate bracket/quiz/live score |
| i18n routing | `astro:i18n` v5 — defaultLocale "en" no prefix, others prefixed; sitemap per locale |
| Multi-currency hiển thị giá hotel | Open Exchange Rates free tier 1000 req/tháng, cache 24h |
| AI text recap | Claude/GPT API generate draft → biên tập tay 5 phút/trận; cost ~$0.01/trận × 104 = $1 cho cả giải |
| Image attribution tự động | Build script đọc YAML frontmatter của ảnh → render `<MediaCredit>` block |
| DMCA workflow | Form trên site → email tới `dmca@` → 48h response SLA → log vào D1 |
| Cookie consent (GDPR) | **CookieYes** free tier hoặc roll-own component |

## A8. Idea "moonshot" — high risk / high reward

1. **AI-generated podcast hằng ngày** — 5 phút, voice ElevenLabs hoặc Coqui TTS, recap & preview. Distribute Spotify/Apple. Cost $10-30/m. Owned audio channel khi mọi người mệt content text.
2. **AR sticker hunt ở 16 host cities** — QR codes ẩn ở landmark, scan → unlock sticker. Marketing stunt — báo địa phương đưa tin = free PR.
3. **Native iOS/Android via Capacitor** — wrap PWA, submit App Store. Push notification = retention. ~$100/y Apple Dev account.
4. **Live match-thread chat** — Durable Objects + websockets. Twitch-style cho từng trận. Stickiness +++.
5. **Twitter/X paid promoted hashtag campaign** — $500-2K ngân sách quanh ngày khai mạc cho 1 viral tweet (vd bracket challenge).
6. **Collab với 1 ex-pro** — fee $500-3K cho 1 podcast/blog AMA → backlink + PR.
7. **Branded swag store** (Printful) — t-shirt theo team, sticker, hat. Margin 30-50%.

---

# APPENDIX B — Top 10 "vũ khí" ưu tiên (nếu chỉ chọn 10)

Đây là shortlist tôi đề xuất nếu bạn muốn tối đa ROI trong 1 tháng:

1. ⭐ **History Hub + 22 trang kỳ World Cup** — foundation SEO + AdSense approval.
2. ⭐ **48 trang Team + ~600 trang [Year][Team]** — content scale tự động từ data.
3. ⭐ **2,256 trang H2H "[Country] vs [Country]"** — keyword vàng, tự gen.
4. ⭐ **Where-to-Watch wizard + VPN affiliate** — high-intent traffic & revenue.
5. ⭐ **Bracket Challenge + mini-league** — viral loop + email capture.
6. ⭐ **Host City Travel Guides x 16** — affiliate booking/tours.
7. ⭐ **Daily Quiz + Sticker Album** — retention engine.
8. ⭐ **On This Day bot + cornerstone scrollytelling article** — daily content + backlink magnet.
9. ⭐ **Newsletter "Daily Kickoff"** — bảo hiểm khi SEO/social biến động.
10. ⭐ **ICS calendar + Print bracket PDF** — high-search utility, dễ làm.
