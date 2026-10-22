# 斗地主 · DOUZIZHU

> 与两家 AI 叫地主、抢地主；点击选牌，支持单张/对子/三带/顺子/炸弹等牌型。

用 [Zan](https://github.com/joyspinch/zan-lang) 语言写的独立游戏项目（仓库根自带 git 仓库）。完整设计与实现说明见 [`PROJECT.md`](PROJECT.md)。

## 玩法

- 与两家 AI 叫地主、抢地主
- 点击选牌，支持单张/对子/三带/顺子/炸弹等牌型
- 出完手牌即获胜，动画发牌与结算

## 构建

需要 [zan-lang](https://github.com/joyspinch/zan-lang) 仓库（`zanc` 编译器、公共包与 stdlib 都在里面）：

```powershell
git clone https://github.com/joyspinch/zan-lang.git
$env:ZAN_HOME = 'D:/path/to/zan-lang'   # 指到刚克隆的 zan-lang 仓库根

pwsh -File client/tools/build.ps1
```

产物：`build/ddz/Ddz.exe`（运行目录要带上它旁边的 `assets/` 与 `skins/`）。

## 运行

```powershell
cd build/ddz
.\Ddz.exe
```

## 测试

```powershell
pwsh -File client/tools/test.ps1
```

## 目录结构

```
ddz/
client/
  src/      # 游戏源码（.zan 源码 + HTML 界面设计稿）
  assets/   # 位图 / 音频素材
  tests/    # 玩法断言用例
  tools/    # 构建与测试脚本
PROJECT.md
```

## 素材与授权

- **大背景图已改为本地程序化生成**：`client/assets/menu_bg.png`、`menu_bg2.png`、`table_bg.png`、`table_bg2.png` 由仓内 `client/tools/make_bgs.py` 生成（纯 numpy + PIL，无外部素材来源），重新生成命令 `python client/tools/make_bgs.py`。
- **卡面 / 牌背 / 点数 / 头像 / 大小王 / 图标 / 标题由图像模型生成**：这批素材源自 zan-lang 早期斗地主模板，由图像模型（gpt-image-2）产出，生成管线为 zan-lang 的 `scripts/gen_assets.ps1` 与 `scripts/gen_game_art.ps1`（统一风格串见 `assets/game/style.md`，卡牌品类限定词 `ornate card illustration, single centered icon, no text`），生成后做绿幕抠图与裁边缩放。逐项来源可追溯：
  - 第一代（`card_face`、`card_back`、`pip_*`、`avatar_left/right/me`、`joker_big/small`、`icon`、`title`、原 `menu_bg`/`table_bg`）—— [zan-lang `7d5efdf9`](https://github.com/joyspinch/zan-lang/commit/7d5efdf91636c7a0852204ee36a5848e7ff0f865)（提交说明原文："ddz: new game with gpt-image art"）。
  - 第二代（`card_face2`、`card_back2`、`joker2`、`avatar_me2/l2/r2`、`panel2`、`title2`、原 `menu_bg2`/`table_bg2`）—— [zan-lang `eb230fdc`](https://github.com/joyspinch/zan-lang/commit/eb230fdc704fdd8ba94bd2b4cb48eb85fa80f568)。
- 仓库不含任何第三方版权素材；全部位图均为本项目自产（图像模型生成或本地程序化生成）。

## 许可

本仓库采用 [MIT 许可](LICENSE)。依赖的 zan-lang 仓库采用 ZCSL（Zan Community Source License 1.0）。

## 更多文档

- [`PROJECT.md`](PROJECT.md) —— 完整设计与实现说明
- [`client/README.md`](client/README.md) —— 客户端侧说明
