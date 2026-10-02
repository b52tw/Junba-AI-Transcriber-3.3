# 峻爸 AI Transcriber v3.3｜最佳化核對版

Windows 10/11 桌面語音轉文字工具。介面顯示作者名稱為「峻爸」，內部 EXE、快取、設定與 GitHub 名稱仍保留 Junba，以相容既有版本。

## v3.3 核心目標

1. **最佳化優先**：一般使用者只需選「最佳化（推薦）」後加入音檔並開始，不必懂 Whisper 模型、OpenVINO、CUDA 或 NPU。
2. **跨電腦自適應**：依 NVIDIA CUDA、Intel GPU/NPU、Intel/AMD CPU、RAM 與本機歷史速度（RTF）選擇路徑；失敗會降級到較穩定裝置。
3. **改善舊款 Intel Iris Xe / UHD 長時間卡住**：自適應初次使用會偏向 CPU INT8，模型也會保守選 small/medium；有實測 RTF 後再依該電腦真正速度調整。
4. **長錄音自動切段**：最佳化開啟時，切割設為 0 代表「自動切段」，不是整支 80 分鐘音檔一次丟進模型。舊款 iGPU/CPU/大模型會用較短區段，提高進度可見性與續跑能力。
5. **音檔相容預檢**：加入檔案後先做 FFmpeg 解碼檢查；讀不到的 M4A/AAC/容器會自動轉為 16 kHz 單聲道 PCM WAV 再處理。OneDrive 雲端佔位檔若尚未下載，會給出明確提示。
6. **錄音核對播放器**：預設輸出 HTML + VTT。開啟 HTML 後可播放原始錄音、搜尋逐字稿、點文字跳到時間點、播放時自動反白目前段落。若搬移檔案導致音訊路徑失效，可在播放器內重新選擇音檔。
7. **混合模式最佳化**：本機 Whisper 先完成有時間軸的逐字稿，再把文字送 Gemini 整理。Gemini 失敗/繁忙不會丟掉本機結果。Word 可同時保留「Gemini 整理後全文」與「時間軸逐字稿（錄音核對用）」。
8. **立即停止仍會輸出**：停止後整理已完成內容並產生可開啟的中止版文件/字幕/核對播放器。

## 一鍵使用方式

- **最佳化（推薦）**：有 Gemini API Key 時自動使用混合模式；沒有 Key 時自動改用完全離線。模型、硬體與切段都自動。
- **完全離線穩定**：不把音訊或文字送上網路，使用本機自適應 Whisper。
- **線上 Gemini 快速**：直接使用 Gemini 音訊轉錄並自動切段。
- **進階自訂**：需要時才手動指定引擎、模型、硬體、切割分鐘數。

## 核對播放器輸出

完成後會看到例如：

- `會議_逐字稿.docx`
- `會議_逐字稿_字幕.vtt`
- `會議_逐字稿_錄音核對.html`

HTML 頁面直接用瀏覽器開啟即可。點選任一段文字會把錄音跳到該時間；播放錄音時目前段落會自動反白。

## GitHub Actions 建置

Workflow 必須位於：

`.github/workflows/build-windows-v3.3.yml`

Actions → **Build Windows EXE v3.3** → Run workflow。

建議 Intel NPU/GPU / OpenVINO 優先下載：

`Junba-AI-Transcriber-v3.3-Portable-Windows-x64`

單檔版：

`Junba-AI-Transcriber-v3.3-Single-EXE-Windows-x64`

## 注意

- 自適應不是保證每一顆 GPU/NPU 永遠不會有驅動錯誤，而是盡量在同一工作內選擇、降級並記住這台電腦較穩定/較快的路徑。
- 舊款 Intel Iris Xe / UHD 不再預設使用大型 OpenVINO Whisper 模型；先求穩定完成，再依實測效能調整。
- 原始音檔如果在 OneDrive 且只有雲端圖示，請先在檔案總管選「永遠保留在此裝置」。
