# ClawedHacker - Kali Linux 渗透测试专家 Agent

![ClawedHacker Avatar](https://img.shields.io/badge/Agent-ClawedHacker-blue?style=for-the-badge&logo=linux&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white)
![Security](https://img.shields.io/badge/Security-Penetration%20Testing-red?style=for-the-badge&logo=shield&logoColor=white)
![OpenClaw](https://img.shields.io/badge/Built%20with-OpenClaw-6e40c9?style=for-the-badge&logo=github&logoColor=white)

**ClawedHacker** 是一个运行在Kali Linux上的专业网络安全渗透测试AI Agent，专为OpenClaw平台设计。它结合了Kali Linux的强大工具集和AI的智能分析能力，提供企业级的安全评估服务。

## 🎯 核心特性

### 🔒 专业渗透测试能力
- **全面工具集成**: 集成nmap、sqlmap、metasploit、burpsuite等Kali工具
- **结构化输出**: 遵循JSON schema的标准化漏洞报告
- **证据驱动**: 每个结论都有可复现的证据链支持
- **风险控制**: 内置安全边界，防止破坏性操作

### 🛠️ 技术栈
- **平台**: OpenClaw + Kali Linux 2024.x
- **工具集**: 完整的Kali渗透测试工具链
- **输出格式**: JSON schema标准化报告
- **集成**: 支持Antigravity Awesome Skills (978+技能)

### 📊 方法论驱动
1. **候选发现** - 主动扫描与指纹识别
2. **证据补全** - 深度分析与证据收集  
3. **exploit/PoC生成** - 攻击向量构建
4. **可复现验证** - 低风险验证测试
5. **修复建议** - 可执行的修复方案

## 📁 项目结构

```
clawedhacker-agent/
├── AGENTS.md                    # 渗透测试工作区配置
├── SOUL.md                      # 核心原则和身份定义
├── IDENTITY.md                  # Agent身份信息
├── TOOLS.md                     # Kali工具配置
├── USER.md                      # 客户信息模板
├── HEARTBEAT.md                 # 监控检查配置
├── MEMORY.md                    # 长期安全知识库
├── config/
│   └── pentest-config.json      # 渗透测试配置
├── pipeline/
│   └── pipeline-config.json     # 多阶段pipeline配置
├── schemas/
│   └── report-schema.json       # 报告JSON schema
├── rules/
│   ├── tool-rules.json          # 工具使用规则
│   └── vulnerability-patterns.json # 漏洞模式规则
├── memory/
│   └── YYYY-MM-DD.md            # 每日工作日志
└── skills/                      # 技能库 (978+技能)
    ├── pentest-commands/
    ├── active-directory-attacks/
    ├── api-security-testing/
    └── ... (965个技能目录)
```

## 🚀 快速开始

### 1. 环境要求
- OpenClaw平台
- Kali Linux环境
- 网络访问权限
- 授权测试目标

### 2. 配置Agent
```bash
# 克隆仓库
git clone https://github.com/your-org/clawedhacker-agent.git

# 配置OpenClaw workspace
cp -r clawedhacker-agent/* ~/.openclaw/workspace-clawedhacker/
```

### 3. 运行渗透测试
```bash
# 启动ClawedHacker agent
openclaw agent start clawdhacker

# 执行安全扫描
curl -X POST http://localhost:8080/scan \
  -H "Content-Type: application/json" \
  -d '{"target": "https://example.com", "scan_type": "full"}'
```

## 🔧 配置详解

### 渗透测试配置 (`config/pentest-config.json`)
```json
{
  "scanning": {
    "aggressiveness": "normal",
    "rate_limit": {"requests_per_minute": 60},
    "tools": {
      "nmap": {"enabled": true, "scripts": ["vuln", "http-enum"]},
      "sqlmap": {"enabled": true, "level": 3, "risk": 2}
    }
  },
  "safety": {
    "target_validation": {"require_authorization": true},
    "scan_limits": {"max_requests_per_target": 1000}
  }
}
```

### Pipeline配置 (`pipeline/pipeline-config.json`)
```json
{
  "stages": {
    "candidate_discovery": {"enabled": true, "parallel": true},
    "evidence_enrichment": {"enabled": true, "depends_on": ["candidate_discovery"]},
    "exploit_generation": {"enabled": true, "mode": "rule_based"},
    "verification": {"enabled": true, "mode": "automated"},
    "remediation": {"enabled": true}
  }
}
```

## 📋 输出示例

### 结构化漏洞报告
```json
{
  "vulnerability_id": "TARGET-20260305-001",
  "severity": "high",
  "category": "sqli",
  "target": "https://target.com/api/users",
  "evidence": [
    {
      "timestamp": "2026-03-05T09:00:00Z",
      "tool": "sqlmap",
      "command": "sqlmap -u 'https://target.com/api/users?id=1' --batch",
      "output": "[INFO] GET parameter 'id' is vulnerable",
      "analysis": "SQL注入漏洞确认"
    }
  ],
  "reproducible_steps": [
    "发送请求: GET /api/users?id=1'",
    "观察SQL错误响应",
    "验证注入点"
  ],
  "root_cause": "未对用户输入进行参数化查询处理",
  "remediation": {
    "immediate": "部署WAF规则",
    "permanent": "使用参数化查询",
    "code_example": "PreparedStatement stmt = conn.prepareStatement(...);"
  }
}
```

## 🛡️ 安全与合规

### 授权要求
- 所有测试必须在授权范围内进行
- 禁止未经授权的系统访问
- 保护客户敏感数据

### 风险控制
- 🚫 禁止破坏性payload（写文件、删库、加密）
- 🚫 禁止横向移动
- 🚫 禁止越权访问敏感数据
- ✅ 仅授权范围内测试
- ✅ 非破坏性验证优先

### 数据保护
- 自动脱敏敏感信息
- 加密存储测试报告
- 定期清理临时数据

## 📈 性能指标

| 指标 | 目标值 | 说明 |
|------|--------|------|
| 扫描覆盖率 | > 90% | 目标攻击面覆盖比例 |
| 误报率 | < 10% | 错误报警比例 |
| 验证成功率 | > 85% | 漏洞验证成功比例 |
| 报告生成时间 | < 5分钟 | 从扫描到报告的时间 |
| 资源使用 | CPU < 80% | 系统资源占用 |

## 🔗 集成能力

### 支持的技能库
- **Antigravity Awesome Skills**: 978+通用技能
- **安全专用技能**: 32个安全测试技能
- **自定义技能**: 支持用户自定义技能扩展

### API接口
```bash
# 启动扫描
POST /api/v1/scan
{
  "target": "string",
  "scan_type": "full|quick|custom",
  "parameters": {}
}

# 获取报告
GET /api/v1/report/{scan_id}

# 状态监控
GET /api/v1/status
```

## 🤝 贡献指南

### 报告问题
1. 在GitHub Issues中创建新issue
2. 提供详细的复现步骤
3. 包含相关日志和配置信息

### 提交代码
1. Fork本仓库
2. 创建功能分支
3. 提交Pull Request
4. 通过所有测试用例

### 开发规范
- 遵循渗透测试安全规范
- 保持代码可读性和可维护性
- 添加必要的测试用例
- 更新相关文档

## 📄 许可证

本项目采用MIT许可证。详见 [LICENSE](LICENSE) 文件。

## 🙏 致谢

- **OpenClaw团队** - 提供强大的AI Agent平台
- **Kali Linux社区** - 优秀的渗透测试工具集
- **Antigravity Awesome Skills** - 丰富的技能库资源
- **所有贡献者** - 感谢你们的支持和贡献

## 📞 联系方式

- **GitHub Issues**: [报告问题](https://github.com/your-org/clawedhacker-agent/issues)
- **文档**: [查看完整文档](https://docs.your-org.com/clawedhacker)
- **社区**: [加入Discord讨论](https://discord.gg/your-community)

---

**ClawedHacker - 专业、严谨、证据驱动的渗透测试专家**

> "安全第一，证据为王。准确发现，严谨验证，实用建议。"

---
*最后更新: 2026-03-05 | 版本: 1.0.0 | 作者: Makari*