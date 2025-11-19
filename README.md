# Debate

<div align=center>

# 0️⃣ 對話變數

</div>

這份筆記整理了你的 **對話變數 (Conversation Variables)** 設定。這些變數是「全域」的，意味著它們會在整場對話中一直存在，並且可以被更新（Read/Write）。

這對於你的**多輪辯論**至關重要，因為你需要它們來記住「目前辯論到哪裡了」以及「雙方的記憶 ID」。

以下是整理好的表格：

---

# 💾 對話變數設定 (Conversation Variables)

這些變數用於在多次對話輪次中保持狀態，確保模型 A 和模型 B 的記憶連貫，並儲存 API 認證資訊。

### 1. 認證與連線 (Authentication)

| 變數名稱 (Name) | 類型 (Type) | 預設值 (Default) | 說明 |
| :--- | :--- | :--- | :--- |
| **`a_cookie`** | String | `share_token=3494...` (長字串) | **模型 A** 的 HTTP 請求 Cookie，包含 Token 和 Mixpanel 資訊。 |
| **`b_cookie`** | String | `share_token=b42f...` (長字串) | **模型 B** 的 HTTP 請求 Cookie，包含 Token 和 Mixpanel 資訊。 |

> **⚠️ 注意**：這些 Cookie 包含敏感資訊且長度很長，確保在 HTTP 請求 Header 中正確引用 `{{conversation.a_cookie}}`。

---

### 2. 對話狀態與記憶 (State & Memory)

這些變數通常初始為空，隨著辯論進行，會被 Workflow 中的「變數賦值器 (Variable Assigner)」不斷更新。

| 變數名稱 (Name) | 類型 (Type) | 初始狀態 | 說明 |
| :--- | :--- | :--- | :--- |
| **`a_conversation_id`**| String | (空) | 記錄 **模型 A** 當前在 Replit/API 端的對話 ID。 |
| **`a_response_id`** | String | (空) | 記錄 **模型 A** 上一次回應的 Message ID (用於追蹤)。 |
| **`a_result`** | String | (空) | 暫存 **模型 A** 最近一次的發言內容。 |
| **`b_conversation_id`**| String | (空) | 記錄 **模型 B** 當前在 Replit/API 端的對話 ID。 |
| **`b_response_id`** | String | (空) | 記錄 **模型 B** 上一次回應的 Message ID (用於追蹤)。 |
| **`b_result`** | String | (空) | 暫存 **模型 B** 最近一次的發言內容。 |

---

### 🔄 變數流轉邏輯 (Workflow Logic)

為了讓你更清楚這些變數怎麼用，以下是它們在工作流中的生命週期：

1.  **初始化**：第一次執行時，`a_conversation_id` 為空，系統可能會使用 Start 節點的 `a_orig_converID` 作為備案。
2.  **HTTP 請求 (A)**：
    *   **Input**: 使用 `a_conversation_id` (若有值)。
    *   **Header**: 使用 `a_cookie`。
3.  **更新變數 (A)**：
    *   API 回傳後，將新的 ID 寫入 `a_conversation_id`。
    *   將回應內容寫入 `a_result`。
4.  **HTTP 請求 (B)**：
    *   **Input**: 使用 `b_conversation_id` 和 `a_result` (作為靶子)。
    *   **Header**: 使用 `b_cookie`。
5.  **更新變數 (B)**：
    *   API 回傳後，更新 `b_conversation_id` 和 `b_result`。

這樣設計能確保無論使用者何時按「繼續」，雙方的記憶都是連貫的！

<div align=center>

# 1️⃣ 開始節點變數

</div>

