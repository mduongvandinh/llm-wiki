# LLM Wiki Variants — Phân tích Top 5 Use Cases

> Mục đích: Đánh giá chi tiết 5 use case có tiềm năng cao nhất để quyết định thứ tự build.
> Ngày: 2026-04-14

---

## Scoring Framework

| Tiêu chí | Mô tả | Trọng số |
|----------|-------|----------|
| **Market size** | Số người có pain point này | x3 |
| **Pain intensity** | Đau đến mức nào? Đang dùng giải pháp tệ? | x3 |
| **Viral potential** | Dễ chia sẻ, demo, gây wow? | x2 |
| **Build effort** | Ít customization = điểm cao | x2 |
| **Community fit** | GitHub/HN/Reddit audience có share không? | x2 |
| **Monetization** | Có thể turn thành SaaS sau không? | x1 |

**Tổng điểm tối đa: 130**

---

## #1 — Job Search Intel Wiki

### Tổng quan
Người đi xin việc research hàng chục công ty, lưu JD, ghi chú interview, track follow-ups — hiện dùng Google Sheets, Notion, hoặc đơn giản là không có hệ thống. Sau mỗi lần apply, kiến thức về công ty đó biến mất.

### Target audience
- Junior đến senior đang tìm việc (xảy ra 3-7 lần trong đời)
- Headhunter / recruiter theo dõi nhiều công ty
- Career switcher cần research sâu về industry mới

### Market size
- ~70 triệu người tìm việc mỗi năm chỉ riêng US
- Việt Nam: hàng triệu người mỗi năm (LinkedIn VN tăng mạnh)
- **Score: 9/10**

### Giải pháp hiện tại & nhược điểm
| Giải pháp | Nhược điểm |
|-----------|-----------|
| Google Sheets | Không có context, không synthesize |
| Notion job tracker | Manual, không tự discover news về công ty |
| LinkedIn | Chỉ tracking, không tổng hợp kiến thức |
| Brain | Quên sau 2 tuần |

### Fit với llm-wiki

**Cần thêm:**
- Entity schema cho `Company`, `Role`, `Interviewer`
- Feed config: track LinkedIn Jobs, Glassdoor, news về target companies
- Command mới: `/llm-wiki interview-prep "Company Name"` → tổng hợp mọi thứ đã biết về công ty thành 1-pager trước interview
- Pain-rank variant: `offer-rank` để so sánh multiple offers

**Không cần thêm:**
- Core ingest/query/lint/discover đã hoạt động
- Chỉ cần config.yaml template phù hợp

**Config.yaml thay đổi:**
```yaml
wiki:
  name: "Job Search Intel"
  schema_extensions:
    - company  # entity type mới
    - role
    - interviewer

topics:
  - name: "Target Company Research"
    keywords: ["company culture", "engineering culture", "layoffs", "funding"]
  - name: "Role Research"
    keywords: ["software engineer salary", "product manager career"]

feeds:
  reddit:
    subreddits_pain_points:
      - sub: "r/cscareerquestions"
      - sub: "r/jobs"
  hackernews:
    keywords: ["hiring", "layoffs", "engineering culture"]
```

### Demo potential
- Người dùng thấy ngay: drop 5 JD vào raw/ → wiki tự tạo comparison table giữa các công ty
- Killer feature: trước interview, hỏi `/llm-wiki query "Tell me everything about Stripe"` → ra 1-pager chuẩn bị
- Dễ screenshot, dễ share trên LinkedIn, Reddit r/cscareerquestions

### Viral potential
- LinkedIn: rất cao — post "I built a personal wiki to track job search" sẽ viral
- Reddit r/cscareerquestions: active community, pain point rõ
- **Score: 8/10**

### Build effort
- Config.yaml template: 1-2 giờ
- Command `interview-prep`: thêm vào SKILL.md ~1 giờ
- Sample data: tạo fake 5 companies, 3 JDs → demo sẵn
- **Score: 9/10** (ít phải thay đổi engine)

