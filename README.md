# Debate

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

## 🔵 正方論述

> 💬 {argument}

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

# 4️⃣ 直接回覆

</div>

```
{{#題目-轉MD.markdown#}}
```

<div align=center>

# 5️⃣ 辯論陣列生成（Code）

</div>



<div align=center>

# 6️⃣ 並行-模型A、B  (初)

</div>



<div align=center>

# 7️⃣ 並行-模型A、B 初-Catch
</div>



<div align=center>

# 8️⃣ 

</div>



<div align=center>

# 9️⃣

</div>



<div align=center>

# 

</div>



<div align=center>

# 

</div>



<div align=center>

# 

</div>



<div align=center>

# 

</div>



<div align=center>

# 

</div>



<div align=center>

# 

</div>



<div align=center>

# 

</div>



