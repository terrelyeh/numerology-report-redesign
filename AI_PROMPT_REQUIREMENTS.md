# Numerology Within — 內容頁優化需求文件

> **用途**：交付給 AI（Claude / ChatGPT / Cursor 等）執行網站內容頁改版
> **適用範圍**：免費報告頁、付費報告頁的內容結構與呈現
> **不含**：首頁（Homepage）、後端 API、金流串接
> **建立日期**：2026-04-06

---

## 如何使用這份文件

將這份文件整份貼入 AI 對話的第一則訊息，然後在下方補充你的具體指令，例如：
- 「請根據這份需求，幫我重寫免費報告頁的前端程式碼」
- 「請根據 Section 4 的 Action Insight 框架，幫我為 Life Path Number 7 撰寫 Action Insight 內容」
- 「請根據這份需求，幫我產生付費報告的 Vue component 結構」

---

## 1. 專案背景

### 1.1 網站資訊
- **網站**：numerologywithin.net（正式站）/ test.numerologywithin.net（測試站）
- **技術架構**：Vue.js SPA（Single Page Application）
- **目標市場**：美國（全英文）
- **定價**：$9.90 USD / 份（一個生日 = 一份報告）
- **商業模式**：使用者輸入生日 → 看免費報告 → 付費解鎖完整報告

### 1.2 核心問題（為什麼要改版）
1. **轉換率低**：免費 → 付費的轉換率不理想
2. **免費版給太多**：完整的 Daily Number 解讀讓使用者覺得「夠了」，沒有升級動機
3. **付費版缺乏差異化**：付費後視覺、功能、內容都跟免費版「感覺差不多」
4. **沒有行動建議**：報告只告訴使用者「你是什麼樣的人」，不告訴你「接下來該怎麼做」
5. **缺乏感知價值功能**：無 PDF 下載、無進度條、無分享功能

### 1.3 設計原型參考
以下 HTML 原型是本次改版的視覺與結構參考，AI 應以這些原型為標準：
- **免費版原型**：https://terrelyeh.github.io/numerology-report-redesign/free.html
- **付費版原型**：https://terrelyeh.github.io/numerology-report-redesign/
- **完整優化指南**���https://terrelyeh.github.io/numerology-report-redesign/guide.html

---

## 2. 視覺設計系統

### 2.1 色彩系統

```css
:root {
  --cream: #EDE8E0;       /* 頁面背景 */
  --warmgray: #2C3345;    /* 主要文字、深色區塊 */
  --sage: #3A6B5E;        /* 主要強調色（CTA、標籤、進度條） */
  --sagelight: #E8F0ED;   /* Action Insight 區塊背景 */
  --sagemid: #D0E2DB;     /* 中間色調、hover 狀態 */
  --gold: #B8964E;        /* 次要強調色（星星、Connection Pattern） */
  --goldlight: #FBF6EE;   /* 金色背景 */
  --rose: #E8D5D0;        /* Frustration Connection 標籤背景 */
  --rosedark: #8B6B62;    /* Frustration Connection 標籤文字 */
}
```

**規則**：
- 禁止使用純黑 `#000000`，主要深色一律用 `--warmgray`（`#2C3345`）
- 頁面背景一律 `--cream`（`#EDE8E0`）
- 數字用 `font-variant-numeric: tabular-nums`，不用 `font-family: monospace`

### 2.2 字體系統

```css
body { font-family: 'DM Sans', sans-serif; }
h1, h2, h3, .font-serif { font-family: 'Instrument Serif', Georgia, serif; }
```

- **Google Fonts 載入**：`Instrument Serif:ital@0;1` + `DM Sans:wght@300;400;500;700`
- 標題、數字、裝飾性文字 → Instrument Serif
- 內文、按鈕、UI 元素 → DM Sans

### 2.3 圓角與間距
- 卡片：`border-radius: 1rem`（16px）到 `1.5rem`（24px）
- 按鈕：`border-radius: 9999px`（pill shape）或 `0.75rem`（12px）
- 區塊間距：`padding: 3rem`（48px）到 `6rem`（96px）

---

## 3. 免費報告頁（Hook Page）需求

### 3.1 核心策略：好奇缺口理論（Curiosity Gap）

**原則**：展示「你的數字是什麼」（What），鎖住「這對你意味著什麼」（How）和「你該怎麼做」（Action）。

