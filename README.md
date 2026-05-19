# AI-Powered-Odoo-Product-Catalog-Agent-n8n-Workflow-
An advanced, intelligent product ingestion pipeline built in n8n that parses catalogs (Excel/PDF) and creates records in Odoo ERP with built-in **strict deduplication guardrails** to prevent duplicate products.
## 🎯 Problem Solved
Manually migrating or expanding a product catalog in Odoo ERP from vendor sheets or PDFs is a massive bottleneck. Native extraction tools often fail at parsing dense, multi-column tables in PDFs, leading to manual data clean-up and high risks of **duplicate product creation**.

This workflow introduces a **Smart Hybrid Processing Engine**:
1. **Excel Files:** Parsed natively and instantly using fast, lightweight data manipulation nodes.
2. **Complex PDFs:** Routed directly to an LLM via API to handle dense multi-row tables, text layouts, and contextual product fields seamlessly.
3. **De-duplication System:** Before any item hits Odoo, it undergoes strict checking against existing Internal References (`default_code`) or Barcodes (`barcode`) to update or skip, ensuring **zero data duplication**.

---

## ⚡ Workflow Architecture
[ Catalog Received (Telegram/API) ]
↓
Is it Excel or PDF?
├── Excel → Native Parsing (Fast & Free)
└── PDF   → LLM Vision/Context Parsing (Handles complex tables)
↓
[ Strict Deduplication Engine ] → Check Odoo default_code / barcode
↓
├── Exists     → Skip Creation / Log as Skipped
└── New Item   → Create Product Record in Odoo ERP
↓
[ Output Summary Compiled & Returned via Excel ]


---

## 🔧 Tech Stack
| Tool | Role |
| :--- | :--- |
| **n8n** | Pipeline orchestration, data routing, and looping |
| **OpenAI / Anthropic API** | High-fidelity extraction of complex tables inside PDF catalogs |
| **Odoo ERP API** | Targeted system of record (`product.template` / `product.product`) |
| **Telegram Bot API** | Chat interface for users to drop catalogs and receive status spreadsheets |

---

## 📋 Key Features & Logic
* **🚫 Zero Duplication Guardrails:** Features validation switches that query Odoo for an existing SKU before executing a create command.
* **📊 Output Summary Report:** Generates a real-time tracking Excel sheet summarizing which products were **Created** and which were **Skipped (Duplicate)**, then pings it back to the operator.
* **🏗️ Robust Error Handling:** Captures broken formatting and falls back gracefully to prevent entire batch execution crashes.

---

## 🚀 Setup Instructions

### Prerequisites
* n8n instance (Self-hosted or Cloud).
* Odoo ERP Instance with access to the Catalog module (`product.template`).
* OpenAI API Key (or alternative LLM integration for PDF extraction).
* Telegram Bot Token (if keeping the chat-based interface active).

### Steps to Deploy
1.  **Import:** Download and import `Odoo_Product_Catalog_Agent.json` into your n8n workspace.
2.  **Configure Ingestion:** Set up your trigger (e.g., update the Telegram node with your Bot Token or switch it to a Webhook/File Upload trigger).
3.  **LLM Mapping:** Open the LLM node dedicated to PDF extraction and link your API credentials.
4.  **Connect Odoo:** Bind your Odoo credentials and ensure the script targets your standard product search fields correctly.
5.  **Activate:** Turn the workflow on to automate ingestion instantly.

---

## 📁 Repository Structure
```bash
odoo-product-catalog-agent/
├── Odoo_Product_Catalog_Agent.json  # The clean n8n catalog automation file (Import this)
├── pipeline-architecture.png         # Visual screenshot of the hybrid pipeline canvas
└── README.md                         # Project documentation
👤 Author
Asem Ahmed — AI Automation Engineer