| 變數名稱 (Key) | 顯示名稱 (Display Name) | 類型 | 預設值 / 範例 | 備註 |
| :--- | :--- | :--- | :--- | :--- |
| **debate_topic** | 辯論主題 | String | 古代男孩 16 歲左右就可以結婚是合理的事情嗎？... | 核心辯題 |
| **max_rounds** | 辯論回合數 | Number | 1 | 建議設為 3-5 |
| **model_a_stance** | 模型A（正方）立場 | String | 可行 | 正方觀點 |
| **model_b_stance** | 模型B（反方）立場 | String | 不可行 | 反方觀點 |
| **a_model** | a_model | String | grok-4 | A 使用的模型 |
| **b_model** | b_model | String | grok-4-1-non-thinking-w-tool | B 使用的模型 |
| **a_mode** | a_mode | String | MODEL_MODE_EXPERT | A 的模式設定 |
| **b_mode** | b_mode | String | MODEL_MODE_GROK_4_1 | B 的模式設定 |
| **a_orig_converID** | 模型A-對話ID | String | bfea2648-92c5-4a7f-86f1-cd147773a20f | A 的記憶錨點 |
| **b_orig_converID** | 模型B-對話ID | String | 3ce49b23-66a0-447e-b7b9-b9155f24ee30 | B 的記憶錨點 |
| **a_orig_respondID**| 模型A-前回應ID | String | 9a7791e6-1a12-4e50-a105-070228292f88 | 用於接續對話 |
| **b_orig_respondID**| 模型B-前回應ID | String | 10e7b966-fa75-4b73-9670-785f0bda2c39 | 用於接續對話 |
| **URL** | 網址 URL | String | `https://...replit.dev/api/chat` | API 端點 |

<div align=center>

# 2️⃣ 辯論架構（LLM）

</div>

## 基本設定

| 項目 | 值 |
|------|-----|
| **節點名稱** | 題目生成（建議） |
| **節點類型** | LLM |
| **模型** | claude-haiku-4.5-thinking |

---

## 輸入變數

| 變數名稱 | 來源 | 說明 |
|---------|------|------|
| `debate_topic` | `{{#start.debate_topic#}}` | 辯論主題 |
| `model_a_stance` | `{{#start.model_a_stance#}}` | 正方立場 |
| `model_b_stance` | `{{#start.model_b_stance#}}` | 反方立場 |
| `sys.query` | `{{#start.sys.query#}}` | 系統查詢（可選） |

---

## SYSTEM 提示詞

```
系統提示詞:
請為以下辯論生成開場白和辯論框架：

辯題：{{#start.debate_topic#}}
正方立場：{{#start.model_a_stance#}}
反方立場：{{#start.model_b_stance#}}
```

### USER 提示詞

```
請以繁體中文 輸出：
1. 辯論背景說明
2. 雙方需要論證的核心觀點
3. 評判標準
4. {{#sys.query#}}

格式化輸出為 JSON。
```

<div align=center>

# 3️⃣ 題目轉 MD（Code）

</div>

### 📥 輸入變量 (Input Variables)

| 變數名稱 (Key) | 來源節點 (Source) | 類型 | 說明 |
| :--- | :--- | :--- | :--- |
| **`argument`** | `題目生成.text` | String | 正方 LLM 生成的原始論述內容 |

> **注意**：你的變數來源標示為 `題目生成.text`，請確保在 Dify 的輸入選擇器中確實選到了對應 LLM 節點的輸出。

---

### 🐍 程式碼邏輯 (Python Code)

```python
def main(argument: str) -> dict:
    """
    將正方發言轉為美觀的 Markdown
    """
    from datetime import datetime
    
    # 獲取當前時間 (時:分:秒)
    timestamp = datetime.now().strftime("%H:%M:%S")
    
    # 構建 Markdown 模板
    # 使用了藍色圓點 🔵 代表正方，引用區塊 > 代表對話
    markdown = f"""
---

# 💬 題目輸出

 {argument}

<sub>⏰ {timestamp}</sub>

---
"""
    
    # 回傳字典，包含格式化後的 MD 和原始文字
    return {
        "markdown": markdown,
        "text": argument
    }
```

---

### 📤 輸出變量 (Output Variables)

請務必在節點右側面板設定以下輸出變數，否則後續節點無法獲取數據。

| 變數名稱 (Key) | 類型 (Type) | 說明 |
| :--- | :--- | :--- |
| **`markdown`** | String | 格式化好的 Markdown 字串 (給 UI 顯示用) |
| **`text`** | String | 原始純文字 (如果後續需要純文字分析可用) |

---

<div align=center>

# --> 直接回覆

</div>

```
{{#題目-轉MD.markdown#}}
```

<div align=center>

# 4️⃣ 辯論陣列生成（Code）

</div>

### ⚙️ 基本設定 (Configuration)

| 設定項目 | 值 / 說明 |
| :--- | :--- |
| **節點類型** | Code (Python 3) |
| **功能** | 生成迭代陣列 (Array Generation) |
| **失敗時重試** | **開啟** (建議，雖然這段代碼極少失敗) |

