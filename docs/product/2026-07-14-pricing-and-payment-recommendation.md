# 新手健身产品定价与支付建议

调研日期：2026-07-14

## 决策摘要

- 首发用户：健身新手。
- 经营主体：个人，App Store 卖家将显示经营者的法定姓名。
- 首轮测试价格：`¥18/月`、`¥128/年`，年付相当于约 7.1 个月月费。
- 暂不提供终身买断。AI、认证、支持和存储都会产生持续成本。
- 免费版继续保留基础记录、基础复盘、完整导出和安全提醒。
- Pro 以 90 天/年度长期报告、聚合 AI 深度解读和后续可信恢复为付费价值。
- 在真实支付和权益闭环上线前，不显示购买按钮或手工 Pro 开关。

## 中国区竞品价格

以下数据来自 Apple 中国区 App Store 在 2026-07-14 返回的应用内购买商品。相同产品存在旧价格或多个商品档位时，表格列出当前可见范围，不把旧档位当成唯一现价。

| 产品 | 月付 | 年付 | 终身 |
| --- | ---: | ---: | ---: |
| Hevy Pro | ¥22-27 | ¥173 | ¥498 |
| Strong Pro | ¥27-35 | ¥163-213 | ¥598-698 |
| Fitbod Elite | ¥88-98 | ¥528-698 | 无 |

来源：

- https://apps.apple.com/cn/app/id1458862350
- https://apps.apple.com/cn/app/id464254577
- https://apps.apple.com/cn/app/id1041517543

当前产品的 Pro 范围窄于 Hevy 和 Strong，也还没有成熟原生体验或跨设备同步，因此不应直接跟随成熟竞品定价。`¥18/月、¥128/年` 是用于验证付费意愿的首轮价格，不是已经被市场验证的最优价格。

首批数据达到可判断规模后，再测试 `¥22/月、¥148/年`。至少观察结账完成率、次月续费率、退款率、长期报告使用率和单次 AI 成本，不能只看购买人数。

## Apple 路径是否需要 Visa

没有 Visa 卡不等于不能开始。

Apple Developer Program 允许个人加入，年费为 99 美元或当地可用币种。通过 iPhone 上的 Apple Developer app 加入时，会员是自动续费订阅；应先直接查看该 app 对当前 Apple Account 展示的支付方式。Apple 官方列出的中国大陆 Apple Account 支付方式包括支付宝、Apple Account 余额、抖音支付、多数信用卡和借记卡、微信支付。最终能否用于开发者会员，以购买页实际显示为准。

如果改走网页加入，Apple 只保证可选择购买页展示的方式；若页面要求信用卡，个人必须使用本人信用卡。此时没有可用信用卡就不能硬绕过，应回到 Developer app 或联系 Apple Developer Support。

官方依据：

- https://developer.apple.com/support/enrollment/
- https://support.apple.com/111741

## Apple Pay 与 App Store 收款的区别

Apple Pay 只是用户付款方式，不是经营者的收款账户。

当前产品是 PWA。要在网站直接接 Apple Pay，仍需支付服务商或收单机构、Merchant ID、证书、域名验证和结算账户。仅有 iPhone 或 Apple Developer 会员不能让网站直接收款。Apple 官方也建议通过支付服务商 SDK 接入。

对当前个人经营阶段，更可行的苹果路线是：先完成个人开发者加入，再把产品做成达到审核要求的 iOS 应用并使用 App Store 应用内购买。这个方向仍需要 iOS 工程、StoreKit、服务端交易校验、退款/续费状态处理、账号删除、隐私披露和审核，不是给现有 PWA 加一个按钮。

官方依据：

- https://developer.apple.com/apple-pay/implementation/
- https://developer.apple.com/help/app-store-connect/getting-paid/overview-of-receiving-payments

## 经营者需要准备的资料

1. 本人法定姓名、身份证明、达到所在地区法定年龄的证明信息。
2. 已开启双重认证的 Apple Account，以及 iPhone 上的 Apple Developer app。
3. 开发者年费可用支付方式。优先验证支付宝、微信支付、借记卡或 Apple Account 余额在购买页是否可用。
4. 本人可收款的银行账户及银行信息。Apple 结算必须在 App Store Connect 提供主银行账户。
5. App Store Connect 的 Paid Apps Agreement、税务资料和所在地区可能要求的发票资料。
6. 稳定的支持邮箱、隐私政策、服务条款、取消和退款说明。
7. 可审核的 iOS 应用、应用图标与截图、隐私标签、账号删除流程和服务器交易校验。

Apple 的小型企业计划对符合条件的新开发者或年收益不超过 100 万美元的开发者提供 15% 的付费应用和应用内购买佣金率，但需要单独申请并满足关联开发者账户规则：

- https://developer.apple.com/app-store/small-business-program/

## 推荐执行顺序

1. 先在 iPhone 安装 Apple Developer app，用本人的 Apple Account 发起个人加入，记录实际出现的支付方式；不要先购买来源不明的境外卡。
2. 同时准备本人银行账户、税务资料和长期可用的支持邮箱。
3. 产品侧先完成正式后端、账号删除、支持入口和交易状态机，再开始 iOS 收款实现。
4. 首发只提供月付和年付，使用 `¥18/月、¥128/年` 做小规模验证。
5. 支付成功、失败、取消、到期、退款和恢复购买都必须由服务端验证后更新权益。