### Monetization path
- SaaS: $9/month "AI job tracker that thinks for you"
- B2B: sell to career coaches, bootcamps

### Tổng điểm
| Tiêu chí | Điểm | x Trọng số | Tổng |
|----------|------|-----------|------|
| Market size | 9 | x3 | 27 |
| Pain intensity | 8 | x3 | 24 |
| Viral potential | 8 | x2 | 16 |
| Build effort | 9 | x2 | 18 |
| Community fit | 8 | x2 | 16 |
| Monetization | 7 | x1 | 7 |
| **TOTAL** | | | **108/130** |

---

## #2 — Competitive Intelligence Wiki

### Tổng quan
Mọi startup, product manager, founder đều cần theo dõi competitors — nhưng thường làm thủ công (Google Alerts, bookmark, Notion page không ai cập nhật). Wiki tự động discover khi competitor ra feature mới, thay đổi pricing, hire hoặc layoff.

### Target audience
- Startup founders & product managers (B2B/B2C)
- Marketing teams track competitor messaging
- VCs track portfolio companies + competitors
- Analysts

### Market size
- Mọi công ty có > 1 competitor đều cần (= hầu hết công ty)
- ~33 triệu SMB tại US, hàng triệu startup toàn cầu
- **Score: 9/10**

### Giải pháp hiện tại & nhược điểm
| Giải pháp | Nhược điểm |
|-----------|-----------|
| Crayon / Klue | $$$, enterprise only, không phải AI-native |
| Google Alerts | Noise cao, không synthesize |
| Manual Notion | Không tự update, decay nhanh |
| ChatGPT search | Không persistent, không cross-reference |

### Fit với llm-wiki

**Cần thêm:**
- Entity schema cho `Competitor`, `Product`, `Pricing`, `Feature`
- Feed: track competitor websites (WebFetch diff), GitHub repos, job postings, ProductHunt, G2 reviews
- Command mới: `/llm-wiki competitive-brief "CompanyName"` → battlecard format
- Lint feature đặc biệt: **change detection** — so sánh state cũ vs mới → alert khi có thay đổi quan trọng

**Config.yaml thay đổi:**
```yaml
wiki:
  name: "Competitive Intel"

topics:
  - name: "Competitor Tracking"
    competitors:
      - name: "Linear"
        website: "https://linear.app"
        github: "linear"
        track: ["pricing", "features", "jobs", "funding"]

feeds:
  github_orgs:
    - org: "competitor-org"
      watch: ["new_repos", "releases"]
  product_hunt:
    enabled: true
    keywords: ["project management", "productivity"]
```

### Demo potential
- Killer demo: drop 3 competitor URLs → wiki tự crawl → tạo comparison table features/pricing/positioning
- "My wiki noticed Linear changed their pricing before their blog post" → viral tweet
- Dễ demo bằng video ngắn

### Viral potential
- Twitter/X: founder & PM community rất active, sẽ share
- ProductHunt: có thể submit như tool
- **Score: 9/10**

### Build effort
- Change detection logic: cần thêm ~2-3 giờ vào discover command
- competitor-brief command: 1-2 giờ
- Sample data: 3 fake competitors → demo
- **Score: 7/10** (cần thêm change detection)

### Monetization path
- SaaS tier: $29-49/month (B2B willing to pay)
- Highest monetization potential trong 5 cái
- Có thể pivot thành full product

### Tổng điểm
| Tiêu chí | Điểm | x Trọng số | Tổng |
|----------|------|-----------|------|
| Market size | 9 | x3 | 27 |
| Pain intensity | 9 | x3 | 27 |
| Viral potential | 9 | x2 | 18 |
| Build effort | 7 | x2 | 14 |
| Community fit | 8 | x2 | 16 |
| Monetization | 10 | x1 | 10 |
| **TOTAL** | | | **112/130** |