使用者讀完免費版後，應該要有的感覺：
- ✅ 「這份報告確實是根據我的生日計算的」（建立信任）
- ✅ 「我的 Ego Number 9 的描述很準」（建立認同）
- ❌ 「但我好想知道 out of balance 會怎樣...」（��奇缺口）
- ❌ 「那個 Action Insight 到底寫了什麼？」（購買衝動）

### 3.2 頁面結構

```
[Header]
  - 個人化問候：「Hi, {名字}.」
  - 出生日期顯示
  - 個人化命理語錄（根據核心數字組合產生的一句話 hook）

[Section 1 — Birth Chart]（完全免費）
  - 3x3 Birth Chart 格線圖（CSS Grid）
  - 每個數字顯示出現頻率（小圓點）
  - 右側 4 張數字快速卡片（Ego / Superego / Identity / Life Path）
  - 標記「Included Free」

[Section 2 — Ego Number 部分解讀]（部分免費）
  - 免費段落：Ego Number 的第一段解讀（數字的核心能量、三大特質）
  - 鎖定區域：
    - 後續段落用 CSS 模糊化（backdrop-filter: blur(6px)）
    - 上方疊加漸層遮罩（transparent → cream）
    - Action Insight 區塊模糊可見但不可讀
  - 鎖定覆蓋文案範例：
    "Your full Ego reading continues... Including what happens when
    your {X} is out of balance, and a personalized Action Insight
    you can apply this week."

[Section 3 — 鎖定預覽卡片]（全部鎖定，6 張卡片）
  每張卡片包含：
  - 數字名稱 + 標題
  - 一句好奇觸發文案（見 3.3）
  - 鎖頭圖示
  - 「Includes Action Insight」標記

[Section 4 — Social Proof]
  - 3 則五星推薦語
  - 每則精準對應一個付費功能（Action Tips / Connection Patterns / Monthly Forecast）
  - 格式：五星 + 推薦語 + 姓名縮寫 + 州名

[Section 5 — 主要 CTA 區塊]（深色背景 bg-warmgray）
  - 標題：「Unlock Your Complete Numerology Blueprint」
  - 6 項價值清單（含勾選圖示）
  - 價格錨定：~~$19.90~~ → $9.90
  - 副標：「One-time payment · Lifetime access」
  - CTA 按鈕：「Unlock My Full Report →」帶脈衝動畫
  - 信任標誌列：Secure Payment / 256-bit Encrypted / 30-Day Guarantee

[Section 6 — FAQ 手風琴]
  - 4 個可展開問答（見 3.5）

[浮動底部 CTA 列]
  - 固定在底部，滾動超過 800px 後滑入
  - 包含：文案 + ~~$19.90~~ $9.90 + CTA 按鈕
```

### 3.3 鎖定卡片的好奇觸發文案

這些文案需要根據使用者的實際數字動態產生。以下是文案撰寫公式和範例：

**公式**：展示「是什麼」+ 暗示「還有更多」+ 不揭露「具體答案」

| 卡片 | 文案範例（需根據使用者數字動態調整） |
|------|------|
| Superego {X} | "Discover why your intuition works best in [情境], and what happens when {X}'s [特質] turns into [負面]." |
| Identity {X} | "Why [某件事] doesn't come naturally to you — and why that gap creates [影響] in your closest relationships." |
| Connection Patterns | "Your chart has {N} connection patterns that reveal hidden strengths and recurring friction points." |
| Life Path {X} | "[正面特質] is your superpower — but without direction, it becomes [負面]." |
| Key Life Ages | "{N} pivotal ages mapped to your chart. At {當前最近的 Peak}, you're in your [第幾] Pyramid — a [主題] phase." |
| Monthly Forecast | "{年份} is your Personal Year {X} — a year of [主題]. Get month-by-month guidance." |

**寫作風格規則**：
- 第二人稱（you / your），直接對使用者說話
- 語氣：溫暖但直接，像一個聰明的朋友在點你
- 不要用「amazing」「incredible」等空洞形容詞
- 每句話要有具體的、跟數字相關的細節
- 製造「未完成感」——讓使用者覺得少了一塊拼圖

### 3.4 CSS 模糊鎖定技術規格

