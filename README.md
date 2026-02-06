# LangEmotionHub API
Backend for a Multi-user Personalized LLM Platform (Fine-tuning + Inference + RAG)

LangEmotionHub API 是支援多使用者的個人化語言模型後端系統，負責模型微調流程、推論服務、Adapter 管理與記憶整合（RAG）。

本平台以語意鍛造為核心概念，透過大型語言模型模擬特定對象的語言風格，讓使用者能夠重現對話情境、保存回憶，並獲得更貼近個人風格的情感互動體驗。

前端 Web App 請參考：

👉 [LangEmotionHub-web](https://github.com/LinZKai/LangEmotionHub-web)

## 核心設計理念
- 共用 Base LLM（中文語言能力基礎）
- 每位使用者擁有獨立 LoRA Adapter（風格隔離）
- 推論時整合 Few-shot Prompting + RAG（情境增強）


## 技術架構
<img width="2048" height="1144" alt="image" src="https://github.com/user-attachments/assets/6f9f65ff-70dd-4395-802d-37131fee53f9" />

1. **Web App**
   - 上傳聊天紀錄（.txt / .csv）
   - 管理事件記事本（Memory）
   - 發送聊天 (inference) 請求

2. **Flask API Server**
   - 訓練與推論 API
   - 使用者模型管理
   - JWT Authentication 與帳號管理
   - GPU 記憶體控制與模型快取

3. **LLM**
   - 使用 Taiwan LLM 作為中文對話基礎模型
   - 透過 LoRA / QLoRA 生成個人化 Adapter

4. **RAG（ChromaDB）**
   - 檢索與當前 query 相關的事件內容
   - 作為 enhanced context 提供給模型


## 模型設計

### Base Model：Taiwan LLM

選用 Taiwan LLM 作為基礎模型，原因包括：

- 中文語境優化
- 使用本地新聞、社群文本、小說等進行訓練
- 本地部署可控性高

### QLoRA / LoRA 微調

為降低硬體需求並支援多使用者架構：

- 4-bit 量化（BitsAndBytes）
- Parameter-Efficient Fine-tuning（LoRA / QLoRA）
- Base Model 不被改寫
- 每位使用者產生獨立 Adapter


## Hybrid Memory 設計

本系統採三層記憶設計：

1.長期風格（Long-term Style Memory）
- 技術：LoRA Adapter  
- 功能：學習使用者語氣與說話方式

2.短期示例（Short-term Prompt Memory）
- 技術：Few-shot Prompting  
- 功能：保持當前對話回應風格

3.情境記憶（Contextual Memory）
- 技術：RAG  
- 功能：檢索與當前 query 相關的事件

此設計使模型在保留長期語言風格的同時，能根據當前情境生成更準確回應。

## Fine-tuning 效果展示
<img width="1630" height="912" alt="image" src="https://github.com/user-attachments/assets/003909ec-32d9-43ae-9dc2-aee665f4527f" />


## 專案背景與團隊
LangEmotionHub 為國立政治大學資訊管理學系 2024 年畢業專題專案，由團隊共同開發完成。

本 Repo 為後端部分，整理後於個人帳號重新建立，用於作品展示。
