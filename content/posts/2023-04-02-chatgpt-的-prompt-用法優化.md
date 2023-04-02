---
title: ChatGPT 的 Prompt 用法優化
date: 2023-04-02T06:22:50.135Z
author: Boison
slug: chatgpt-prompt-101
tags:
  - AI
  - ChatGPT
draft: false
---
## 一、基礎

### 1. 問答：精確，正向定義

* 使用精確量詞
* 與其用否定限定，用正向限定對其可能更直接

  * 否定

    1. Please suggest me some essential words for IELTS
    2. Please recommend me some places to visit in Hong Kong. Do not recommend museums.
  * 正向

    1. Please suggest me 10 essential words for IELTS
    2. Please recommend me some places to visit in Hong Kong including amusement parks.

資料來源：[Learning Prompt](https://learningprompt.wiki/)

- - -

### 2. 造句：給 AI 參考用的答案，請他自己學習造樣造句

```javascript
Suggest three names for an animal that is a superhero.

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline

Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot

Animal: Horse
Names: ...
```

```javascript
Product description: A home milkshake maker
Seed words: fast, healthy, compact.
Product names: HomeShaker, Fit Shaker, QuickShake, Shake Maker

Product description: A pair of shoes that can fit any foot size.
Seed words: adaptable, fit, omni-fit.
Product names: ...
```

```javascript
Convert movie titles into emoji.

Back to the Future: 👨👴🚗🕒
Batman: 🤵🦇
Transformers: 🚗🤖
Star Wars: ...
```

- - -

### 3. 總結：請 AI 總結時把給 AI 的文本用符號標示

```javascript
Please summarize the following sentences to make them easier to understand.

Text: """
OpenAI is an American artificial intelligence (AI) research laboratory consisting of the non-profit OpenAI Incorporated (OpenAI Inc.) and its for-profit subsidiary corporation OpenAI Limited Partnership (OpenAI LP). OpenAI conducts AI research with the declared intention of promoting and developing a friendly AI. OpenAI systems run on the fifth most powerful supercomputer in the world.[5][6][7] The organization was founded in San Francisco in 2015 by Sam Altman, Reid Hoffman, Jessica Livingston, Elon Musk, Ilya Sutskever, Peter Thiel and others,[8][1][9] who collectively pledged US$1 billion. Musk resigned from the board in 2018 but remained a donor. Microsoft provided OpenAI LP with a $1 billion investment in 2019 and a second multi-year investment in January 2023, reported to be $10 billion.[10]
"""
```

- - -

### 4. 總結：請 AI 總結時把要 AI 輸出的格式標示清楚

```javascript
Extract the important entities mentioned in the article below. First extract all company names, then extract all people names, then extract specific topics which fit the content and finally extract general overarching themes

Desired format:

1. Company names: <comma_separated_list_of_company_names>
2. People names: -||-
3. Specific topics: -||-
4. General themes: -||-





Text: """Powering Next Generation
Applications with OpenAI Codex
Codex is now powering 70 different applications across a variety of use cases through the OpenAI API.

May 24, 2022
4 minute read
OpenAI Codex, a natural language-to-code system based on GPT-3, helps turn simple English instructions into over a dozen popular coding languages. Codex was released last August through our API and is the principal building block of GitHub Copilot.

Warp is a Rust-based terminal, reimagined from the ground up to help both individuals and teams be more productive in the command-line.

Terminal commands are typically difficult to remember, find and construct. Users often have to leave the terminal and search the web for answers and even then the results might not give them the right command to execute. Warp uses Codex to allow users to run a natural language command to search directly from within the terminal and get a result they can immediately use.

“Codex allows Warp to make the terminal more accessible and powerful. Developers search for entire commands using natural language rather than trying to remember them or assemble them piecemeal. Codex-powered command search has become one of our game changing features.”

—Zach Lloyd, Founder, Warp


Machinet helps professional Java developers write quality code by using Codex to generate intelligent unit test templates.

Machinet was able to accelerate their development several-fold by switching from building their own machine learning systems to using Codex. The flexibility of Codex allows for the ability to easily add new features and capabilities saving their users time and helping them be more productive.

“Codex is an amazing tool in our arsenal. Not only does it allow us to generate more meaningful code, but it has also helped us find a new design of product architecture and got us out of a local maximum.”

—Vladislav Yanchenko, Founder, Machinet"""
```

```javascript
Summarize the main points of the following speech

Use the following format:

Topic 1: <topic_name_1>
- <point_1>
..

Topic 2: <topic_name_2>
- <point_1>
..

Topic 10: ..






Text: """
Thank you so much, Fred, for that lovely introduction. And thanks to the Atlantic Council for hosting me today.

The course of the global economy over the past two years has been shaped by COVID-19 and our efforts to fight the pandemic. It’s now evident, though, that the war between Russia and Ukraine has redrawn the contours of the world economic outlook. Vladimir Putin’s unprovoked attack on Ukraine and its people is taking a devastating human toll, with lives tragically lost, families internally displaced or becoming refugees, and communities and cities destroyed.
...

"""
```

- - -

## 二、進階

### 1. 加上 `Let‘s think step by step`

原本是錯誤的，但在問題最後加入 `Let‘s think step by step` 這句話之後，模型就生成了正確的答案，原因如下：

算出來的答案錯誤的原因，只是因為它在中間跳過了一些步驟（B）。而讓模型一步步地思考，則有助於其按照完整的邏輯鏈（A > B > C）去運算，而不會跳過某些假設，最後算出正確的答案。

&nbsp;

### 2. openai 的 Playground 設定

1. **Mode**

   * 最近跟新了第四種 Chat 模式，一般使用 Complete 就好
   * 當然你可以用其他模式，其他模式能通過GUI 的方式輔助你撰寫prompt。
2. **Model**\
   這裡可以切換模型。不同的模型會擅長不同的東西，根據場景選對模型，能讓你省很多成本：

   * Ada\
     這是最便宜，但運算速度最快的模型。\
     官方推薦的使用場景是解析文本，簡單分類，地址更正等。
   * Babbage\
     這個模型能處理比Ada 複雜的場景。\
     但稍微貴一些，速度也比較快。適合分類，語義搜索等。
   * Curie\
     這個模型官方解釋是「和Davinci 一樣能力很強，且更便宜的模型」。\
     但實際上，這個模型非常擅長文字類的任務，比如寫文章、語言翻譯、撰寫總結等。
   * Davinci\
     這是 GPT-3 系列模型中能力最強的模型。\
     可以輸出更高的質量、更長的回答。每次請求可處理4000 個token。適合有復雜意圖、因果關係的場景，還有創意生成、搜索、段落總結等。
3. **Temperature**

   * 這個主要是控制模型生成結果的隨機性。
   * 簡而言之，溫度越低，結果越確定，但也會越平凡或無趣。
   * 如果你想要得到一些出人意料的回答，不妨將這個參數調高一些。但如果你的場景是基於事實的場景，比如數據提取、FAQ 場景，此參數就最好調成0 。
4. **Maximum length**

   * 設置單次生成內容的最大長度。
5. **Stop Sequence**

   * 該選項設置停止生成文本的特定字符串序列。如果生成文本中包含此序列，則模型將停止生成更多文本。
6. **Top P**

   * 該選項是用於 nucleus 採樣的一種技術，它可以控制模型生成文本的概率分佈，從而影響模型生成文本的多樣性和確定性。
   * 如果你想要準確的答案，可以將它設定為較低的值。如果你想要更多樣化的回复，可以將其設得高一些。
7. **Presence Penalty**

   * 該選項控制模型生成文本時是否避免使用特定單詞或短語，它可以用於生成文本的敏感話題或特定場景。
8. **Best of**

   * 這個選項允許你設置生成多少個文本後，從中選擇最優秀的文本作為輸出。默認為1，表示只生成一個文本輸出。
9. **Injection start text**

   * 這個選項可以讓你在輸入文本的開頭添加自定義文本，從而影響模型的生成結果。
10. **Injection restart text**

    * 這個選項可以讓你在中間某個位置添加自定義文本，從而影響模型繼續生成的結果。
11. **Show probabilities**

    * 這個選項可以讓你查看模型生成每個單詞的概率。
    * 打開此選項後，你可以看到每個生成的文本單詞後面跟著一串數字，表示模型生成該單詞的概率大小。

&nbsp;

&nbsp;

- - -

&nbsp;

> 資料來源：
>
> 1. [OpenAI 的 API 最佳實踐文檔](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)
> 2. [Learning Prompt](https://learningprompt.wiki/)
> 3. [Elavis Saravia 總結的框架](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-intro.md)
> 4. [Large Language Models are Zero-Shot Reasoners](https://arxiv.org/abs/2205.11916)