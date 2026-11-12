# 作品与笔记

一个可以直接放到 GitHub Pages 的个人主页，外加一个真的在训练的 MLP 交互演示。

主页是 `index.html`，演示在 `mlp/index.html`。两个页面都是纯静态文件，没有构建步骤，也没有外部依赖。

## 文件

```text
.
├── index.html          个人主页
├── mlp/
│   └── index.html      MLP 观测台（可交互）
├── assets/
│   ├── homepage.png    主页首屏截图
│   ├── preview.png     演示大图
│   ├── replay.png      回放截图
│   ├── spiral.png      螺旋数据集截图
│   └── ledger.png      参数账本局部
├── .nojekyll           告诉 GitHub Pages 不要跑 Jekyll
└── README.md
```

## 发布到 GitHub Pages

### 方式一：网页上传

1. 新建仓库。想做个人首页就命名为 `<你的用户名>.github.io`；普通项目仓库也可以。
2. 在仓库首页点 **Add file → Upload files**，把本目录里的 `index.html`、`mlp/`、`assets/`、`.nojekyll` 一起拖进去。
3. 提交后进入 **Settings → Pages**。
4. **Build and deployment** 里选 **Deploy from a branch**，分支选 `main`，目录选 `/ (root)`，保存。
5. 等一分钟左右：
   - 个人站仓库：`https://<你的用户名>.github.io/`
   - 普通仓库：`https://<你的用户名>.github.io/<仓库名>/`

### 方式二：命令行

```bash
git init
git add .
git commit -m "personal site"
git branch -M main
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git push -u origin main
```

推上去之后，同样在 **Settings → Pages** 里选 `main` 和 `/ (root)`。

`.nojekyll` 建议一起传。它不是必需的，但能避免 GitHub 把下划线开头的文件或某些目录当成 Jekyll 站点处理。

## 改成你自己的

打开 `index.html`，在文件底部找到这段：

```js
const PROFILE = {
  name: '胡天宇'
};
```

改掉名字，页头、关于区和页脚会一起更新。再顺手改一下 `<title>`、`<meta name="description">` 和 `og:` 标签。

如果想换作品区的大图，替换 `assets/preview.png` 即可；比例建议保持 16:10。

## MLP 演示里有什么

`mlp/index.html` 是一个 2–H–1 网络：两个输入、H 个隐藏单元、一个 sigmoid 输出。损失是二元交叉熵，更新是带动量的全批量梯度下降，最多 800 轮。

页面上这些数字不是预先写好的：

- 概率场和决策边界由当前参数逐点算出来。
- 单样本的 `z⁽¹⁾`、`a⁽¹⁾`、`δ⁽¹⁾`、`p`、`L` 都来自当前这一轮。
- 参数账本里的权重、梯度、本步位移都来自同一组数组。
- 时间轴里存的是每一轮的参数快照，拖动后整个页面回到那一轮。

可切换月牙、螺旋、四角、圆环，隐藏单元 3–8，tanh / ReLU，学习率、动量和速度。点击样本、参数格子，或拖动时间轴都能互动。

## 说明

- 两个页面都没有外部请求，不需要联网也能打开。
- 参数是随机初始化的。偶尔会卡在局部解，换一组数据或加一个隐藏单元再试。
- 损失曲线画的是每轮更新后的全量损失，不是抽样估计。
- 主页底部只保留了一个名字字段，发布前确认名字、标题和描述一致。
