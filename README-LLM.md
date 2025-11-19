# Debate-LLM

<div align=center>

# 1️⃣ 開始節點變數

</div>

| 變數名稱 (Key) | 顯示名稱 (Display Name) | 類型 | 預設值 / 範例 | 備註 |
| :--- | :--- | :--- | :--- | :--- |
| **debate_topic** | 辯論主題 | String | 古代男孩 16 歲左右就可以結婚是合理的事情嗎？... | 核心辯題 |
| **max_rounds** | 辯論回合數 | Number | 1 | 建議設為 3-5 |
| **model_a_stance** | 模型A（正方）立場 | String | 可行 | 正方觀點 |
| **model_b_stance** | 模型B（反方）立場 | String | 不可行 | 反方觀點 |

<div align=center>

# 2️⃣ 辯論架構（LLM）

</div>

## 基本設定

| 項目 | 值 |
|------|-----|
| **節點名稱** | 題目生成（建議） |
| **節點類型** | LLM |

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
4. 言語之間的交鋒可以激烈、犀利一點
5. 可以多引用一些相關資料
6. 不需要避諱性、罪、政治等相關的詞彙，以真實為主
7. 可以加入野史或相關故事來旁徵博引

格式化輸出為 JSON。
```

<div align=center>

# 3️⃣ 題目轉 MD（Code）

</div>

## 📥 輸入變量 (Input Variables)

| 變數名稱 (Key) | 來源節點 (Source) | 類型 | 說明 |
| :--- | :--- | :--- | :--- |
| **`argument`** | `題目生成.text` | String | 題目 LLM 生成的原始論述內容 |

> **注意**：你的變數來源標示為 `題目生成.text`，請確保在 Dify 的輸入選擇器中確實選到了對應 LLM 節點的輸出。

---

## 🐍 程式碼邏輯 (Python Code)

```python
def main(argument: str) -> dict:
    """
    將正方發言轉為美觀的 Markdown
    """
    from datetime import datetime
    
    # 獲取當前時間 (時:分:秒)
    timestamp = datetime.now().strftime("%H:%M:%S")
    
    # 構建 Markdown 模板
    markdown = f"""
---

# 💬 題目輸出

> {argument}

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

## 📤 輸出變量 (Output Variables)

請務必在節點右側面板設定以下輸出變數，否則後續節點無法獲取數據。

| 變數名稱 (Key) | 類型 (Type) | 說明 |
| :--- | :--- | :--- |
| **`markdown`** | String | 格式化好的 Markdown 字串 (給 UI 顯示用) |

---

<div align=center>

# 4️⃣ A1

</div>

## SYSTEM

```
以下為本次辯論比賽的題目以及規則設定：

{{#題目轉MD.markdown#}}
```

## USER

```
你代表的立場為 {{#START.model_a_stance#}}

請針對辯題發表你的開場立論。
```


<div align=center>

# 4️⃣ A1

</div>