---

## #3 — Academic Literature Review Wiki

### Tổng quan
PhD students, researchers, và lifelong learners đọc hàng trăm papers — dùng Zotero để lưu citations, nhưng không có cách nào tổng hợp, phát hiện contradictions giữa studies, hoặc hỏi "điều gì đã được chứng minh chắc chắn về X?"

### Target audience
- PhD students (đặc biệt năm 2-4 khi đang literature review)
- Independent researchers
- Knowledge workers đọc nhiều reports (analyst, consultant)
- Curious people theo dõi một field

### Market size
- ~7 triệu PhD students toàn cầu
- Hàng chục triệu academics/researchers
- **Score: 7/10** (niche hơn 2 cái trên)

### Giải pháp hiện tại & nhược điểm
| Giải pháp | Nhược điểm |
|-----------|-----------|
| Zotero | Chỉ citation management, không synthesize |
| Notion/Obsidian | Manual hoàn toàn |
| Elicit / Consensus | Chỉ search, không build wiki |
| ChatGPT | Không persistent, hallucinate citations |

### Fit với llm-wiki

**Cần thêm:**
- PDF ingest support (đọc arxiv papers, PDFs)
- Entity schema cho `Author`, `Study`, `Claim`, `Methodology`
- Lint đặc biệt: **contradiction detection** giữa các studies ("Study A claims X, Study B claims ¬X")
- Citation graph: visualize connections giữa papers
- Command: `/llm-wiki literature-map "topic"` → tóm tắt state of the field

**Config.yaml thay đổi:**
```yaml
wiki:
  name: "Literature Review — [Topic]"
  schema_extensions:
    - study
    - claim
    - author

feeds:
  arxiv:
    enabled: true
    categories: ["cs.AI", "cs.LG"]
    keywords: ["your research topic"]
  semantic_scholar:
    enabled: true
```

### Demo potential
- "Drop 20 PDFs → wiki maps contradictions between studies" → impressive demo cho academic Twitter
- Niche nhưng passionate community
- **Score: 6/10** (ít người relate hơn)

### Viral potential
- Academic Twitter: có thể viral trong niche
- HN: researchers trên HN sẽ upvote
- **Score: 6/10**

### Build effort
- PDF reading: cần test khả năng Claude đọc PDF (đã có trong tools)
- literature-map command: 2-3 giờ
- Contradiction detection: đã có một phần trong lint
- **Score: 7/10**

### Monetization path
- Khó monetize trực tiếp (academics nghèo + cheap)
- Nhưng là gateway vào enterprise research tools

### Tổng điểm
| Tiêu chí | Điểm | x Trọng số | Tổng |
|----------|------|-----------|------|
| Market size | 7 | x3 | 21 |
| Pain intensity | 9 | x3 | 27 |
| Viral potential | 6 | x2 | 12 |
| Build effort | 7 | x2 | 14 |
| Community fit | 7 | x2 | 14 |
| Monetization | 4 | x1 | 4 |
| **TOTAL** | | | **92/130** |

---

## #4 — Chronic Condition Management Wiki

### Tổng quan
Người có bệnh mãn tính (tiểu đường, huyết áp, lupus, IBS, ADHD...) phải tự quản lý thông tin từ nhiều nguồn: kết quả xét nghiệm, ghi chú bác sĩ, bài viết y tế, nhật ký triệu chứng. Hiện tại tất cả scattered. Wiki giúp tổng hợp, phát hiện patterns, và có context đầy đủ khi đi khám.

### Target audience
- ~133 triệu người Mỹ có ít nhất 1 bệnh mãn tính
- Caregiver chăm sóc người thân (bố mẹ già, con em bệnh hiếm)
- Health-conscious người muốn track wellness data

### Market size
- 40%+ dân số các nước phát triển
- **Score: 10/10**