```css
.content-locked {
  position: relative;
  user-select: none;
}

.content-locked::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
  background: linear-gradient(
    to bottom,
    transparent 0%,
    var(--cream) 100%
  );
  z-index: 10;
}
```

**重要**：模糊區域內的文字必須是真實內容（不是 lorem ipsum），讓使用者隱約看到「裡面有東西」但讀不清���。

### 3.5 FAQ 內容

| 問題 | 回答重點 |
|------|---------|
| How is this different from free numerology sites? | 強調 Action Insights（行動建議）是獨家功能 |
| What are 'Action Insights'? | 解釋每個數字解讀末尾都有具體的、本週可執行的行動建議 |
| Can I access my report later? | 永久存取 + PDF 下載 |
| What if I don't find it valuable? | 30 天退款保證 |

---

## 4. 付費報告頁需求

### 4.1 整體架構

付費報告分為兩個主要頁籤（Tab）：

```
[閱讀進度條]（固定在頁面最頂部，3px 高，sage 綠色）

[導航列]
  - 左：Numerology Within logo
  - 右：Share 按鈕 + Save PDF 按鈕（桌面版）

[Hero 區塊]
  - 個人化標題
  - 出生日期
  - 雙頁籤切換：Inner Blueprint | Outer Path

[Tab 1：Inner Blueprint — 內在藍圖]
  ├── Birth Chart（3x3 互動格線 + 數字導航卡片）
  ├── Ego Number — Your Conscious Self + Action Insight
  ├── Superego Number — Your Higher Inner Guidance + Action Insight
  ├── Identity Number — Your Raw Inner Drives + Action Insight
  ├── Isolated Number — The Missing Piece + Action Insight
  ├── Enquirer Connection + Action Insight
  └── Frustration Connection + Action Insight

[Tab 2：Outer Path — 外在路徑]
  ├── Life Path Number — Core life lessons + Action Insight
  ├── Attitude Number — First impression + Action Insight
  ├── Day of Birth — Natural talents + Action Insight
  ├── Key Life Ages（視覺時間線 + 4 個 Peak Pyramid）+ Action Insight
  └── Monthly Forecast（年度概述 + 12 張月卡）+ This Month's Focus Action Insight

[報告完成區塊]
  - 完成圖示 + 鼓勵文案
  - 兩張 Next Step CTA 卡片（見 4.6）

[底部 Save & Share 區塊]
  - PDF 下載按鈕
  - 分享連結按鈕

[Footer]
```

### 4.2 頁籤切換機制

```javascript
function switchTab(tab) {
  document.getElementById('content-inner').classList.toggle('hidden', tab !== 'inner');
  document.getElementById('content-outer').classList.toggle('hidden', tab !== 'outer');
  // 切換 tab button 的 active/inactive 樣式
  // 滾動到頁面頂部（y: 200 左右，跳過 hero）
}
```

- 頁籤按鈕使用 `position: sticky; top: 65px`（在導航列下方固定）
- Active tab：`bg-sage text-white`
- Inactive tab：`bg-transparent text-warmgray/50 hover:text-warmgray`

### 4.3 Action Insight 機制（最重要的差異化元素）

**每一個數字解讀的末尾都必須附帶一個 Action Insight 區塊。**

#### 結構

```html
<div class="bg-sagelight rounded-xl p-6 border border-sage/20 mt-6">
  <p class="text-sage text-xs tracking-widest uppercase font-medium mb-3">
    YOUR ACTION INSIGHT
  </p>
  <p class="text-warmgray/80 text-sm leading-relaxed mb-4">
    {個人化洞察段落：連結這個數字的含義與使用者的實際生活}
  </p>
  <div class="border-t border-sage/10 pt-4">
    <p class="text-sage text-xs font-bold uppercase tracking-wider mb-1">Try this:</p>
    <p class="text-warmgray/70 text-sm leading-relaxed">
      {一句話的具體微行動，本週可以立刻執行}
    </p>
  </div>
</div>
```

#### 寫作框架

每個 Action Insight 包含兩層：

1. **個人化洞察**（2-3 句）：把這個數字的抽象含義翻譯成使用者的日常體驗
   - 不要重複解讀段落已經說過的內容
   - 要連結到「真實生活情境」，讓使用者覺得「對，這就是我的情況」
   - 語氣像一個智慧的朋友在跟你喝咖啡時說的話

