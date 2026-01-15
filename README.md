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

### 2. pfSense 與 Wazuh 整合實作

本部分展示如何將 pfSense (BSD-based Firewall) 產生的系統與防火牆日誌，透過網路層級的對接，即時推送至 Wazuh Manager 進行監控與分析。

#### 核心實作紀錄

* **跨網段對接**：克服虛擬環境與實體網路 IP 衝突（192.168.1.1），重新配置 pfSense LAN (192.168.56.100) 與 WAN 介面，確保與 Wazuh Manager (192.168.18.133) 之連通性。

* **Wazuh 接收器配置**：手動修改 Wazuh Manager 容器內之 ossec.conf，新增 UDP 514 (Syslog) 監聽服務，並開啟 <logall>yes</logall> 存檔開關以捕捉所有原始數據。

* **數據即時監控**：部署 tcpdump 診斷工具，確認 pfSense 產生的 filterlog 與 nginx 登入事件已成功傳入 Wazuh。

#### 整合步驟

1.**pfSense 端**：於 Status > System Logs > Settings 開啟 Remote Logging，格式設定為 RFC 5424，並精確指向 Wazuh Manager 提供之 514 埠位。

2.**Wazuh 端**：配置 ossec.conf 以接收 UDP 514 埠的 Syslog 流量，並重新啟動 Manager 服務。

3.**數據驗證**：透過 tail -f /var/ossec/logs/archives/archives.log 觀察即時噴發的日誌流，確保防火牆阻擋 (Drop) 與系統異動事件已被 Wazuh 完整擷取。

**Wazuh Manager 配置**：為了接收來自 pfSense 的日誌，我們在 /var/ossec/etc/ossec.conf 中加入了 Syslog 遠端接收設定。詳情請參閱 ossec_manager_syslog.xml。在實作過程中，透過 tcpdump 成功攔截封包，證實數據已跨越虛擬網路邊界。
