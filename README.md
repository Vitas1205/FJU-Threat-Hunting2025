# FJU-Threat-Hunting2025: 期末實作專案

## 🛡️ Wazuh AI Threat Hunting & pfSense Integration Project

### **1. MCP Server for Wazuh - Threat Hunting 展示**
本部分展示如何透過 **Claude Desktop** 整合 **MCP Server**，實現以自然語言進行資安管理。

#### **核心實作紀錄**
* **API 連線驗證**：成功透過 `wazuh-wui` 帳號獲取 Wazuh API Token。
* **Indexer 驗證**：經測試，Indexer (Port 9200) 已可正常回傳 JSON 數據。

#### **功能展示**
* 成功透過對話列出 Agents 列表，確認 **Agent 003 (Windows 11)** 處於 **Active** 狀態。

---

### **2. pfSense 與 Wazuh 整合實作**
展示如何將 pfSense 防火牆日誌接入 Wazuh 進行監控。

#### **整合步驟**
1.  **pfSense 端**：開啟 Remote Logging，將日誌指向 Wazuh Manager (`192.168.18.133`)。
2.  **Wazuh 端**：配置 `ossec.conf` 以接收 UDP 514 埠的 Syslog 流量。
3.  **數據驗證**：透過 Wazuh Dashboard 監控防火牆阻擋 (Drop) 事件。

> **Wazuh Manager 配置**：為了接收來自 pfSense 的日誌，我們在 `/var/ossec/etc/ossec.conf` 中加入了 Syslog 遠端接收設定。詳情請參閱 `ossec_manager_syslog.xml`。