---

### 📥 輸入變量 (Input Variables)

| 變數名稱 (Key) | 來源節點 (Source) | 類型 | 說明 |
| :--- | :--- | :--- | :--- |
| **`max_rounds`** | `開始.max_rounds` | Number | 辯論的總回合數 (例如: 5) |

---

### 🐍 程式碼邏輯 (Python Code)

```python
def main(max_rounds: int) -> dict:
    """
    生成迭代用的數字陣列
    例如 max_rounds = 3 -> 回傳 [1, 2, 3]
    """
    # 簡單直接：生成範圍從 1 到 max_rounds 的數字列表
    rounds = list(range(1, max_rounds + 1))
    
    return {
        "rounds_array": rounds 
    }
```

---

### 📤 輸出變量 (Output Variables)

請務必在節點右側面板設定正確的變數類型，這是最容易出錯的地方。

| 變數名稱 (Key) | 類型 (Type) | 說明 |
| :--- | :--- | :--- |
| **`rounds_array`** | **Array[Number]** | 純數字陣列 (例如 `[1, 2, 3]`) |

> **⚠️ 特別注意**：輸出類型一定要選 **Array[Number]**，不能選 Array[String] 或 Object，否則後面的迭代節點會因為類型不符而報錯 (`The element ... must be a number`)。

<div align=center>

# 5️⃣ 並行-模型A、B  (初)

</div>

## 🌐 HTTP 節點：模型請求

此節點負責將「題目生成」節點產生的 Prompt 發送給外部 API（Model A），以獲取正方的論述。

### ⚙️ 基本設定 (Configuration)

| 設定項目 | 值 / 說明 |
| :--- | :--- |
| **方法 (Method)** | `POST` (預設) |
| **URL** | `{{#start.URL#}}` (引用開始節點的變數) |
| **鑑權 (Auth)** | 無 (None) - *透過 Body 或 Header 自行傳遞* |
| **驗證 SSL** | 開啟 (Enabled) |

---

### 📨 請求標頭 (Headers)

| 鍵 (Key) | 值 (Value) |
| :--- | :--- |
| **Content-Type** | `application/json` |

---

### 📦 A - 請求內容 (Body) - JSON

```json
{
  "message": "{{#題目生成.text#}}",
  "model": "{{#start.a_model#}}",
  "model_mode": "{{#start.a_mode#}}",
  "conversation_id": "{{#start.a_orig_converID#}}",
  "parent_response_id": "{{#start.a_orig_respondID#}}",
  "cookie": "{{#conversation.a_cookie#}}"
}
```

---

### 📦 B - 請求內容 (Body) - JSON

```json
{
  "message": "{{#題目生成.text#}}",
  "model": "{{#start.b_model#}}",
  "model_mode": "{{#start.b_mode#}}",
  "conversation_id": "{{#start.b_orig_converID#}}",
  "parent_response_id": "{{#start.b_orig_respondID#}}",
  "cookie": "{{#conversation.b_cookie#}}"
}
```


<div align=center>

# 6️⃣ 並行-模型A、B 初-Catch

</div>

此節點負責解析 API 回傳的 JSON 資料，提取模型的回應內容及更新後的對話 ID。

## ⚙️ 基本設定 (Configuration)

| 設定項目 | 值 / 說明 |
| :--- | :--- |
| **節點類型** | Code (Python 3) |
| **功能** | JSON 解析 (JSON Parsing) |
| **失敗時重試** | **開啟** (建議，避免因為網路波動導致單次解析失敗) |

---

### 📥 輸入變量 (Input Variables)

| 變數名稱 (Key) | 來源節點 (Source) | 類型 | 說明 |
| :--- | :--- | :--- | :--- |
| **`api_response`** | `模型A (初).body` | String | HTTP 請求回傳的完整內容 |

> **⚠️ 注意**：來源節點名稱為 `模型A (初)`，這應該是你上一個 HTTP 節點的名稱。請確保選擇的是 `body` 屬性。

---

### 🐍 程式碼邏輯 (Python Code)

這段代碼有做基本的防呆處理，能應對成功和失敗的情況。

