---
layout: post
title: OpenAI API 的試用、收費、模型蒸餾與輸出 JSON
date: 2025-06-01 16:00:00 +0800
categories: [Python]
--- 

在 ChatGPT 上用得很開心，想要改用 OpenAI API 自動化嗎？那麼，請閱讀下面的文章，了解如何選擇合適的收費方案，並規範回答欄位。

### 免費試用的限制

- 只有使用部分模型，無法使用的模型會顯示 Free (Tier) Not supported。
- 以 `gpt-4o-mini` 為例，一分鐘只能送出 3 次請求，最多 20,000 個 Token。
- 沒辦法使用比較特殊的模型：
    - 例如包含網路搜尋功能的 model：[Web search - OpenAI API](https://platform.openai.com/docs/guides/tools-web-search?api-mode=chat) ，從網路搜尋取得更精確、即時的結果。

- 可參考 [Models - OpenAI API](https://platform.openai.com/docs/models ) 查看各種模型的使用限制。

### 收費方式

各模型的收費方式可參考：[Pricing - OpenAI API](https://platform.openai.com/docs/pricing)，例如 gpt-4o 每 1M Token 輸入收費是 2.5 美元、輸出 1M Token 是 10 美元。不過若要使用有特殊功能的模型，例如 Web Search，則還要另外收費，例如 gpt-4o-search-preview 呼叫 1K 次搜尋的收費是 35 美元  (紀錄於 2025 年 3 月)。

此外，OpenAI API 和 ChatGPT Pro 是不同的收費方案 (詳情請見 [What is ChatGPT Pro? - OpenAI Help Center](https://help.openai.com/en/articles/9793128-what-is-chatgpt-pro))，需要留意。

### 模型蒸餾

模型蒸餾指的是將大型模型 (例如 GPT-4o) 的輸出結果用於微調較小的模型 (例如 GPT-4o-mini)，讓較小的模型在特定任務上能有類似 (大型模型) 的表現。因為小型模型計算比較有效率，可以節省成本和延遲。

大概有以下的步驟：

1. 儲存高品質的輸出 (回答)，在 `client.chat.completions` 指定 `store = true`。
2. 同時用大型、小型模型評估儲存的結果。
3. 選擇需要的輸出，建立訓練資料，來微調小型模型。
4. 評估微調後的模型與大型模型相比的表現。

<br>

- 詳細說明請參考官方說明：[Model distillation - OpenAI API](https://platform.openai.com/docs/guides/distillation)
- 這項功能大約是在 2024/10 發表的：[SBIR-智財新知：OpenAI DevDay 發表四大功能，Realtime API 助開發者建立AI 語音對話](https://www.sbir.org.tw/ipcc/news_content?id=11849&page=1)

### 輸出 JSON 結構的回答

- GPT 模型的回答基本上會是文章或條列式的內容，雖然我們可以在提示中說明要輸出 JSON 格式，偶爾還是會輸出不符合格式的內容。可以用 structured output 功能，明確要求輸出符合欄位格式的 JSON 資料。
    - 詳細說明：[OpenAI 能輸出你想要的格式 (JSON Schema) · YWC 科技筆記](https://ywctech.net/ml-ai/openai-structued-output-json-schema/#python-sdk)
    - 官方指引：[Structured Outputs - OpenAI API](https://platform.openai.com/docs/guides/structured-outputs?api-mode=chat)
    - 在 Python 內操作時，如果需要多組自訂格式的輸出，可以使用 [Pydantic](https://docs.pydantic.dev/latest/) 搭配 list 達成。

- 以下是範例：

```python
class Classmate(BaseModel):
    Id: str
    Name: str

class ClassmateList(BaseModel):
    MateList: list[Classmate]

completion = client.beta.chat.completions.parse(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": "請產生幾位學生的 ID 和名字。"}
        ],
        store = True,
        response_format = ClassmateList
    )

    print(completion.choices[0].message)
```

<br>

Note: Pydantic 可以建立帶有型別提示的類別，可以用於資料驗證，例如：[資料驗證（上）Pydantic 單一欄位驗證 - Code and Me](https://blog.kyomind.tw/django-ninja-19/ )、[pydantic 小筆記 - 六小編 Editor Leon](https://editor.leonh.space/2023/pydantic/)、[Pydantic 的介紹. 簡單使用 Pydantic，像是 List, Dict… - by Kiwi lee - Medium](https://sean22492249.medium.com/pydantic-%E7%9A%84%E4%BB%8B%E7%B4%B9-3721a0691162)。

### Bonus: 中文 OpenAI API 功能介紹與教學

- [\[D8\] OpenAI API 入門 - Chat Completion 訊息角色 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10317113)
- [ChatGPT/OpenAI程式設計 - HackMD](https://hackmd.io/@aaronlife/python-topic-openai)