# ChatRAG

## Prouduct Definition
結合AMB82及LLMRAG所建構的問答系統，以提供高效且準確地聊天和問答服務。 應具備以下功能:

1. 語音交互: 能夠透過語音與使用者進行交互，讓使用者能夠自然地與ChatGPT進行對話。
2. 連接性: 具有Wi-Fi或藍牙等連接功能，以便與互聯網或其他設備進行連接，從而擴展其功能或者進行遠程更新和管理。
3. 智能對話能力: 具有ChatGPT的核心功能，能夠理解和回應使用者的提問、請求、指令等，並提供相應的回答或解決方案。

---
## Development Board Introduciton
### AMB82-MINI overview
<p><img width="50%" height="50%" src="https://www.amebaiot.com/wp-content/uploads/2023/03/amb82_mini.png"></p>

[RTL8735B](https://www.amebaiot.com/en/amebapro2/): 32-bit Arm v8M, up to 500MHz, 768KB ROM, 512KB RAM, 16MB Flash (MCM embedded DDR2/DDR3L up to 128MB) 802.11 a/b/g/n WiFi 2.4GHz/5GHz, BLE 5.1, NN Engine 0.4 TOPS, Crypto Engine, Audo Codec, …

* [Ameba Arduino](https://www.amebaiot.com/en/ameba-arduino-summary/)

* [Amebapro2 AMB82-mini Arduino Example Guides](https://www.amebaiot.com/en/amebapro2-amb82-mini-arduino-peripherals-examples)

* [Amebapro2 AMB82-mini Arduino getting started](https://www.amebaiot.com/en/amebapro2-amb82-mini-arduino-getting-started/)
---
## System Diagram
<p><img width="50%" height="50%" src="https://github.com/rkuo2000/portable-ChatGPT/blob/main/assets/Portable-ChatGPT_System_Diagram.png?raw=true"></p>

---
## Implementation
### Server:
model_name = "meta-llama/Meta-Llama-3-8B-Instruct"<br>
**Code:**[AmebaPro2_Whisper_LlamaIndex_RAG_server.py](https://github.com/xnwei/portable-ChatGPT/blob/main/AmebaPro2_Whisper_LlamaIndex_RAG_server.py)<br>
```
    # print(decoded_data)
    #Save the decoded data to an MP4 file
    with open("output.mp4", "wb") as f:
        f.write(decoded_data)
  
    # Whisper transcribe
    result = ASR.transcribe("output.mp4",fp16=False)
    header1 = "ASR: "
    result1 = result["text"]        
    print(header1+result1)
    
    # Query-Engine
    prompt = result["text"]
    response = query_engine.query(prompt)
    header2 = "LLaVA: "
    result2 = response.response
    print(header2+result2)
    return Response(header1+result1+"\n"+header2+result2)
```

### Client
**Code:**[RecordMP4_HTTP_Post_Audio_TFTLCD.ino](https://github.com/xnwei/portable-ChatGPT/blob/main/RecordMP4_HTTP_Post_Audio_TFTLCD.ino)<br>
```
#define TFT_RESET 5 #define TFT_DC 4 #define TFT_CS SPI_SS
AmebaILI9341 tft = AmebaILI9341(TFT_CS, TFT_DC, TFT_RESET); #define TFT_RESET 5
#define TFT_DC 4
#define TFT_CS SPI_SS
AmebaILI9341 tft = AmebaILI9341(TFT_CS, TFT_DC, TFT_RESET);
#define ILI9341_SPI_FREQUENCY 20000000
#define FILENAME "TestRecordingAudioOnly.mp4"
char ssid[] = "Hsun"; // your network SSID (Home WiFi or Smartphone Hotspot)
char pass[] = "48918925"; // your network password
int status = WL_IDLE_STATUS;
char server[] = "172.20.10.9"; // the server IP running HTTP server on PC
#define PORT 5000
```
---
## Demo Video
https://youtube.com/shorts/CBXGBWKEMnU?feature=share

---