### Giải pháp hiện tại & nhược điểm
| Giải pháp | Nhược điểm |
|-----------|-----------|
| Apple Health / Samsung Health | Chỉ metrics, không synthesize với y văn |
| MyChart / hospital apps | Siloed, không integrates với research |
| Notion health tracker | Manual, không discover guidelines mới |
| Memory | Quên, không có patterns |

### Fit với llm-wiki

**Cần thêm:**
- Entity schema cho `Symptom`, `Medication`, `Lab Result`, `Doctor`
- Ingest support: PDF lab results, medical records
- Strict **privacy mode**: không discover từ internet (chỉ ingest local files)
- Lint đặc biệt: **timeline analysis** — "symptom X tệ hơn sau khi bắt đầu medication Y?"
- Command: `/llm-wiki health-brief` → tóm tắt 1-pager cho buổi khám tiếp theo

**QUAN TRỌNG — Privacy concerns:**
- Health data là sensitive nhất
- Local-only: không upload lên cloud, không external API calls
- Cần rõ ràng trong README: "Runs 100% locally via Claude Code"

**Config.yaml thay đổi:**
```yaml
wiki:
  name: "Personal Health Wiki"
  privacy_mode: strict  # no web discovery, local only

discovery:
  strategies:
    - gap_fill  # only search for medical info when explicitly asked
  medical_disclaimer: true
```

### Demo potential
- Khó demo publicly (privacy concern)
- Demo ẩn danh: fake patient data → show pattern detection
- Caregiver use case dễ relate hơn: "I built this to track my dad's diabetes"
- **Score: 5/10** (khó demo, sensitive topic)

### Viral potential
- Health communities: strong but private (Facebook groups, không public)
- Reddit r/ChronicIllness, r/diabetes: active nhưng không viral theo nghĩa tech
- **Score: 5/10**

### Build effort
- Privacy mode: thêm config flag, disable web discovery
- health-brief command: 2 giờ
- Medical disclaimer UI: 1 giờ
- **Score: 8/10** (chủ yếu là config + 1 command mới)

### Monetization path
- Highest willingness to pay (health = priority)
- $15-25/month subscriptions khả thi
- Nhưng regulatory risk (FDA, HIPAA nếu làm SaaS)

### Tổng điểm
| Tiêu chí | Điểm | x Trọng số | Tổng |
|----------|------|-----------|------|
| Market size | 10 | x3 | 30 |
| Pain intensity | 9 | x3 | 27 |
| Viral potential | 5 | x2 | 10 |
| Build effort | 8 | x2 | 16 |
| Community fit | 5 | x2 | 10 |
| Monetization | 7 | x1 | 7 |
| **TOTAL** | | | **100/130** |

---

## #5 — Book Companion Wiki

### Tổng quan
Người đọc sách dài — tiểu thuyết phức tạp (Dune, ASOIAF, Mistborn), non-fiction chuyên sâu, hoặc series nhiều tập — mất track nhân vật, timeline, foreshadowing, themes. Wiki tự build như một fan wiki cá nhân khi bạn đọc từng chương.

### Target audience
- Độc giả nghiêm túc (average book reader đọc 12-15 sách/năm)
- Book club members (cần shared discussion context)
- Non-fiction learners muốn retain knowledge sau khi đọc
- Students đọc academic texts

### Market size
- ~40 triệu avid readers ở US
- Book community online rất lớn (BookTok, Goodreads, r/books)
- **Score: 8/10**

### Giải pháp hiện tại & nhược điểm
| Giải pháp | Nhược điểm |
|-----------|-----------|
| Fandom.com wikis | Chỉ có cho bestsellers, không cá nhân hóa |
| Notion book notes | Manual, không có relationships |
| Highlights trong Kindle | Passive, không synthesize |
| Memory | 80% bị quên sau 1 tuần |

### Fit với llm-wiki