2. **Try this 微行動**（1 句）：一個本週可以立刻執行的具體行動
   - 必須非常具體（不是「be more mindful」這種空話）
   - 必須低門檻（5-10 分鐘可完成）
   - 必須跟這個數字的含義直接相關

#### 範例（供 AI 生成其他數字的 Action Insight 時參考）

**Ego Number 9：**
> 洞察：Your 9 energy drives you to take responsibility for everything — but not everything is yours to carry. The gap between your idealism and reality isn't a flaw; it's the tension that makes you effective. The key is choosing which battles deserve your full weight.
>
> Try this: Write down one thing you're currently carrying that isn't yours to hold. Then consciously set it down this week — not because you don't care, but because your energy belongs elsewhere.

**Superego Number 8：**
> 洞察：Your intuition doesn't speak in words — it speaks in gut feelings, sudden clarity, and quiet knowing. But it works best when you're not rushing. The more noise around you, the harder it is to hear what your 8 is trying to tell you.
>
> Try this: Before your next important decision, take 10 minutes of complete silence — no phone, no music, no input. Just sit with the question. Notice what surfaces.

**Identity Number 1：**
> 洞察：Expressing deep emotion doesn't come naturally to you, but that doesn't mean those feelings aren't there. Your 1 processes internally first, and by the time you're ready to share, the moment has often passed. This creates distance in relationships — not from lack of caring, but from timing.
>
> Try this: This week, tell one person something you appreciate about them — something you'd normally keep to yourself.

### 4.4 各數字解讀的內容框架

每個數字解讀（Ego、Superego、Identity 等）應遵循以下結構：

```
[數字標題]（例：Ego Number 9 — Your Conscious Self）

[段落 1：核心能量]
  這個數字的基本含義，它代表什麼能量、什麼傾向。
  2-3 個核心特質關鍵字。

[段落 2：平衡狀態]
  當這個數字能量處於健康平衡時，使用者會表現出什麼特質。
  具體的行為描述，不是抽象概念。

[段落 3：失衡狀態]
  當這個數字能量失衡時會怎樣。
  具體的行為模式或困境描述。

[段落 4：與其他數字的互動]（如適用）
  這個數字如何跟使用者的其他核心數字產生交互作用。
  例如：「Your 9 Ego and 1 Identity create a push-pull between idealism and raw instinct...」

[Action Insight 區塊]（見 4.3）
```

**寫作風格規則**：
- 全程第二人稱（you / your）
- 語氣：溫暖、直接、有深度，但不故弄玄虛
- 避免：「amazing」「incredible」「powerful」等空洞形容詞
- 每段話都要有具體的、可辨識的行為描述
- 讓使用者覺得「這說的就是我」，不是「這說的是所有人」
- **長度**：每個數字解讀 200-400 字英文（不含 Action Insight）

### 4.5 特殊區塊規格

#### Connection Patterns

Connection Pattern 是 Birth Chart 中特定數字組合形成的模式。常見的有：

- **Enquirer Connection**（探索者連結）：使用 `bg-gold/20 text-gold` 標籤
- **Frustration Connection**（挫折連結）：使用 `bg-rose text-rosedark` 標籤

每個 Connection Pattern 需要：
- 標籤（Badge）+ 標題
- 2-3 段解讀（這個連結代表什麼、如何影響日常、如何善用）
- Action Insight

#### Key Life Ages（視覺時間線）

- 顯示 4 個 Peak Pyramid 年齡，以圓形 + 箭頭連接
- 圓形：`bg-sage text-white w-16 h-16 rounded-full`
- 每個 Peak 下方有獨立的解讀段落
- 標示使用者目前處於哪個 Pyramid

#### Monthly Forecast（月度預測）

- 先顯示年度概述（Personal Year {X} 的主題）
- 12 張月卡，使用 `grid sm:grid-cols-2 lg:grid-cols-3` 排列
- 每張卡片：月份名稱 + 2-3 句能量概述
- 卡片 hover 效果：`translateY(-2px)` + 陰影增加
- 當月卡片可用不同邊框色強��

### 4.6 報告完成區塊（Next Step CTAs）

報告最底部，在使用者讀完後顯示兩張並排 CTA 卡片：

**卡片 A：幫別人買一份報告**
- 圖示：person + icon，sage 綠色
- 標題：「Get a Reading for Someone Else」
- 說明：「Enter a family member's or friend's birthday and discover their unique numerology blueprint.」
- 連結文字：「Start their reading →」

