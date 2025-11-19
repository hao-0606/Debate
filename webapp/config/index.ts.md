```ts
import type { AppInfo } from '@/types/app'
export const APP_ID = `${process.env.NEXT_PUBLIC_APP_ID}`
export const API_KEY = `${process.env.NEXT_PUBLIC_APP_KEY}`
export const API_URL = `${process.env.NEXT_PUBLIC_API_URL}`
export const APP_INFO: AppInfo = {
  title: 'AI 辯論',  // 標題名稱
  description: '丟出一個主題，讓 AI 進行辯論，並有評委進行評分',  // 簡述主題
  copyright: 'hao-0606',  // 顯示在著作權聲明處（不標示的話就是主題名稱）
  privacy_policy: '',
  default_language: 'zh',　// 預設語言
  disable_session_same_site: true, // set it to true if you want to embed the chatbot in an iframe
}

export const isShowPrompt = false
export const promptTemplate = 'I want you to act as a javascript console.'

export const API_PREFIX = '/api'

export const LOCALE_COOKIE_NAME = 'locale'

export const DEFAULT_VALUE_MAX_LEN = 48
```