**Cần thêm:**
- Entity schema cho `Character`, `Location`, `Event`, `Theme`, `Quote`
- Ingest workflow đặc biệt: drop text per chapter → wiki cập nhật dần (không cần đọc hết sách mới ingest)
- Spoiler protection: mark chapters đã đọc, không reveal thông tin từ chương chưa đọc
- Command: `/llm-wiki book-summary` → tóm tắt có cấu trúc toàn bộ
- Timeline visualization: export dạng Obsidian canvas

**Config.yaml thay đổi:**
```yaml
wiki:
  name: "Dune — Reading Companion"
  book_mode: true
  current_chapter: 24  # spoiler protection

topics:
  - name: "Characters"
    auto_extract: true
  - name: "Factions & Politics"
    auto_extract: true
  - name: "Themes"
    auto_extract: true
```

### Demo potential
- **Highest viral potential** trong 5 cái
- Demo: "Tôi đọc Game of Thrones và AI tự build wiki nhân vật cho tôi" → screenshot impressive
- Video demo ngắn 60s: drop chapter text → xem wiki build real-time
- BookTok / Bookstagram: visual content potential cao
- **Score: 10/10**

### Viral potential
- Book community = content creator heaven (BookTok 100B+ views)
- r/Fantasy, r/books, r/scifi: hàng triệu subscribers
- Có thể share pre-built wikis cho sách nổi tiếng → community contribution
- **Score: 10/10**

### Build effort
- Spoiler protection logic: cần thêm ~2 giờ
- book-summary command: 1-2 giờ
- Chapter-by-chapter ingest workflow: chủ yếu là README/docs
- **Score: 8/10**

### Monetization path
- Khó trực tiếp (readers không trả tiền nhiều)
- Nhưng viral → funnel người dùng về llm-wiki chính
- Book club tier: shared wiki for group

### Tổng điểm
| Tiêu chí | Điểm | x Trọng số | Tổng |
|----------|------|-----------|------|
| Market size | 8 | x3 | 24 |
| Pain intensity | 7 | x3 | 21 |
| Viral potential | 10 | x2 | 20 |
| Build effort | 8 | x2 | 16 |
| Community fit | 10 | x2 | 20 |
| Monetization | 3 | x1 | 3 |
| **TOTAL** | | | **104/130** |

---

## Bảng tổng hợp & Thứ tự ưu tiên

| Rank | Variant | Score | Lý do ưu tiên |
|------|---------|-------|--------------|
| 🥇 #1 | **Competitive Intelligence** | 112/130 | Pain intensity + monetization cao nhất, community fit tốt |
| 🥈 #2 | **Job Search Intel** | 108/130 | Market lớn, dễ build, viral trên LinkedIn/Reddit |
| 🥉 #3 | **Book Companion Wiki** | 104/130 | Viral potential cao nhất → growth flywheel |
| 4 | Chronic Condition | 100/130 | Market khổng lồ nhưng khó demo & viral |
| 5 | Academic Literature | 92/130 | Niche, khó monetize, nhưng kỹ thuật ấn tượng |

---

## Chiến lược đề xuất

### Phase 1 — Viral First (tuần 1-2)
Build **Book Companion Wiki** trước — vì viral potential cao nhất, dễ demo, dễ share.
Mục tiêu: 500 GitHub stars, được share trên r/Fantasy và BookTok.
→ Tạo brand awareness cho toàn bộ llm-wiki ecosystem.

### Phase 2 — Monetization (tuần 3-4)
Build **Competitive Intelligence Wiki** — pain point rõ, B2B willing to pay.
Mục tiêu: 10 người dùng trả tiền đầu tiên.
→ Validate product-market fit.

### Phase 3 — Breadth (tháng 2)
Build **Job Search Intel** — mass market, LinkedIn virality.
→ Mở rộng user base.

### Phase 4 — Long-term
- Academic (prestige, attracts smart users)
- Health (regulatory research cần thiết trước)

---

*File này được tạo tự động từ brainstorming session 2026-04-14*