**卡片 B：配對報告（關係分析）**
- 圖示：heart icon，gold 金色
- 標題：「Compatibility Reading」
- 說明：「See how your numbers interact with a partner, family member, or friend — discover your dynamic together.」
- 連結文字：「Explore your connection →」

### 4.7 功能規格

#### 閱讀進度條
```javascript
window.addEventListener('scroll', () => {
  const scrollTop = window.scrollY;
  const docHeight = document.documentElement.scrollHeight - window.innerHeight;
  const progress = (scrollTop / docHeight) * 100;
  document.getElementById('progress-bar').style.width = progress + '%';
});
```
- 固定在頁面最頂部，`position: fixed; top: 0; z-index: 9999`
- 高度 3px，顏色 `var(--sage)`

#### PDF 下載
- 原型中使用 `window.print()` 作為 placeholder
- 正式版建議使用 Puppeteer 或 wkhtmltopdf 在伺服器端生成 PDF
- PDF 應包含：封面頁（名字 + 生日 + 日期）、所有解讀、Birth Chart、月度預測
- 下載入口放置位置：
  - 桌面版：導航列右側
  - 手機版：Hero 區域下方（`flex md:hidden`）
  - 底部：獨立的 Save & Share 區塊

#### 分享功能
```javascript
function shareReport() {
  navigator.clipboard.writeText(window.location.href).then(() => {
    // 顯示 Toast：「Link copied to clipboard!」
    // Toast 從底部滑入，2.5 秒後自動消失
  });
}
```

---

## 5. 內容生成指引（給 AI 寫命理內容時使用）

### 5.1 語氣與風格

```
DO:
- 像一個聰明的、有洞察力的朋友在跟你說話
- 用具體的行為描述，不用抽象概念
- 承認複雜性（「this creates tension」而不是「this is bad」）
- 用「you」直接對話
- 每句話都要有資訊量

DON'T:
- 不要用「amazing」「incredible」「powerful」「magical」等空洞詞
- 不要寫成百科全書式的解說
- 不要用「Dear one」「Beautiful soul」等 New Age 口吻
- 不要每段都以「Your number X means...」開頭（變化句式）
- 不要寫超過 400 字一個數字解讀（精簡有力）
```

### 5.2 Action Insight 生成提示詞

當需要 AI 為特定數字生成 Action Insight 時，使用以下提示詞模板：

```
請為 Numerology Within 的付費報告撰寫一個 Action Insight。

數字：{數字名稱} {數字值}（例：Ego Number 9）
數字含義摘要：{一句話的含義}
使用者的其他核心數字：Ego {X}, Superego {X}, Identity {X}, Life Path {X}

要求：
1. 洞察段落（2-3 句英文）：
   - 把這個數字的含義翻譯成日常生活體驗
   - 不要重複解讀段落已說過的內容
   - 語氣像聰明朋友的觀察，不像教科書

2. Try this 微行動（1 句英文）：
   - 本週可立刻執行
   - 5-10 分鐘可完成
   - 跟這個數字的含義直接相關
   - 非常具體（不是「be more mindful」）

參考範例：
[貼入 Section 4.3 的範例]
```

### 5.3 好奇觸發文案生成提示詞

當需要 AI 為免費版鎖定卡片生成好奇觸發文案時：

```
請為 Numerology Within 的免費版鎖定卡片撰寫好奇觸發文案。

數字：{數字名稱} {數字值}
這張卡片在付費版中會展示：{付費版中這個數字解讀的核心主題}

公式：展示「是什麼」+ 暗示「還有更多」+ 不揭露「具體答案」

要求：
- 1-2 句英文
- 必須包含使用者的具體數字
- 製造「未完成感」
- 語氣直接，不要用「amazing」「incredible」等詞
- 參考格式：「Discover why [某件事], and what happens when [數字特質] turns into [負面].」
```

---

## 6. 技術注意事項

### 6.1 前端修正清單（現有 bug）

| Bug | 修正 |
|-----|------|
| 「FRUSTRATI Connection」標題截斷 | 完整顯示「Frustration Connection」，檢查容器 overflow 設定 |
| S.L. 推薦語重複出現 | 推薦語去重，確保不重複 |
| 「engery」拼字錯誤 | 全站搜尋替換為「energy」 |

