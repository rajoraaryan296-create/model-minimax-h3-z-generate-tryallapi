---
layout: default
title: "模型上架：minimax-h3-z，多模态视频生成可走 tryallapi"
description: "2026 年 11 月 23 日 17:01（Asia/Shanghai），第三方接口聚合站 tryallapi（tryallapi.com）在系统公告里写了一条："
---

# 模型上架：minimax-h3-z，多模态视频生成可走 tryallapi

2026 年 11 月 23 日 17:01（Asia/Shanghai），第三方接口聚合站 tryallapi（[tryallapi.com](https://tryallapi.com/)）在系统公告里写了一条：

> 模型上架：minimax-h3-z

![tryallapi 系统公告：模型上架 minimax-h3-z](https://upload.maynor1024.live/file/1795435546655_01-announcement-minimax-h3-z.jpg)

> 图注：2026-11-23 站点系统公告截图。与本文直接相关的是「模型上架：minimax-h3-z」这一条；同屏还有出图计费规则、Grok / Gemini 临时下架、ACE 音乐上新等通知，均与本文无关。

要在自己的项目或脚本里按次调用，可以从这个注册入口进入：[https://tryallapi.com/register?aff=5A6A](https://tryallapi.com/register?aff=5A6A)。

本文按 **2026 年 11 月 23 日晚（Asia/Shanghai）** 对系统公告、公开定价目录和站点文档的复核来写。先把几件事说清楚：

- tryallapi.com 是**第三方 API 聚合站**，不是 MiniMax（海螺 / Hailuo）官方。
- 站点里本来就已经有 `MiniMax-H3`。这次公告里的模型 ID 是 `minimax-h3-z`，**不要把两个名字混用**，也不要按 MiniMax-H3 的分辨率和时长限制直接去调。
- 公开资料里，MiniMax 官方常见的产品名是 MiniMax-H3（视频）、MiniMax-H3-Max、MiniMax-H3-Image（图像）等。本文没有在官方文档里核到一个就叫「H3-Z」的产品名；下面只转述 tryallapi 模型广场对 `minimax-h3-z` 的说明。
- 截至复核时，站点文档《创建视频》里列举的模型仍是 MiniMax-H3 和 MiniMax-H3-Max，**还没有写 `minimax-h3-z` 的字段约束**。接口能否一次调通、分辨率怎么拼写，请以当天控制台和实际返回为准。
- 模型是否在线、分组是否开放、价格都会变。

## 模型广场怎么写

定价目录里，`minimax-h3-z` 的中文描述原文是：

> minimax-h3-z 是一个通用型多模态视频生成模型。它支持文本、图像、视频和音频作为输入，可通过提示词结合首帧、尾帧、参考图、参考视频和参考音频生成视频，输出分辨率覆盖 480p、720p、1080p，时长 2–15 秒，支持 adaptive、21:9、16:9、4:3、1:1、3:4、9:16 等多种画幅。可在画质与生成速度之间灵活取舍，适用于短视频创作、广告素材、图生视频和多素材参考生成等场景。

站内标签是「视频 / 异步」，供应商显示为 Minimax，当天目录里这条模型为可用。目录中该记录的创建时间是 2026-11-18 23:01（Asia/Shanghai），系统公告则是 11 月 23 日 17:01 发出。本文没有实测生成效果。

和站点里已有的 `MiniMax-H3` 对照如下，信息都来自当天定价目录或文档的描述，不是评测结论：

| 项目 | `minimax-h3-z`（站点描述） | `MiniMax-H3`（站点描述） |
| --- | --- | --- |
| 类型 | 多模态视频，异步 | 全模态视频，异步 |
| 分辨率 | 480p、720p、1080p | 768p 和 2K |
| 时长 | 2–15 秒 | 目录写最长 15 秒，文档另写 4–15 秒 |
| 输入 | 文本，以及首帧、尾帧、参考图、参考视频、参考音频 | 文本、图像、视频、音频构成的多模态上下文 |
| 其他说明 | 写了可在画质与生成速度之间取舍 | 写了带原生立体声音频 |
| 当天可用分组 | Self-Deployed-1 | Hailuo-2 |
| 调用端点类型 | hailuo-video | hailuo-video |

不要把 MiniMax-H3 描述里的「原生立体声音频」「2K」自动套到 `minimax-h3-z` 上。

## 怎么调用

定价目录给 `minimax-h3-z` 标注的端点类型是 `hailuo-video`，对应文档里的创建视频接口：

1. 提交：`POST /minimax/v2/video_generation`
2. 查询：`GET /minimax/v2/query/video_generation/{task_id}`（异步任务，先提交再查结果）

文档页面上的示例 Host 仍写着 `https://api.maynorai.asia`，这条备用线路目前已被墙，**不要作为首选**。实际请求请用控制台或文档说明里的可用地址，国内节点可优先考虑 `https://api.tryallapi.com`，主站是 `https://tryallapi.com`。

文档对请求体的通用说明包括：`content` 里必须且只能有 1 条非空 text；可以带首帧 / 尾帧，或参考图、参考视频、参考音频（首尾帧与参考素材不能同时出现）；数量上限为首帧 1、尾帧 1、参考图 9、参考视频 3、参考音频 3；文本最长 7000 字；纯文本生成时 `ratio` 必填，且不能为 adaptive。

但文档里 `model`、`duration`、`resolution` 的取值，目前只写了 MiniMax-H3（768P 或 2K，时长 4–15 秒）和 MiniMax-H3-Max（480P 或 768P，时长 5–15 秒，不支持 2K），**没有列出 `minimax-h3-z`**。调用新模型时，把 `model` 填成 `minimax-h3-z`，分辨率和时长按模型广场的说明选择（广场写的是 480p / 720p / 1080p，2–15 秒）。字段大小写、是否支持 `adaptive` 画幅，请在控制台用小流量先试，不要直接套用 MiniMax-H3 示例里的 `768P` / `2K`。

文档里的结构示例仍是：

```json
{
  "content": [
    {
      "text": "海边日落，镜头缓慢推进",
      "type": "text"
    }
  ],
  "duration": 5,
  "model": "MiniMax-H3",
  "ratio": "16:9",
  "resolution": "768P"
}
```

把其中的 `model` 改成 `minimax-h3-z`，并按广场说明调整 `resolution` 和 `duration`。

## 价格和分组

截至 2026-11-23 晚，公开定价目录里：

- `minimax-h3-z` 的计费类型是按次（目录字段 `quota_type` 为 1），`model_price` 为 0.01。
- 可用分组只有 **Self-Deployed-1**，分组倍率约为 0.66。这是 tryallapi 的分组名，不是 MiniMax 官方的产品线。
- 同目录里 `MiniMax-H3` 也是按次、`model_price` 同为 0.01，但分组是 Hailuo-2（倍率约 1.65）。两条模型不要混用令牌分组。

目录里的基准价乘倍率，不一定等于控制台模型广场里按分辨率、时长或操作显示的单价。不同分辨率、不同时长具体怎么扣，请打开模型广场里 `minimax-h3-z` 的详情核对，本文不另推算「每秒多少钱」「每条视频多少钱」。实际扣费以控制台账单和当时命中的分组为准。

上架不等于每个分组一直可用，也不承诺生成时长、成功率或画质。

## 快速上手

1. 通过 [https://tryallapi.com/register?aff=5A6A](https://tryallapi.com/register?aff=5A6A) 注册并登录控制台。
2. 新建令牌时，确认分组能用到 **Self-Deployed-1**（以当天控制台为准）。
3. 调用时 `model` 填写 `minimax-h3-z`，不要写成 `MiniMax-H3`。
4. 先提交任务，再按返回的 task_id 查询结果。
