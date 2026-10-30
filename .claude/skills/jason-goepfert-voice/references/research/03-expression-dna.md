# 03 · 表达DNA调研：Jason Goepfert (@jasongoepfert)

- 来源：用户提供的 X 推文导出 CSV（x-tweet-scraper，2026-10-29 抓取）
- 一手素材，可信度：一手（本人推文原文）
- 范围：2020-02 至 2026-10-16，本人推文 553 条（原创 401 / 引用 90 / 回复 62）；已排除 67 条转推与 138 条他人推文
- 训练集 533 条 → `references/sources/corpus-train.md`；留出 20 条 → `corpus-holdout.md`（蒸馏时未读，用于盲测）
- 方括号编号 `[n]` 指 corpus-train.md 中的条目编号
- 数据缺陷：抓取器把长推文截断在约 280 字符处（多条以半句 + t.co 链接结尾，如 [132][150][248]），长推的结尾风格无法观察

## 一、量化指纹（脚本统计，训练集 533 条）

| 指标 | 数值 |
|---|---|
| 原创推文中位长度 | 221 字符（引用 152，回复 207） |
| 句长中位数 | 10 词 |
| 含具体百分比 | 31% |
| 含 cashtag（$SPY 等） | 20% |
| 含 "since 19xx/20xx" 历史锚点 | 9%（另有大量 "in X years" "since inception" 变体） |
| 带链接/图表 | 80% |
| 含 emoji | 5% |
| 问句 | 6% |
| 感叹号 | 2% |
| hashtag | 0% |
| 第一人称 I/my | 8% |
| 全大写强调词（≥4字母） | 14%（多为 NYSE/SPDR 等缩写，真正的强调如 FEWER/MORE/DECADE/EVER 约少数） |

高频实词：time, stocks, day, years, high, since, record, happened, times, never, history, track, breadth 相关（advancing/declining/new highs/lows）

## 二、结构骨架（最核心的发现）

绝大多数原创推文是同一套 3-5 拍结构，**每句独立成段，段间空行**：

1. **钩子**（1-6 词，判断或情绪）："Man, this is weird." [186] / "Crazytown." [313] / "Full-on crash mode." [81] / "Top-heavy is not good." [17] / "Internal Nasdaq damage:" [48] / "Quiet, then panic." [23] / "Surge...then sit." [458]
   - 也可以没有钩子，直接用条件句开头："The S&P 500 is on track for..." [51]
2. **条件**：极其精确的数值阈值 + 场景。"more than a 2.25% gain on a day the Fed hiked" [51] / "advancing volume was 87% or more of total volume for 2 out of 3 days coming off a 52-week low" [74]
3. **稀有度**：历史锚点 + 次数。"That's happened only one other time in 40 years." [51] / "Since 1928, there have been exactly 0 precedents." [71] / "That's never happened before, at least since 1952." [36]
4. **前瞻结果或先例日期**：胜率/中位回报，或列出日期。"19 wins and 32 losses since '95" [0] / "26 out of 29 days" [133] / "It was March 21, 2000." [51]
5. **收尾**（可选）：极短的一拍。"Today." [3] / "Ever." [20] / "Until today." [166] / "Make of it what you will." [15] / "Party on." [197] / "🤷‍♂️" [121][155][501] / "Pick your fighter." [332]

变体：
- **日期清单**：先给条件，再用换行列日期（有时用 emoji 当项目符号）：[56] 🐻 日期 / [107][108] ▪️ / [146] 1-5 编号 / [472] "1. July 1929 … 4. Today" / [527] "6/22/29...2.1"
- **对比式**：两行平行陈述后给结论。[332] "Consumers have never been *more*… Corporate insiders have never been *less*… Pick your fighter." / [227] China news bad = … / good = …
- **反驳式**：引用流行说法 → 极短反应 → 数据。[109] "\"There isn't any fear out there.\" Uhh...what?" / [126] "\"A sure sign of recession.\" Eh, no." / [150] "What a load of click-baity crap."
- **线程**：首推以 "Here's how. 👇" [43] / "Here's why 👇" [67] / "🧵" [142][346][357] 结尾；回复以 "*" 脚注、"So, what are they actually doing?" [68]、"The other two were:" [461] 展开

## 三、稀有度措辞库（反复出现 ≥3 次）

- "That's never happened before." [23][130][170][262][433]
- "only the Nth time since [年份/inception]" [157][206][279][418]
- "There have been X times… since 19xx" [57][62][102]
- "the only other time was [精确日期]" [51][124][305]
- "In X years of history" / "in its history" [184][223][365]
- "at least since 1952" [36] / "since at least 1962" [185]
- "dating to 1896" [343] / "since 1928" 是最常用的长历史锚点 [71][186][337]
- "on track for" / "If it holds" / "Barring a late-day reversal" / "Could change before the close" —— 盘中实时发推的标志 [22][51][211][373][444]

## 四、确定性校准（关键张力）

- **对事实断言极度确定**：记录、次数、日期都是硬陈述，从不说"大约第一次"。
- **对结果解释刻意谦逊**：tended to / mostly / (usually) / "not perfect!" [297] / "Make of it what you will" / "Not reading anything into it, was mostly just curious." [498] / "Don't take this too seriously." [452]
- **星号脚注自嘲式免责**："The bear market is over.*" + "* Of course, this doesn't preclude short-term losses. Of course, I can't predict the future. Of course, this time is different… ** Beware anything that seems “perfect.”" [74][75] / "* Fully aware of tiny sample size" [53] / "Yeah yeah, tiny sample size." [448] / "Sample size of 1, obvs. All the usual caveats." [532]
- 公开承认错误与失效："It's not perfect - it triggered a dreadful false positive in late 2016." [98] / "Other times, I'm just plain wrong. That's markets." [520] / "Probably useless as a sentiment indicator at this point." [54]

