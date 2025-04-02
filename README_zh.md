```markdown
# Open Duck 参考动作生成器

> 文档机翻的捏，有做一定修改，有不足的地方麻烦提issue~

Open Duck 项目的参考动作生成器，用于模仿学习，基于 [Placo](https://github.com/Rhoban/placo)。

这些参考动作被用于两项强化学习（RL）工作：

> 译者注：我一般只用第一个哈

- 一项使用 Mujoco Playground [这里](https://github.com/SteveNguyen/openduckminiv2_playground)
- 另一项使用 Isaac Gym [这里](https://github.com/rimim/AWD)。

![banner](https://github.com/user-attachments/assets/f445e373-74fc-413b-aa73-0f17c76b1171)

## 安装

安装 uv

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 使用

### 生成动作

> 译者注：我这里是 open_duck_mini_v2 这个，而且一般用了 --sweep

```bash
uv run scripts/auto_waddle.py (-j?) --duck ["go_bdx", "open_duck_mini", "open_duck_mini_v2"] (--num <> / --sweep) --output_dir <>
```

参数：
- `-j?` 指定并行任务数。如果未指定 `j`，将按顺序运行。如果仅指定 `j` 而不带数字，可能会导致计算机崩溃 :) （使用所有可用核心运行）。例如可以使用 `j4`。
- `--duck` 选择鸭子类型。
- `--sweep` 在 `Open_Duck_reference_motion_generator/open_duck_reference_motion_generator/robots/<duck>/auto_gait.json` 中指定的范围内生成所有组合的动作。
- `--num` 生成 <num> 个随机动作。
- `--output_dir` 输出目录（自解释）。

### 拟合 多项式(polynomials)

这将生成 `polynomial_coefficients.pkl` 文件。

> 译者注：在训练的时候要用到的这个文件。在部署的时候虽然作者引用的这个文件，但是并不需要。

```bash
uv run scripts/fit_poly.py --ref_motion <>
```

绘制图表：

```bash
uv run scripts/plot_poly_fit.py --coefficients polynomial_coefficients.pkl
```

### 回放

```bash
uv run scripts/replay_motion.py -f recordings/<file>.json
```

### Playground

```bash
uv run open_duck_reference_motion_generator/gait_playground.py --duck ["go_bdx", "open_duck_mini", "open_duck_mini_v2"]
```

## 待办事项(TODO)

- 机器人描述应存放在单独的仓库中，并作为子模块导入。这有助于避免重复相同的文件或不同版本的问题。
- 验证是否可以使用这些动作训练策略（在移植过程中可能已损坏某些部分）。
- 修复 gait_playground 中的小错误（刷新、更换机器人等）。
- 更好的可视化？可以直接使用 meshcat 进行动作可视化。
- 编写文档以说明参考动作格式，如果有人希望将一些动作捕捉数据转换为模仿奖励使用。
- 该仓库以鸭子为主题，但它可以是一个基于 Placo 的通用双足机器人动作生成器（稍后会添加 sigmaban）。
  - 子待办事项：解释如何添加新机器人。
```
