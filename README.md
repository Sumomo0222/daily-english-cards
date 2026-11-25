# 每日英语单词卡 · Daily Words

适配手机的英语单词学习页：每天 5 个精选单词（生活 + 职场，四六级 ~ 雅思 6-7 难度），左右滑动卡片 + 英美发音 + 艾宾浩斯间隔复习。

**线上地址**：https://sumomo0222.github.io/daily-english-cards/

## 文件结构

```
index.html        页面骨架
css/style.css     两套配色主题（纸间 paper / 夜航 night）
js/words.js       词库：BANK_META.start + WORD_BANK.days["YYYY-MM-DD"] = 5 词
js/pool.js        候选词池 WORD_POOL（定时任务取词用）
js/app.js         滑动、解锁、复习调度、发音、进度存储
```

## 核心规则

- **解锁**：日期批次 `YYYY-MM-DD` 在北京时间 08:27 对用户可见（`js/app.js` 中 `batchAvailable`，首日直接可用）。页面本身无需后端。
- **滑动**：左滑 = 已记住（间隔 1/2/4/7/15/30 天递增，六轮后归入已掌握）；右滑 = 明天再练（明天重现，等级回退一级）。
- **队列**：到期复习词优先，其后是「最早一个未学且已解锁」批次的新词，一轮 5 词起。
- **进度**：localStorage（键 `dwc_v1`），仅保存在本机浏览器；支持在「使用说明」里导出/导入备份。

## 例句来源说明

为保证出处真实可靠，**所有例句均为自编例句**并逐条标注「自编例句」，不引用未经核实的他人原文。

## 每日更新（定时任务）

由本机 ZCode 定时自动化任务执行（每天本地时间 08:27，即北京时间 08:27，本机时区为 Asia/Shanghai）：

1. 计算今天的北京日期，检查 `js/words.js` 是否已有当日批次；
2. 若缺失（含补齐最多 3 天的缺口），从 `js/pool.js` 依序取 5 个未用过的词，编写完整词条后追加到 `days`；
3. `git commit + push`，GitHub Pages 自动重新发布；
4. `curl` 验证线上 `words.js` 已包含当日日期。

**运行条件**：任务触发时这台 Mac 需开机且 ZCode 在运行、网络可用、GitHub（gh）登录有效。错过的时点不会自动补触发；但下一次成功运行会补齐缺失日期的批次。词池（约 130 词）耗尽后任务会报告并暂停新增。

## 手动部署

```bash
git add -A && git commit -m "update words" && git push
```

首次部署（已完成）：

```bash
gh repo create daily-english-cards --public --source=. --push
gh api -X POST repos/Sumomo0222/daily-english-cards/pages -f "source[branch]=main" -f "source[path]=/"
```
