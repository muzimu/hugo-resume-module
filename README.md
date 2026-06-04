# Hugo Resume Module

一个 **data 驱动**、可叠加在现有 Hugo 主题（如 Blowfish）之上的简历模块。基于 [DevResume](http://themes.3rdwavemedia.com/)（Bootstrap 4）移植。

- ✅ **开封即用**：作为 Hugo Module 引入，无需改动现有主题
- ✅ **数据驱动**：所有简历内容写在 `data/resume/<lang>/*.yaml`，模板与数据分离
- ✅ **多语言**：按语言目录隔离（如 `zh-cn/`、`en/`）
- ✅ **参数隔离**：站点级配置收敛在 `params.resume.*` 命名空间，不污染宿主参数

## 快速开始

### 1. 引入 Module

在你的 Hugo 项目根目录启用 Hugo Modules（若尚未启用）：

```bash
hugo mod init github.com/<你的用户名>/<你的站点>
```

在 `config/_default/module.toml`（或 `hugo.toml` 的 `[module]` 段）添加：

```toml
[[imports]]
  path = "github.com/muzimu/hugo-resume-module"
```

拉取依赖：

```bash
hugo mod get github.com/muzimu/hugo-resume-module
hugo mod tidy
```

> 本地开发可在 `go.mod` 用 `replace github.com/muzimu/hugo-resume-module => ./modules/resume` 指向本地目录。

### 2. 创建简历页面

```bash
mkdir -p content/resume
```

`content/resume/index.md`（默认语言）：

```markdown
---
title: 我的简历
type: resume
---
```

多语言再加 `content/resume/index.en.md`（英文）等。

### 3. 填写简历数据

在 `data/resume/<lang>/` 下创建 YAML 文件（`<lang>` 对应你的 `Site.Language.Lang`，如 `zh-cn`、`en`）。
最简只需 `profile.yaml`：

```yaml
# data/resume/zh-cn/profile.yaml
enable: true
name: 张三
tagline: 全栈工程师
avatar: avatar.png
```

完整数据文件清单见下方「数据文件」。可直接复制本模块 `exampleSite/data/resume/` 的示例作为起点。

### 4. （可选）配置站点级参数

在 `params.toml` 添加 `[params.resume]`（全部可选，有默认值）：

```toml
[params.resume]
  title = "张三的简历"          # 页面 <title>，缺省回退站点 Title
  description = "全栈工程师简历"  # meta description
  author = "张三"               # meta author
  primaryColor = "#54B689"      # 主题主色
  textPrimaryColor = "#292929"  # 主文字色
```

### 5. 运行

```bash
hugo server
# 访问 /resume/
```

## 数据文件

`data/resume/<lang>/` 下每个文件对应简历的一个区块，均有 `enable` 开关：

| 文件 | 区块 | 关键字段 |
|------|------|----------|
| `profile.yaml` | 头部姓名/职位/头像 | `name` `tagline` `avatar` |
| `contact.yaml` | 联系方式 | `location` `list[].{icon,url,text}` |
| `summary.yaml` | 个人简介 | `text` |
| `experience.yaml` | 工作经历 | `list[].{title,company,dates,details,items}` |
| `projects.yaml` | 项目经历 | `list[].{title,meta,tagline}` |
| `education.yaml` | 教育背景 | `list[].{degree,university,dates,reward}` |
| `awards.yaml` | 荣誉奖项 | `list[].{name,body,date}` |
| `skills.yaml` | 技能 | `list[].{title,items[].details}` |
| `languages.yaml` | 语言 | `list[].{name,level}` |
| `interests.yaml` | 兴趣 | `list[].name` |
| `information.yaml` | 其他信息 | `list[].{title,details,items}` |
| `social.yaml` | 社交链接（页脚） | `list[].{icon,url,title}` |

图标使用 [Font Awesome 6](https://fontawesome.com/icons)，例如 `fab fa-github`、`fab fa-bilibili`、`fas fa-envelope-square`。

## 翻译

模块自带 `i18n/zh-cn.yaml`、`i18n/en.yaml`（区块标题翻译）。在宿主 `i18n/<lang>.yaml` 中定义同名键即可覆盖。

## 许可

MIT。原始模板 DevResume 由 Xiaoying Riley (3rd Wave Media) 以 CC BY 3.0 发布。
