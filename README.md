[n8n ผู้ช่วยวิจัยอัตโนมัติ (1).json](https://github.com/user-attachments/files/22707433/n8n.1.json)
{
  "name": "My workflow",
  "nodes": [
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "triggerAtHour": 5
            }
          ]
        }
      },
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.2,
      "position": [
        48,
        192
      ],
      "id": "0c4d8b1e-0deb-44cc-ad4d-84b49f4826d9",
      "name": "Schedule Trigger",
      "disabled": true
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=หัวข้อ: {{ $json.message.text }}\n1. \"ช่วยค้นหาบทความวิจัยจาก arXiv ในหัวข้อ 'การบริหารความเสี่ยงทางการเงิน' ขอ 3 บทความล่าสุด พร้อมชื่อเรื่อง ผู้แต่ง ลิงก์ และวันที่ส่ง\"\n\n2. \"ช่วยค้นหาบทความวิจัยจาก arXiv ในหัวข้อ 'ฟิสิกส์' ขอ 3 บทความล่าสุด พร้อมชื่อเรื่อง ผู้แต่ง ลิงก์ และวันที่ส่ง\"\n\n3.\"ช่วยค้นหาบทความวิจัยจาก arXiv ในหัวข้อ 'สถิติ' ขอ 3 บทความล่าสุด พร้อมชื่อเรื่อง ผู้แต่ง ลิงก์ และวันที่ส่ง\"\n\n\nหากไม่มีข้อมูลตรงกับหัวข้อ ให้ตอบว่า \"ไม่พบข้อมูลที่เกี่ยวข้องโดยตรง\"\n\n",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 2.2,
      "position": [
        384,
        0
      ],
      "id": "234f125e-38fb-4d4e-a3fa-071eb5a0db62",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.2,
      "position": [
        16,
        0
      ],
      "id": "f40983ca-8c16-4c00-a923-82ee1568aa21",
      "name": "Telegram Trigger",
      "webhookId": "dc0dc315-7fc8-458f-90d5-338b19f5a8e7",
      "credentials": {
        "telegramApi": {
          "id": "j69cLWQTM5ymUNnf",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.output }}",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        768,
        0
      ],
      "id": "3c319a8a-6a30-43ee-80ed-8ec13747fc17",
      "name": "Send a text message",
      "webhookId": "b44a799d-a4c1-4d6c-bc6e-ea7db76fd1f9",
      "credentials": {
        "telegramApi": {
          "id": "j69cLWQTM5ymUNnf",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        288,
        208
      ],
      "id": "9d204ca5-a458-4d3a-b172-f79254da6719",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "kWyV6B5UolDX8JNK",
          "name": "Google Gemini(PaLM) Api account 2"
        }
      }
    },
    {
      "parameters": {
        "url": "http://export.arxiv.org/api/query",
        "sendQuery": true,
        "queryParameters": {
          "parameters": [
            {
              "name": "search_query",
              "value": "all:AI"
            },
            {
              "name": "start",
              "value": "0"
            },
            {
              "name": "max_results",
              "value": "2"
            },
            {
              "name": "sortBy",
              "value": "submittedDate"
            },
            {
              "name": "sortOrder",
              "value": "descending"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        192,
        0
      ],
      "id": "2b924dfe-90cb-4e70-8c57-0c9f85769ba4",
      "name": "HTTP Request1"
    }
  ],
  "pinData": {},
  "connections": {
    "Schedule Trigger": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "HTTP Request1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Send a text message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request1": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "2ec32f69-a181-466c-b222-acc757badf56",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "9e6d9202c9294cd941c9307009c428b1c08e5e330d6f5265b13eb4c15834070e"
  },
  "id": "cna8SApNJOaTdz1A",
  "tags": []
}
