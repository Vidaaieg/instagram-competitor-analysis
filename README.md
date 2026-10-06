# 📊 حلّل ريلز منافسينك بـ Claude وApify واعرف إيه اللي بيضرب

بتنزل ريلز ومش عارف ليه ريل يضرب وريل يموت؟ والمنافسين بتوعك بيجيبوا أرقام وانت مش فاهم السر؟
Claude مع Apify يقدروا يسحبوا ريلز أي حسابات على Instagram، ويحللوها، ويطلعولك Dashboard كامل بالعربي + أفكار ريلز جاهزة ليك.

هنا هتلاقي **الـ Prompt اللي استخدمته بالظبط** في الريل، ومعاه الـ Prompts اللي بكمّل بيها بعده.

> 💡 انسخ الـ Prompt زي ما هو، وغيّر بس الحاجات اللي بين `[ ]`.

---

## 🔌 الأول: اربط Apify بـ Claude

1. اعمل حساب على [Apify](https://apify.com) (فيه Free plan تبدأ بيه).
2. من الـ Connectors في Claude، ضيف Apify واعمل Login بحسابك.
3. اتأكد إنه شغال: اسأل Claude «إيه الأدوات اللي متاحة في Apify؟»

لو احتجت مساعدة في الـ Connectors: [support.claude.com](https://support.claude.com)

> ⚠️ Apify بيحاسب على حسب الاستخدام. عشان كده الـ Prompt بيخلي Claude يوريك **التكلفة الأول** ويستنى موافقتك قبل ما يسحب أي حاجة.

---

## 🧠 1. الـ Prompt الأساسي: تحليل المنافسين الكامل

ده الـ Prompt اللي في الريل. حط حسابك وحسابات منافسينك (5 أو أكتر، عرب وأجانب) وسيبه يشتغل.

```
You are my Instagram competitor analyst. I make Egyptian Arabic reels about Claude, Claude Code, Codex, n8n and AI automation.

My account: @[حسابك]
Competitors: @[منافس 1], @[منافس 2], @[منافس 3], @[منافس 4], @[منافس 5]

All results (dashboard, findings, ideas) must be in Egyptian Arabic. Keep tool names, handles and numbers as they are.

────────────────
0) COST FIRST
Before scraping anything, show me a cost estimate table (per actor, per step, and total) and wait for my OK.
- Use Apify pay-per-event prices for the Free tier (worst case) and say paid plans are cheaper.
- Set maxTotalChargeUsd on every run so nothing can go over the estimate.
- At the end, tell me the estimated total spent.

1) SCRAPE (Apify)
- apify/instagram-reel-scraper: resultsLimit 40 per account, onlyPostsNewerThan = Jan 1 of the current year. Do NOT turn on shares count or video download.
- apify/instagram-profile-scraper: follower counts for every handle.
- Assign each reel to the handle I gave (from inputUrl), not ownerUsername, because collab reels show other owners.
- Drop pinned reels from previous years.
- Skip any handle that isn't found or has no reels and list it at the end with the reason.
- If the shell can't reach api.apify.com, read the dataset through the Apify connector in pages of 100 and process the saved files with Python.

2) ANALYZE
- Each creator's median views (play count) for the year. Each reel's multiple = views ÷ that creator's median. 3x or more = breakout.
- Rank everything by multiple, never by raw views.
- Flag creators with fewer than 10 reels or a median under 500 views. Their multiples are unstable, so mention it next to their numbers.
- Tag each reel by hand from its caption with exactly one topic: Claude tips/skills, Claude Code/Codex, n8n, connectors/MCP, free tools, automate your life, business automation, model news, skits/humor, learning/roadmaps. Use "other/off-topic" for personal posts, events and empty captions.
- Hook = the first sentence of the caption. Empty caption = "n/a".
- Keyword CTA = the caption asks people to comment a specific word (or "any word") to get something by DM, in any language (Comment X, اكتب كلمة, علق, Comenta, 留言…). "Tell me in the comments" is NOT a keyword CTA. After detecting with a regex, review the misses and false positives by hand.
- Compare keyword CTA vs none two ways:
  (a) all competitors pooled;
  (b) only creators who post both ways (at least 3 reels on each side), so creator size can't explain the gap.
- Hook features: breakout rate when the first sentence names a tool, says "free", has a number, or is a question.
- Duration buckets: <20s, 20–30, 30–45, 45–60, 60–90, 90+.
- Find the same idea going viral in different languages and group the reels by idea.
- Transcripts:
  - Only for the top 30 reels by multiple, using the reel scraper with includeTranscript on the reel URLs.
  - Before running, estimate the cost from the started minutes (by duration) and keep it under $3.
  - Spoken hook = first sentence of the transcript.
  - Record the spoken language, since some creators caption in English but speak Arabic.

3) DASHBOARD
Build one interactive HTML dashboard in Egyptian Arabic, right-to-left. It must work at phone width and in light and dark mode. Keep numbers and handles left-to-right, and keep hooks in their original language.
Sections:
1. Summary numbers: reels, creators found, breakouts, field breakout rate vs mine, my median length vs the field.
2. Breakout rate by topic, with my rate marked on each bar where I posted that topic.
3. Sortable creator leaderboard: followers, reels, median views, median ÷ followers, breakouts, breakout %, best multiple, keyword CTA %, median length.
4. Duration vs multiple scatter (log scale, 3x line, my reels highlighted) plus breakout rate by duration bucket.
5. Keyword CTA vs none: pooled, same creators only, and me.
6. Top 20 breakouts table: creator, multiple, views, topic, spoken language, length, caption hook, spoken hook, link.
7. Same idea in different languages.
8. Me vs the field: breakout rate, CTA share, length, topic mix, and all my reels ranked by multiple.
Footer: data source, date range, and the handles that were skipped.

4) GIVE ME (in Egyptian Arabic)
- The 5 strongest patterns, each with its numbers (rate, count out of total, median multiple).
- 15 reel ideas for me as a table:
  - the hook in Egyptian Arabic, under 15 words, with the tool name and the result in the first sentence;
  - the source reel(s) and their multiple;
  - a Growth tag (reach/followers) or an Acquisition tag (brings business clients).
- Topics I should stop making, backed by my own numbers and the field's.
- The handles that were skipped and why, and the total cost.

Use real numbers only. Mark anything missing "n/a".
```

**بيعمل إيه؟** بيسحب آخر 40 ريل لكل حساب من أول السنة، ويقارن كل ريل بالمتوسط بتاع صاحبه (مش بعدد المشاهدات)، عشان تعرف الفكرة نفسها اللي قوية مش الحساب الكبير. وفي الآخر بيطلعلك Dashboard بالعربي + 15 فكرة ريل.

**ليه القسمة على المتوسط مهمة؟** الحساب اللي عنده مليون فولو هيكسب دايمًا في المشاهدات. لكن لو ريل جاب 5 أضعاف المتوسط بتاع صاحبه، يبقى الفكرة دي هي اللي ضربت.

**نصيحة:** متغيّرش في الـ Prompt غير الحسابات والمواضيع. لو بتعمل محتوى عن حاجة تانية غير Claude وn8n، غيّر أول سطر ولستة الـ Topics في جزء 2.

---

## 🧭 2. إزاي تختار المنافسين صح

قبل ما تشغّل الـ Prompt الأساسي، خلّي Claude يساعدك تختار:

```
I make Egyptian Arabic reels about [موضوع المحتوى بتاعك].
Suggest 15 Instagram creators in the same niche: 5 Arabic, 10 in other languages (English, Spanish, Hindi...).
Mix sizes: some under 50K followers, some over 500K.
For each: handle, language, approximate size, and why they're relevant.
Mark any handle you are not sure exists, so I can check it before scraping.
```

**بيعمل إيه؟** بيديك لستة متنوعة. الحسابات الأجنبية مهمة جدًا لإن الأفكار بتضرب عندهم الأول وبعدين توصل هنا.

**نصيحة:** افتح كل حساب بنفسك واتأكد إنه موجود وبينزل ريلز قبل ما تحطه في الـ Prompt.

---

## 💡 3. بنك أفكار كبير من نفس الداتا

بعد ما التحليل يخلص، في نفس الشات:

```
Using the same dataset and findings, build me an idea bank of 40 reel ideas.
For each idea:
- hook in Egyptian Arabic (under 15 words, tool name + result in the first sentence)
- the source reel(s) and their multiple
- topic
- format: talking head / screen recording / screen + face / skit
- Growth or Acquisition tag
Sort by the source multiple, highest first. Only use ideas backed by real reels in the data.
```

**بيعمل إيه؟** بيحوّل الريلز اللي ضربت عند غيرك لأفكار ليك، وكل فكرة معاها دليل من الأرقام.

---

## 📅 4. خطة نشر 13 أسبوع

```
Turn the idea bank into a 13-week posting plan.
- [عدد الريلز في الأسبوع] reels per week.
- Mix Growth and Acquisition ideas every week, with more Growth.
- Don't put two ideas from the same topic back to back.
- Put the ideas with the highest source multiple in the first 4 weeks.
Show it as a table: week, day, hook, topic, format, tag.
```

**بيعمل إيه؟** بدل ما تصحى كل يوم تفكر تنزل إيه، الخطة جاهزة قدامك.

---

## 🔍 5. حلّل ريل واحد ضرب عند حد

لو شفت ريل عمل أرقام مجنونة وعايز تفهم ليه:

```
Analyze this reel: [لينك الريل]
Use apify/instagram-reel-scraper with includeTranscript on this one URL only. Show me the cost first and wait for my OK.
Then tell me in Egyptian Arabic:
- the spoken hook (first sentence) and why it works
- the structure second by second (hook, setup, payoff, CTA)
- how I can make my own version in Egyptian Arabic, with 3 hook options
```

**بيعمل إيه؟** بيفكك الريل لأجزاء ويديك نسختك منه.

---

## 🔁 6. حدّث التحليل كل شهر

```
Re-run the same competitor analysis, but only for reels posted after [تاريخ آخر تحليل].
Show me the cost first and wait for my OK.
Then tell me in Egyptian Arabic: what changed vs last time, any new topic that started breaking out, and 5 new ideas.
```

**بيعمل إيه؟** الترندات بتتغير بسرعة. كده بتفضل عارف إيه الجديد من غير ما تعيد كل حاجة من الأول.

---

## 🇪🇬 تقدر تكتبله بالمصري كمان

```
عايزك تحلل ريلز الحسابات دي على Instagram: [الحسابات]
هات آخر 40 ريل لكل حساب من أول السنة عن طريق Apify، بس وريني التكلفة الأول واستنى موافقتي.
اقسم مشاهدات كل ريل على متوسط صاحبه، وأي ريل جاب 3 أضعاف أو أكتر اعتبره ضرب.
وبعدين قولي: أنهي مواضيع بتضرب أكتر، وأنهي هوكات، وأنهي طول فيديو أحسن، وطلعلي 15 فكرة ريل ليا بالمصري.
```

---

## ⚠️ نصايح مهمة

- **التكلفة الأول دايمًا:** متخليش Claude يشغّل أي Scraper قبل ما يوريك الرقم وتوافق.
- **متشغّلش Video download أو Shares count:** بيغلّوا التكلفة ومش محتاجهم في التحليل.
- **راجع الأرقام:** لو حاجة شكلها غريب، اسأل Claude «الرقم ده جه منين؟» قبل ما تبني عليه قرار.
- **الحسابات الصغيرة:** لو حساب عنده ريلز قليلة أو مشاهدات قليلة، أرقامه مش ثابتة. خد بالك منها.
- **انقل الفكرة مش الريل:** خد الفكرة اللي ضربت واعملها بطريقتك وبالمصري، متنسخش حد.

---

## 🚀 الخلاصة

| قبل | بعد |
|---|---|
| بتنزل ريلز على حسب المزاج | بتنزل أفكار ضربت قبل كده بالأرقام |
| بتقارن نفسك بالحسابات الكبيرة وتتضايق | بتقارن كل ريل بمتوسط صاحبه |
| مش عارف تصوّر إيه بكرة | عندك بنك أفكار وخطة 13 أسبوع |
| بتتابع المنافسين يدوي | Claude بيحلل مئات الريلز في دقايق |

---

لو استفدت، تابعني عشان فيه Prompts وأتمتة أكتر جاية 🔥

**Steven Ayman — Vida AI**