### 6.2 Email 驗證碼流程（P0 優先修復）

**現行流程（問題）**：
```
點擊購買 → 輸入 Email → 去信箱找驗證碼 → 回來輸入 → 才看到付款頁
```

**建議新流程**：
```
點擊購買 → 直接進入付款頁面（Email 作為付款表單的一個欄位）→ 付款完成 → 報告顯示
```

### 6.3 響應式設計斷點

```css
/* 手機版 */          < 640px   → 單欄
/* 平板（sm）*/       ≥ 640px   → 2 欄
/* 桌面（md）*/       ≥ 768px   → 導航列完整顯示
/* 大桌面（lg）*/     ≥ 1024px  → 3 欄格線（月度預測等）
```

### 6.4 效能考量

- Birth Chart 用 CSS Grid 實作，不要用 canvas 或 SVG
- 月度預測 12 張卡片先全部渲染，不需要 lazy load（量不大）
- 頁籤切換用 `display: none / block`，不要用路由切換（保持單頁體驗）
- 進度條的 scroll event 用 `requestAnimationFrame` 或 `passive: true`

---

## 7. 驗收標準

完成改版後，使用以下清單驗收：

### 免費版
- [ ] Birth Chart 完整顯示，數字頻率小圓點正確
- [ ] Ego Number 第一段免費可讀，後續段落模糊鎖定
- [ ] 模糊區域可以看到文字輪廓但讀不清楚
- [ ] 6 張鎖定卡片都有好��觸發文案 + 鎖頭圖示
- [ ] 價格顯示 ~~$19.90~~ → $9.90
- [ ] 信任標誌（Secure Payment / Encrypted / Guarantee）出現在 CTA 區塊
- [ ] 浮動底部 CTA 列在滾動 800px 後出現
- [ ] FAQ 手風琴可正常展開/收合
- [ ] 手機版排版正常，CTA 按鈕夠大（最小觸控目標 44px）

### 付費版
- [ ] 兩個頁籤可正常切換（Inner Blueprint / Outer Path）
- [ ] 頁籤切換後自動滾動到內容頂部
- [ ] 每個數字解讀末尾都有 Action Insight 區塊（sage 綠色背景）
- [ ] 每個 Action Insight 都有「Try this:」微行動
- [ ] 閱讀進度條跟隨滾動更新
- [ ] PDF 下載按鈕存在於：桌面導航列 / 手機 Hero 下方 / 底部區塊
- [ ] 分享按鈕點擊後複製連結 + Toast 通知
- [ ] Connection Pattern 卡片有正確的彩色標籤
- [ ] Key Life Ages 時間線正確顯示 4 個 Peak 年齡
- [ ] Monthly Forecast 12 張月卡正確排列
- [ ] 報告完成區塊有兩張 Next Step CTA 卡片
- [ ] 「FRUSTRATION Connection」標題不再截斷
- [ ] 「energy」拼字正確
- [ ] 推薦語無重複
- [ ] 免費版與付費版在視覺上有明顯差異（色彩、功能、佈局）

---

## 附錄：快速參考

### 所有原型連結
| 頁面 | URL |
|------|-----|
| 首頁 | https://terrelyeh.github.io/numerology-report-redesign/home.html |
| 免費�� | https://terrelyeh.github.io/numerology-report-redesign/free.html |
| 付費�� | https://terrelyeh.github.io/numerology-report-redesign/ |
| 優化指南 | https://terrelyeh.github.io/numerology-report-redesign/guide.html |

### 色彩快速對照
| 用途 | 色碼 |
|------|------|
| 背景 | `#EDE8E0` cream |
| 文字 | `#2C3345` warmgray |
| CTA / 強調 | `#3A6B5E` sage |
| Action Insight 背景 | `#E8F0ED` sagelight |
| 次要強調 | `#B8964E` gold |
| Frustration 標籤 | `#E8D5D0` rose |

### 核心文案
| 位置 | 文案 |
|------|------|
| 免費版 CTA | Unlock My Full Report → |
| 付費版完成 | You've Read Your Full Report |
| 價格行 | ~~$19.90~~ $9.90 · One-time payment · Lifetime access |
| 信任標誌 | Secure Payment · 256-bit Encrypted · 30-Day Guarantee |
