# Book Companion Wiki — Schema Extensions

> Extends the base CLAUDE.md schema with book-specific entity types.
> Merged automatically by: /llm-wiki setup book-companion

## Additional Entity Types

### character (wiki/entities/)

```markdown
---
type: entity
category: character
book: "Book Title"
first_appearance: chapter N
status: alive | dead | unknown
faction: faction-name
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [chapter-notes-files]
---

# Character Name

Mô tả ngắn 1-2 câu: vai trò, background.

## Profile
- **Full name:** [tên đầy đủ nếu có]
- **Age:** [tuổi ước tính]
- **Faction:** [[faction-name]]
- **Role:** [vai trò trong câu chuyện]
- **Status:** Alive / Dead / Unknown (đến chapter hiện tại)

## Personality & Motivations
[Tính cách, động lực, mục tiêu]

## Key Relationships
- [[character-1]]: [mối quan hệ]
- [[character-2]]: [mối quan hệ]

## Key Moments
- Chapter N: [Sự kiện quan trọng]
- Chapter N: [Sự kiện quan trọng]

## Notable Quotes
> "Quote" — Chapter N

## Liên kết
- [[faction-name]]
- [[location-name]]

## Nguồn
- [Chapter N notes](../raw/articles/chapter-N-notes.md)
```

### location (wiki/entities/)

```markdown
---
type: entity
category: location
book: "Book Title"
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Location Name

Mô tả ngắn 1-2 câu.

## Tổng quan
[Mô tả địa điểm, khí hậu, đặc điểm]

## Significance
[Tại sao địa điểm này quan trọng trong câu chuyện]

## Key Events Here
- Chapter N: [Sự kiện xảy ra tại đây]

## Residents / Factions
- [[character-name]]
- [[faction-name]]

## Liên kết
- [[related-location]]

## Nguồn
- [Chapter N notes](../raw/articles/chapter-N-notes.md)
```

### faction (wiki/entities/)

```markdown
---
type: entity
category: faction
book: "Book Title"
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# Faction Name

Mô tả ngắn 1-2 câu: phe phái, mục tiêu.

## Tổng quan
[Goals, ideology, power base]

## Members
- [[character-1]] — [vai trò]
- [[character-2]] — [vai trò]

## Alliances & Enemies
- Allies: [[faction-2]]
- Enemies: [[faction-3]]

## Key Actions (đến chapter hiện tại)
- Chapter N: [Hành động quan trọng]

## Liên kết
- [[location-name]]

## Nguồn
- [Chapter N notes](../raw/articles/chapter-N-notes.md)
```

### event (wiki/concepts/)

```markdown
---
type: concept
domain: event
book: "Book Title"
chapter: N
created: YYYY-MM-DD
sources: []
---

# Event Name

Mô tả ngắn.

## Bối cảnh
[Tại sao sự kiện xảy ra]

## Diễn biến
[Tóm tắt ngắn gọn]

## Hậu quả
[Ảnh hưởng đến câu chuyện]

## Nhân vật liên quan
- [[character-1]]

## Liên kết
- [[related-event]]

## Nguồn
- [Chapter N notes](../raw/articles/chapter-N-notes.md)
```

### theme (wiki/concepts/)

```markdown
---
type: concept
domain: theme
book: "Book Title"
created: YYYY-MM-DD
sources: []
---

# Theme Name

Mô tả ngắn chủ đề.

## Biểu hiện trong truyện
- Chapter N: [Ví dụ cụ thể]
- Chapter N: [Ví dụ cụ thể]

## Ý nghĩa
[Tại sao theme này quan trọng với tác phẩm]

## Liên kết
- [[related-theme]]
- [[character-embodying-theme]]

## Nguồn
- [Chapter N notes](../raw/articles/chapter-N-notes.md)
```

## Ingest Rules (book_mode)

1. **Spoiler protection:** Mỗi file raw nên có frontmatter `chapter: N`. Nếu `N > config.current_chapter` → skip.
2. **Character extraction:** Mỗi tên nhân vật xuất hiện lần đầu → tạo character page
3. **Location extraction:** Mỗi địa điểm mới → tạo location page
4. **Quote capture:** Câu nói đáng nhớ → thêm vào character page + wiki/concepts/quotes.md
5. **Timeline:** Mỗi sự kiện quan trọng → thêm vào event page và LOG
6. **Cross-reference:** Mỗi character page link đến faction, location, và characters liên quan

## Sample File Naming

```
raw/articles/
├── ch01-opening-test.md          # chapter: 1
├── ch02-arrakis-briefing.md      # chapter: 2
├── ch03-departure.md             # chapter: 3
├── characters-master-list.md     # chapter: 0 (safe, no spoilers)
└── world-building-notes.md       # chapter: 0 (general notes)
```
