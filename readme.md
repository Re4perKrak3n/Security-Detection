# Sentinel-Brain: Autonomous Agentic Security Pipeline

**Sentinel-Brain** is a high-performance, local-first security framework that integrates real-time computer vision with multimodal large language models (MLLMs). It transforms traditional motion alerts into an "intelligent" security agent capable of reasoning, threat assessment, and verbal intervention.

---

## 🗝️ Core Concept

Traditional security systems suffer from high false-alarm rates. **Sentinel-Brain** solves this by using a hierarchical execution model to maximize GPU efficiency:

1.  **The Sentinel (YOLO26):** A lightweight, always-on observer that filters for specific entities (Persons) with near-zero latency.
2.  **The Brain (Qwen2.5-VL):** A multimodal specialist that performs deep contextual analysis only when a trigger occurs.
3.  **The Executive:** A dispatch layer that handles Text-to-Speech (TTS) intervention and encrypted mobile notifications.

---

## 🛠️ System Architecture

### Phase 1: Neural Filtering (YOLO26)
Utilizing the **YOLO26-Nano** architecture, the system monitors a 24/7 RTSP or local video stream.
* **Model:** `yolo26n` (End-to-End/NMS-Free).
* **Efficiency:** Optimized for edge deployment, consuming <1GB VRAM.
* **Logic:** When a `person` is detected with high confidence ($> 0.85$), a high-resolution buffer is captured and passed to the secondary stage.

### Phase 2: Multimodal Reasoning (Qwen2.5-VL)
The captured frame is processed by **Qwen2.5-VL-3B** (Int4 Quantized). Unlike standard object detectors, the Brain interprets **intent**:
* **Visual Grounding:** Identifies tools, weapons, or suspicious attire (masks/balaclavas).
* **Contextual Analysis:** Differentiates between a delivery driver and a potential intruder.
* **Structured Output:** Generates a JSON payload containing a `threat_score` and `chain_of_thought`.

### Phase 3: Immediate Intervention (TTS & Notify)
If the `threat_score` exceeds the user-defined threshold:
* **Verbal Deterrent:** A local TTS engine (e.g., Piper or gTTS) generates an audible warning via the system's audio output.
* **Mobile Alert:** The reasoning chain and image are dispatched via Telegram/Matrix.

---

## 📈 Projected Performance
*Benchmarked on RTX 3060 (6GB VRAM) / Ryzen 7 5800H*

| Component | Precision | VRAM | Latency |
| :--- | :--- | :--- | :--- |
| **YOLO26-Nano** | FP16 | ~500 MB | ~2ms |
| **Qwen2.5-VL-3B** | INT4 | ~2.8 GB | ~1.5s |
| **System Baseline** | - | **~3.5 GB** | **Total: <2s** |

---

## 📂 Project Structure
```text
├── sentinel/           # YOLO26 Inference & Stream Handling
├── brain/              # Qwen2.5-VL Prompting & Reasoning Logic
├── executive/          # TTS Modules & Alert Dispatchers
├── config/             # Threat Thresholds & System Rules
└── main.py             # Main Event Loop



<img width="979" height="706" alt="Screenshot 2026-02-03 101242" src="https://github.com/user-attachments/assets/b31f34b2-d7ff-4488-a6c1-d851c9a914ae" />

