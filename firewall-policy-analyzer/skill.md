# firewall-policy-analyzer

Analyze FortiGate firewall policies, resolve address/group objects to actual IPs, and generate security assessment reports.

# Firewall Policy Analyzer

## 📌 描述

解析 FortiGate 防火牆政策清單（JSON 格式），交叉比對 `firewall address` 與 `firewall addrgrp` 設定，將政策中的來源/目的物件解析為實際 IP，並以資安工程師角度逐條評估安全性，最終輸出完整的 Markdown 分析報告。

## 🎯 何時使用

- 📋 收到 FortiGate 防火牆政策匯出檔（JSON），需要製作安全分析報告
- 🔍 需要將 address 物件名稱對應到實際 IP/FQDN
- 🛡️ 需要以資安角度審查每條 policy 的安全性
- 📊 需要產出可供稽核使用的防火牆規則清單

## 📋 前置要求

- 防火牆政策清單 JSON 檔案（含欄位：政策、來源、目的、服務、採取行動、NAT、排程、資安功能設定、流量記錄、流量位元組、介面組合）
- （選用）`config firewall address` 設定文字
- （選用）`config firewall addrgrp` 設定文字

## 🔄 工作流程

### 步驟 1：讀取並解析政策 JSON

讀取政策 JSON 檔案，統計總數、接受/拒絕數量。確認欄位結構是否完整。

### 步驟 2：解析 Firewall Address 設定

從使用者提供的 `config firewall address` 文字中，提取每個 address 物件的 IP 資訊：

| 類型 | 解析方式 | 輸出格式 |
|------|---------|---------|
| subnet | `set subnet IP MASK` | 轉換為 CIDR（如 `192.168.96.0/24`） |
| iprange | `set start-ip` / `set end-ip` | `起始IP-結束IP` |
| fqdn | `set fqdn` | `FQDN:域名` |
| dynamic | `set sub-type ems-tag` | `dynamic:ems-tag` |
| 無定義 | 無 subnet/fqdn/iprange | `未定義IP` |

子網遮罩轉 CIDR 對照：
- `255.255.255.255` → `/32`
- `255.255.255.0` → `/24`
- `255.255.0.0` → `/16`

### 步驟 3：解析 Firewall Address Group 設定

從 `config firewall addrgrp` 中提取 group 成員，並遞迴展開為實際 IP：

```
Group 名稱 → 成員列表 → 逐一查找 address 物件 → 取得 IP
```

### 步驟 4：交叉比對政策與 IP

將每條政策的「來源」和「目的」欄位中的 address 名稱，替換為 `名稱_IP/遮罩` 格式：

- 單一 address：`HTW_TP_IPPBX_192.168.96.239/32`
- Address group：`SSL-VPN Risk IPaddress_(85.209.11.132/32, 185.7.214.72/32, ...)`
- User group：`hakuto_sslvpn_(user group)`
- 找不到定義：`物件名_(未在address/addrgrp清單)`
- all：`all_0.0.0.0/0`

### 步驟 5：拆分介面組合

將「介面組合」欄位（格式：`來源介面,目的介面`）拆分為兩個獨立欄位：

- `lan,vsw.PoE-Switch` → 來源介面：`lan`，目的介面：`vsw.PoE-Switch`
- `隱性的` → 兩欄皆為 `隱性的`

### 步驟 6：資安評估

以資安工程師角度，逐條評估每個政策的安全性。評估維度：

| 檢查項目 | 高風險條件 🔴 | 中風險條件 🟡 | 低風險條件 🟢 |
|---------|-------------|-------------|-------------|
| 來源/目的 | 皆為 all | 部分為 all | 皆有限縮 |
| 服務 | ALL | ALL 但有 UTM | 限定特定服務 |
| 安全檢測 | no-inspection 且有活躍流量 | no-inspection 但零流量 | 啟用 cert-inspection/IPS/AV |
| 流量記錄 | 已停用 | UTM only | 全部 |
| 命名 | 名實不符（如 Deny 但 Accept） | 僅有編號無描述 | 描述性名稱 |
| 流量狀態 | 名稱標示「可刪」且零流量 | 零流量需確認 | 有活躍流量 |

風險等級定義：
- 🔴 高風險：存在明顯安全漏洞，建議立即處理
- 🟡 中風險：有改善空間，建議排程優化
- 🟢 低風險：設定合理，符合安全基準
- ⚪ 資訊：備註或建議清理項目

### 步驟 7：產出報告

生成 Markdown 報告，結構如下：

```markdown
# 防火牆政策清單安全分析

> 來源：`原始檔名`
> 分析日期：YYYY-MM-DD
> 分析角色：資安工程師

## 政策總覽
（統計數據）

## 風險等級說明
（等級表格）

## Firewall Address 對照表
（所有政策中使用到的 address 物件與 IP 對應）

## Firewall Address Group 對照表
（group 成員展開與 IP 對應）

## 政策分析表
| # | 政策名稱 | 來源 | 目的 | 服務 | 動作 | NAT | 排程 | 資安功能 | 流量記錄 | 流量 | 來源介面 | 目的介面 | 資安評估 |

## 綜合安全建議
### 🔴 高優先處理事項
### 🟡 中期改善事項
### 🟢 長期優化建議
```

### 步驟 8：儲存報告

將報告儲存至與原始 JSON 相同的目錄，檔名格式：
`{原始檔名去掉.json}_analysis.md`

## ⚠️ 注意事項

1. **漸進式分析**：若使用者一開始只提供 JSON，先以 address 名稱產出報告。待使用者補充 `firewall address` 和 `addrgrp` 設定後，再更新 IP 對應。
2. **未知物件標記**：找不到定義的物件標記為 `(未在address/addrgrp清單)` 或 `(user group)`，不要猜測 IP。
3. **User Group 識別**：名稱含 `sslvpn`、`ldap`、`user`、`group` 等關鍵字且不在 address/addrgrp 中的，標記為 `(user group)`。
4. **大量 all/all/ALL 規則**：這是 FortiGate 常見的過度開放問題，應在綜合建議中特別強調。
5. **介面組合展開**：同一條 policy 可能因介面組合展開而出現多次，應在評估中標註「同 #N」避免重複說明。
6. **報告語言**：繁體中文。

## 📊 輸出範例

### Address 對照表範例
```
| Address 名稱 | 解析結果 |
|-------------|---------|
| all | all_0.0.0.0/0 |
| HTW_TP_IPPBX | HTW_TP_IPPBX_192.168.96.239/32 |
| SSL-VPN Risk IPaddress | SSL-VPN Risk IPaddress_(85.209.11.132/32, 185.7.214.72/32, ...) |
| hakuto_sslvpn | hakuto_sslvpn_(user group) |
```

### 政策表格範例
```
| 1 | TP LAN to WAN1 (43) | all_0.0.0.0/0 | all_0.0.0.0/0 | ALL | 接受 | NAT | always | cert-inspection2, ... | 全部 | 2.43 TB | vsw.PoE-Switch | virtual-wan-link | 🟡 UTM 完整但全開放... |
```

### 綜合建議範例
```
### 🔴 高優先處理事項
1. **大量 all/all/ALL + no-inspection 規則**：站對站 VPN 流量幾乎全部無安全檢測...
2. **Wi-Fi 到內網無檢測**：Wi-Fi 為不可信任網段，應優先啟用 inspection...
3. **標示「可刪」的規則**：名稱已標示可刪除且零流量，應立即清理...
```