```python
import json

def main(api_response: str) -> dict:
    """
    更新對話上下文
    從 API 回應中提取 conversation_id 和 response_id
    """
    try:
        # 1. 解析 JSON：兼容字串或字典輸入
        data = json.loads(api_response) if isinstance(api_response, str) else api_response
        
        # 2. 判斷 API 是否成功 (假設 API 回傳包含 success 欄位)
        if data.get("success"):
            result_data = data.get("data", {})
            
            # 3. 提取關鍵資訊
            return {
                "answer": result_data.get("response", ""),
                "new_conversation_id": result_data.get("conversation_id", ""),
                "new_response_id": result_data.get("response_id", ""),
                "is_new": result_data.get("is_new_conversation", False)
            }
        else:
            # API 回傳失敗訊息
            return {
                "answer": f"錯誤：{data.get('error', '未知錯誤')}",
                "new_conversation_id": "",
                "new_response_id": "",
                "is_new": False
            }
            
    except Exception as e:
        # 程式解析炸裂 (例如 JSON 格式錯誤)
        return {
            "answer": f"解析錯誤：{str(e)}",
            "new_conversation_id": "",
            "new_response_id": "",
            "is_new": False
        }
```

---

### 📤 輸出變量 (Output Variables)

| 變數名稱 (Key) | 類型 (Type) | 說明 |
| :--- | :--- | :--- |
| **`answer`** | String | 模型的回答內容 (最重要！) |
| **`new_conversation_id`** | String | 更新後的對話 ID |
| **`new_response_id`** | String | 更新後的回應 ID |
| **`is_new`** | Boolean | 是否為新對話 |

<div align=center>

# 7️⃣ 變數賦值器：更新 A、B 記憶 (Update A Memory)

</div>

## ⚙️ 基本設定 (Configuration)

| 設定項目 | 值 / 說明 |
| :--- | :--- |
| **節點類型** | 變數賦值器 (Variable Assigner) |
| **功能** | 寫入模式 (Write Mode) |

---

## 🔄 賦值邏輯 (Assignment Logic)

| 目標變數 (Target Variable) | 操作 (Operation) | 來源值 (Source Value) | 說明 |
| :--- | :--- | :--- | :--- |
| **`conversation.a_response_id`** | **覆寫 (Set)** | `A初-CATCH.new_response_id` | 更新 A 的回應 ID |
| **`conversation.a_conversation_id`** | **覆寫 (Set)** | `A初-CATCH.new_conversation_id` | 更新 A 的對話 ID (最重要!) |

<div align=center>

# 8️⃣ A、B 初轉 MD

</div>

## 📥 輸入變量 (Input Variables)

| 變數名稱 (Key) | 來源節點 (Source) | 類型 | 說明 |
| :--- | :--- | :--- | :--- |
| **`argument`** | `A初-CATCH.answer` | String | 正方 LLM 生成的原始論述內容 |

---

## 🐍 程式碼邏輯 (Python Code)

### A-Code

```python
def main(argument: str) -> dict:
    """
    將正方發言轉為美觀的 Markdown
    """
    from datetime import datetime
    
    timestamp = datetime.now().strftime("%H:%M:%S")
    
    markdown = f"""
---

## 🔵 正方論述

> 💬 {argument}

<sub>⏰ {timestamp}</sub>

---
"""
    
    return {
        "markdown": markdown,
        "text": argument
    }
```

---

### B-Code

```python
def main(argument: str) -> dict:
    """
    將反方發言轉為美觀的 Markdown
    """
    from datetime import datetime
    
    timestamp = datetime.now().strftime("%H:%M:%S")
    
    markdown = f"""
---

## 🔴 反方論述

> 💬 {argument}

<sub>⏰ {timestamp}</sub>

---
"""
    
    return {
        "markdown": markdown,
        "text": argument
    }
```

### 📤 輸出變量 (Output Variables)

請務必在節點右側面板設定以下輸出變數，否則後續節點無法獲取數據。

| 變數名稱 (Key) | 類型 (Type) | 說明 |
| :--- | :--- | :--- |
| **`markdown`** | String | 格式化好的 Markdown 字串 (給 UI 顯示用) |
| **`text`** | String | 原始純文字 (如果後續需要純文字分析可用) |

---

<div align=center>

# 9️⃣ --> 直接回覆

</div>



<div align=center>

# 🔟 迭代

</div>




<div align=center>

## 

</div>



<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>


<div align=center>

## 

</div>
