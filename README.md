# 羽山数据企业数据查询 MCP Server

羽山数据（上海羽山数据服务有限公司）提供的托管式 MCP Server，面向 AI 智能体与企业应用，提供中国企业的公开数据查询能力。

- **服务端点**：`https://mcp.usendata.com/mcp`
- **传输方式**：Streamable HTTP（MCP 协议版本 `2025-06-18`）
- **鉴权方式**：Bearer Token（`Authorization: Bearer <YOUR_TOKEN>`）
- **能力性质**：全部为只读查询，无写入、无删除操作
- **服务官网**：https://mcp.usendata.com/
- **接入指南**：https://mcp.usendata.com/guide

---

## 快速接入

### 1. 获取 Token

访问控制台 https://mcp.usendata.com/admin 注册并创建 Token（`ysk_` 开头）。
Token 明文**仅显示一次**，请当场妥善保存；如遗失，可停用旧 Token 后重新生成。

### 2. 客户端配置

```json
{
  "mcpServers": {
    "yushan": {
      "type": "streamableHttp",
      "url": "https://mcp.usendata.com/mcp",
      "headers": {
        "Accept": "application/json, text/event-stream",
        "Authorization": "Bearer <YOUR_TOKEN>"
      },
      "timeout": 30000
    }
  }
}
```

> 请将 `<YOUR_TOKEN>` 替换为你在控制台创建的 Token。**不要将真实 Token 提交到任何代码仓库。**

---

## 工具清单（15 个，均为只读）

| 工具 | 说明 |
|---|---|
| `fuzzy_query_company` | 企业名称关键词模糊查询，返回候选企业及工商全称 |
| `query_company_basic_info` | 工商基本信息：统一社会信用代码、法定代表人、注册资本、经营范围、注册地址 |
| `query_company_overseas_investments` | 股东构成与对外投资（投资金额、持股比例、股东类型） |
| `query_company_court_litigation_related_info` | 法院涉诉信息：案件、当事人身份、判决结果 |
| `query_company_abnormal_business_operation` | 经营异常名录记录 |
| `query_company_serious_violation_blacklist` | 严重违法失信名单记录 |
| `query_company_change_records` | 工商变更历史记录 |
| `query_company_trademark_list` | 商标列表（商标名称、所属企业、状态） |
| `query_company_patent_list` | 专利列表 |
| `query_company_software_copyright_info` | 软件著作权登记信息 |
| `verify_company_name_and_president` | 法人姓名与企业名称二要素核验（一致/不一致） |
| `verify_company_four_elements` | 企业四要素核验：法人姓名、身份证号、统一社会信用代码、企业全称，返回各项匹配标志；可选填写被查询人手机号，由系统发送授权短链完成本人授权 |
| `query_company_credit_info` | 企业年度信用与财务指标：资产总额、负债总额、利润总额、净利润、营业总收入、主营业务收入、纳税总额、所有者权益、参考人数 |
| `query_company_global_info` | 境外 / 离岸主体登记信息 |
| `parse_address` | 地址结构化解析：省、市、区、街道、门牌、邮编 |

## 使用建议

1. 先调用 `fuzzy_query_company` 拿到企业**工商全称**，再用全称调用其余工具——多数工具要求精确全称。
2. 用户指代不明确时（如“腾讯”），应先让用户确认具体主体，避免查错公司。
3. 单次查询可能产生费用，请按业务需要组合调用，避免无意义重复查询。

## 示例提示词

1. 帮我找一下名字里带“腾讯”的公司，并给出工商全称。
2. 查询深圳市腾讯计算机系统有限公司的工商基本信息和股东构成。
3. 看看这家公司是否被列入经营异常名录或严重违法失信名单。
4. 查询这家公司 2022 到 2024 年的资产总额、负债和净利润。
5. 把地址“上海市浦东新区张江高科技园区博云路2号301室”解析成省市区、街道和门牌。

## 数据与合规

- 主体查询对象为中国境内注册企业的**公开工商、司法及信用公示信息**。
- `verify_company_four_elements` 涉及法定代表人身份证号核验，需经**被查询人本人授权**（通过短信授权短链完成），请确保已取得合法授权后再调用。
- 服务不涉及数据写入或修改，全部为只读查询。
- 请在各数据源的授权范围内使用查询结果，并遵守《个人信息保护法》《数据安全法》等相关法律法规。

## 支持

- 官网：https://www.yushanshuju.com/
- 数据服务咨询：400-110-8298 · bd@yushanshuju.com
