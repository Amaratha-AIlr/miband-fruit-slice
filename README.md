# 水果切乐 · Fruit Slice for Mi Band

> 小米手环 11（Vela 快应用）上的切水果小游戏。手指划过就切开，果汁飞溅、连击加分、越玩越快。

![version](https://img.shields.io/badge/version-1.8.0-brightgreen)
![size](https://img.shields.io/badge/rpk-122.7%20KB-blue)
![platform](https://img.shields.io/badge/Xiaomi%20Vela-QuickApp-orange)

**下载**：[米坛社区 · 水果切乐](https://www.bandbbs.cn/resources/8684/)

---

## 玩法

1. 打开后点「开始」
2. **手指划过水果就切开**（直接点一下也可以）
3. 漏掉 1 个水果掉 1 颗心，掉光 3 颗结束
4. 最高分保存在本地

## 内容

- **10 种水果**，按大小给分：西瓜 / 菠萝 3 分；苹果 / 橙子 / 桃子 2 分；葡萄 / 柠檬 / 香蕉 / 草莓 / 猕猴桃 1 分
- **果汁飞溅**：每种水果都有独立配色，切西瓜是红的、切柠檬是黄的
- **震动反馈**：切开、开始、结束都有震动
- **连击奖励**：连续切到第 3、6、9 个额外 +2 分
- **难度递增**：分数越高水果出得越密（1.1 秒/个 → 0.66 秒/个）
- **新纪录提示**：破纪录时结束界面会显示

## 技术要点

没有 Canvas，所有水果都是真实 DOM，用 `left` / `top` 逐帧更新（30fps）。

| 项目 | 取值 | 为什么 |
|---|---|---|
| 同屏水果 | 3 个 | 实测 4 个约 44fps、8 个约 35fps、12 个约 18fps |
| 同屏飞溅 | 3 个（各存活 210ms） | 控制元素总量 |
| 果汁飞溅 | **预渲染图片**，一次切只加 1 个元素 | 视觉上是几十滴果汁，成本只算 1 个元素 |
| 位置驱动 | `left` / `top` 内联绑定 | `transform` 内联会导致整页黑屏（见 docs） |

> 开发中踩到的坑都记在 <https://github.com/Amaratha-AIlr/miband-vela-notes>（整理中）。

## 编译

需要 [Xiaomi AIoT-IDE](https://iot.mi.com/vela/quickapp/zh/tools/aiot-ide.html) 或 `aiot-toolkit`。

```bash
npm install
npx aiot build --complete-feature      # 开发包
npx aiot release --complete-feature    # 发布包（需先准备签名）
```

**签名**（release 必需）：把 `private.pem` 和 `certificate.pem` 放进 `sign/` 目录。
`sign/` 已被 `.gitignore` 排除 —— **私钥不要提交**。

## 安装到手环

1. 小米运动健康 → 我的 → 关于 → 连点橙色 logo 进开发者模式，拿 AuthKey
2. 手环切到「设置 → 我的设备 → 连接新手机」
3. 用**表盘自定义工具**或 **AstroBox** 侧载 rpk
4. ⚠️ AuthKey 是设备钥匙，不要公开

## 已知限制

- **炸弹功能已移除**：切到炸弹会让 Vela 运行时原生崩溃（连调试环境都卡死），代码和素材保留但概率归零
- **音效**：手环没有扬声器，做不了
- 只在小米手环 11 上测试过

## 素材与许可

- 水果图片：**Apple Emoji**（经 npm 包 `emoji-datasource` 获取），个人非商业用途
- 代码：MIT，见 [LICENSE](./LICENSE)

## 支持

小米手环 11（全金属版 / NFC / 陶瓷 / 标准版）。其他支持快应用的手环理论上可装，未实测。