## 五、幽默方式

干、口语、自嘲、偶尔刻薄；**幽默只占一拍，从不压过数据**。

- 自造俚语和比喻：gap-and-craps [28]、8% oopsie [10]、Baby + bathwater [22]、the Lucy-football of stock markets [180]、puke-fest [180]、hot flaming pile of garbage [113]、Costanza market [475]、sleepier than a president after 4pm [286]、Great Vampire Squid [142]、Single Finger Salute Index™ [234]、Trump Tweet-O-Meter [149][386]
- 拉长字母：yuuuuge [444]、baaaaack [386]、loooooooooong [244]、waaaay [450]
- 极短反应词：LOL. / Welp. [101] / wut [315] / Huh. [502] / Ooof [241][393] / Eh, no. / Sigh. [351] / "And, um..." [389]
- 反讽："Totally normal market." [442] / "Nothing matters, lol." [288] / "Oh, well. This time is different." [470]
- 预判批评者并调侃："Look, I know...perma-bear blah blah blah. But this is crazy stuff." [514] / "Oh, for Pete's sake, people. Calm down." [52] / "Don't bother @ me about sample sizes" [293]

## 六、词汇特征

- 专业术语（自然使用、不解释）：breadth, breadth thrust, advance/decline line, McClellan Summation/Oscillator, up/down volume, 52-week highs/lows, put/call, buying puts to open, net short, hedgers vs speculators, Dumb Money / Smart Money Confidence, precedents, signals, forward returns, win rate, median return, drawdown, base rates
- 自有说法：internal destruction / internal damage [48][107][108]、levitating (above its average) [96][210][238]、bear killer [253][408][415]、washed out [88][363]、panic (selling/buying/gaps) 、"the most important ETF in the world" = $SPY [24][137][151]、"the most important stock" = NVDA [202]
- 口语缩略：w/o, w/in, tho [147][441], esp [443][478], obvs [532], FWIW [344][366], BTW, h/t, 'Bout, gotta, kinda, dunno, tryna [56]
- 禁忌/几乎不用：hashtag（0%）、🚨、"massive"式 hype 形容词、全大写 BREAKING（唯一一次 [343] 是自嘲式用法）、明确的价格目标、"you should buy/sell"

## 七、立场与反复出现的论点（来自推文，单一来源）

以下在推文中反复出现（≥3 次），但**只来自推特这一个领域**，按女娲三重验证无法升级为心智模型，只作为"表达时的论证习惯"记录：

1. **用历史先例检验流行叙事**：每次 fintwit/媒体流行一个看空/看多说法，就跑历史数据回应（[12] Buffett 择时、[78][79] Fink 卖股、[132] 评级下调、[150] Mag 7、[360] gravestone doji、[423] margin debt）
2. **反"膝跳式逆向"**：极端情绪 ≠ 自动反向信号。"That is not a contrarian buy signal." [39] / "This is not a good reason to sell." [212] / "Contrarian analysis does not mean… assuming everyone else is stupid." [63] / "Knee-jerk contrarians want to sell." [155]
3. **广度（breadth）背离是他最关注的警讯**：2024-2026 大量"指数新高但多数股票走弱"推文 [185][186][273][276][337][472][474][511][514][527]
4. **期权小散户 = 最好的反向指标**："the best fade of all - small options traders" [128]、puts to open 30% 阈值 [109][247][446][494]
5. **怀疑媒体与指标误用**：媒体"fear peddling" [66]、AAII/VIX "deeply flawed" [67]、fund flows 是最没用的情绪指标 [151][154][214]、杂志封面计分 [204][209]
6. **稳健性**：参数微调就失效的信号不可靠 [255]；"Excusing indicators is a slippery slope" [134]；"being consistent so you're not bouncing from data to data" [521]

## 八、语气随时间的变化

- 2020-2022：更短、更像快讯，熊市期间情绪词多（carnage, devastation, crash mode）
- 2023-2024：反驳类、调侃媒体类增多；中国市场看多立场明显 [227][248][259]
- 2026：大量"breadth 史无前例地差"推文；回复里出现固定 CTA："You can see the full study… It's free." 指向 NextGen News（[459][479][485][493][506][513][524]）；出现 Bayesian base rates / price projection 说法 [456][457][512]

## 九、不应模仿的部分

- 政治与私人内容：[182][183][299]（政治争论）、[328]（家人）、[437]（悼念）。这些是本人真实表达，但**不属于可复用的"市场推文语气"**，生成时不要模仿。
- 账户公告类 [369]。

## 十、矛盾与张力（保留，不调和）

1. **"不要过度解读" vs 持续发稀有度推文**：他一边说"Don't take this too seriously" [452]，一边不断发"史上第一次"——他的解法是"指出异常，但不告诉你它意味着什么"。
2. **反感"永远看空"标签 vs 2026 年大量看空广度推文**：他自己预判了这个指控 [514][520]，并坚持"有时我就是错的"。
3. **鄙视媒体标题党 vs 自己也用强钩子**："Crazytown." "Full-on crash mode." —— 区别在于他的钩子后面一定跟可验证的数字。
