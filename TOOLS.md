# TOOLS.md - Kali 渗透测试工具配置

这是你的Kali Linux渗透测试环境配置和工具笔记。

## Kali Linux 环境

### 系统信息
- **OS**: Kali Linux 2024.x
- **内核**: Linux 6.x
- **Shell**: zsh (oh-my-zsh)
- **工作目录**: `/root/.openclaw/workspace-clawedhacker`

### 环境变量
```bash
# 常用工具路径
export NMAP_PRIVILEGED=""
export SQLMAP_PATH="/usr/share/sqlmap"
export METASPLOIT_PATH="/usr/share/metasploit-framework"
export BURP_PATH="/usr/share/burpsuite"

# 工作目录
export PENTEST_WORKDIR="/root/pentest"
export REPORTS_DIR="/root/reports"
export EVIDENCE_DIR="/root/evidence"

# 代理配置（如需要）
export HTTP_PROXY="http://127.0.0.1:8080"
export HTTPS_PROXY="http://127.0.0.1:8080"
```

## 渗透测试工具集

### 侦察与信息收集
```markdown
- **nmap**: 网络扫描和端口发现
  - 常用参数: `-sV -sC -O -p- --min-rate 1000`
  - 输出格式: `-oA <filename>` (所有格式)
  
- **masscan**: 快速端口扫描
  - 常用参数: `-p1-65535 --rate 1000`
  
- **subfinder**: 子域名发现
  - 常用参数: `-d <domain> -all -silent`
  
- **amass**: 攻击面映射
  - 常用参数: `enum -d <domain> -active -brute`
  
- **httpx**: HTTP探测
  - 常用参数: `-title -status-code -tech-detect`
```

### 漏洞扫描
```markdown
- **nikto**: Web服务器扫描
  - 常用参数: `-h <target> -Format xml -output <file>`
  
- **nuclei**: 模板化漏洞扫描
  - 常用参数: `-u <target> -t <template> -severity critical,high`
  
- **zaproxy**: 代理式Web扫描
  - 模式: 被动扫描 + 主动扫描
  
- **sqlmap**: SQL注入检测
  - 常用参数: `-u <url> --batch --level 3 --risk 2`
  - 安全模式: `--test-filter` (避免破坏性操作)
  
- **xsstrike**: XSS检测
  - 常用参数: `-u <url> --crawl`
```

### 利用与验证
```markdown
- **metasploit**: 漏洞利用框架
  - 模块搜索: `search <cve>`
  - 安全使用: 仅在授权目标，避免破坏性payload
  
- **burpsuite**: Web代理和测试
  - 配置: 设置安全扫描范围
  - 扩展: Logger++, Autorize等
  
- **custom scripts**: 自定义验证脚本
  - 位置: `/root/scripts/`
  - 原则: 最小化、可复现、低风险
```

### 后渗透测试（仅在授权范围内）
```markdown
- **mimikatz**: Windows凭证提取
  - 使用限制: 仅在授权Windows目标
  
- **bloodhound**: Active Directory分析
  - 使用限制: 仅在授权AD环境
  
- **crackmapexec**: 网络环境评估
  - 常用参数: `-u <user> -p <pass> -M <module>`
  
- **impacket**: 网络协议工具包
  - 常用工具: smbclient, wmiexec, secretsdump
```

## 工作目录结构

```
/root/
├── pentest/                    # 渗透测试工作目录
│   ├── targets/               # 目标分类
│   │   ├── external/
│   │   ├── internal/
│   │   └── web/
│   ├── scans/                 # 扫描结果
│   │   ├── nmap/
│   │   ├── nuclei/
│   │   └── nikto/
│   ├── evidence/              # 证据收集
│   │   ├── screenshots/
│   │   ├── http-traffic/
│   │   └── tool-outputs/
│   └── exploits/              # exploit脚本
│       ├── verified/
│       └── poc/
├── reports/                   # 报告输出
│   ├── daily/
│   ├── weekly/
│   └── final/
└── scripts/                   # 自定义脚本
    ├── reconnaissance/
    ├── vulnerability/
    ├── exploitation/
    └── reporting/
```

## 安全配置

### 工具安全参数
```bash
# sqlmap 安全模式
alias sqlmap-safe='sqlmap --batch --level 3 --risk 2 --test-filter="NOTES"'

# nmap 非侵入式扫描
alias nmap-safe='nmap -sV -sC --script "not intrusive"'

# nuclei 仅关键漏洞
alias nuclei-critical='nuclei -severity critical,high -etags intrusive'
```

### 代理和流量控制
```markdown
- **Burp Suite代理**: 127.0.0.1:8080
- **ZAP代理**: 127.0.0.1:8090
- **流量记录**: 使用tcpdump或Wireshark
- **速率限制**: 避免DDoS风险
```

## 报告模板

### 漏洞报告模板位置
```
/root/.openclaw/workspace-clawedhacker/schemas/
├── vulnerability-report.json
├── executive-summary.md
└── technical-details.md
```

### 证据收集规范
1. **截图**: PNG格式，包含URL和时间戳
2. **HTTP流量**: HAR格式或原始请求/响应
3. **工具输出**: 原始输出 + 分析注释
4. **时间线**: 测试活动的时间顺序记录

## 自动化脚本

### 常用脚本
```bash
# 快速侦察
#!/bin/bash
# quick-recon.sh <domain>
subfinder -d $1 -silent | httpx -title -status-code -tech-detect

# 安全扫描
#!/bin/bash
# safe-scan.sh <target>
nmap -sV -sC --script "not intrusive" $1
nikto -h $1 -Format xml -output nikto_scan.xml

# 报告生成
#!/bin/bash
# generate-report.sh <target> <date>
python3 /root/scripts/reporting/generator.py --target $1 --date $2
```

## 监控与日志

### 测试活动日志
- **位置**: `/var/log/pentest/`
- **格式**: JSONL (每行一个活动记录)
- **内容**: 命令、目标、时间戳、结果摘要

### 性能监控
- **资源使用**: 监控CPU、内存、网络
- **工具状态**: 记录工具运行状态和错误
- **时间跟踪**: 记录各阶段耗时

---

_保持工具更新，记录配置变化，遵循安全最佳实践。_
