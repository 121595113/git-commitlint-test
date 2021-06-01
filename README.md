# 前端代码规范之路

## 摘要

### Commit Message

项目的 Git Commit Message 参考了 [AngularJS 规范](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.google.com%2Fdocument%2Fd%2F1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y)

```js
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

- `type` 类型，修改的类型级别:
  - feat: 新功能
  - fix: 修补 Bug
  - docs: 文档
  - style: 格式变更，不影响代码的运行
  - refactor: 重构（既不是新增功能，也不是修改 bug 的代码变- 动）
  - perf: 重构（性能优化）
  - test: 增加测试
  - chore: 构建过程或辅助工具的变动
  - up: 不属于上述分类时，可使用此类别
  - merge: 用于 merge branch 时，需要手写 commit message 的情况
  - revert: 用于撤销之前的 commit
- `scope` 修改范围，主要是这次修改涉及到的部分，最好简单的概括
- `subject` 修改的副标题，主要是具体修改的加点
- `body` 修改的主体标注
- `footer` 放置不兼容变更和Issue关闭的信息

### npm scripts

```json
{
  "scripts": {
    "commit": "commit",
    "release": "standard-version --skip.commit --skip.tag --skip.changelog",
    "changelog": "npm run release && conventional-changelog -p angular -i CHANGELOG.md -w -s -r 1",
    "eslint": "eslint --fix --format codeframe 'src/**/**.{js,vue}'"
  }
}
```

## 一、commit message 校验

### 1、项目下安装commitlint依赖

```bash
npm install --save-dev @commitlint/{cli,config-conventional}@9.1.1
```

package.json添加`commitlint`filed,配置如下:

```json
"commitlint": {
  "extends": [
    "@commitlint/config-conventional"
  ]
}
```

### 2、用git commit-msg hook对提交信息校验

安装

```bash
npm i ghooks -D
```

使用`ghooks`，把`commitlint`加为commit-msg时运行

```json
{
  "config": {
    "ghooks": {
      "commit-msg": "commitlint -e"
    }
  }
}
```

## 二、其它配置

### 1、lint-staged对提交代码eslint代码格式规范化校验

安装

```bash
npm i lint-staged -D
```

lint-staged可以对git暂存中的代码进行格式校验，package.json添加配置如下：

```json
"lint-staged": {
  "src/**/**.{js,vue}": [
    "eslint --fix",
    "git add"
  ]
}
```

ghook中添加

```json
{
  "pre-commit": "lint-staged"
}
```

### 2、新增标签tag

自动生成新的tag，并产生changlog

```bash
npm i standard-version -D
```

```json
{
  "release": "standard-version --skip.commit --skip.tag --skip.changelog",
}
```

可以由”standard-version”自动加1生成tag,而且会更改package.json中的version字段。 也可以自己指定生成对应的tag

```bash
npm run release -- --release-as 1.0.0
```

### 3、生成changelog

当然也可以不加tag，自己生成changelog。给package.json添加脚本scripts

```bash
npm install conventional-changelog-cli -D
```

```json
{
"changelog": "npm run release && conventional-changelog -p angular -i CHANGELOG.md -w -s -r 1"
}
```

> -s –same-file 输出到指定文件 CHANGELOG.md -r 1 –release-count tag生成数量，0为重新生成整个变更日志，包含所有tag

### 4、`git commit`替代方案(可选)

```bash
npm install --save-dev @commitlint/prompt-cli
```

以后，凡是用到git commit命令，一律改为使用npx commit 或者 给package.json添加脚本scripts

```json
{
  "commit": "commit"
}
```

## 三、综合使用

```bash
npm i
git add --all
npm run commit
```

### 自动生成版本号

```bash
npm run release #-- --release-as 1.0.0
```

- `-r` Specify the release type manually 自定义版本
- `-p` This will tag your version as: 1.0.1-0 小版本
- `-p alpha` This will tag the version as: 1.0.1-alpha.0
- `-m` [DEPRECATED] Commit message, replaces %s with new version.\nThis option will be removed in the next major version, please use --releaseCommitMessageFormat.
- `-n` Bypass pre-commit or commit-msg git hooks during the commit phase
- `-t` Set a custom prefix for the git tag to be created

### eslint代码检查

```bash
npm run eslint
```

### 自动changelog文件

```bash
npm run changelog
```
