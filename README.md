# 前端代码规范之路

## 摘要

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
    "commit": "npx git-cz",
    "release": "standard-version",
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -s -r 0",
    "eslint": "eslint --fix --format codeframe 'src/**/**.{js,vue}'"
  }
}
```

## 一、git commit替代方案(2选1)

### 1、全局

> Commitizen是一个撰写合格 Commit message 的工具。

```bash
npm install -g commitizen
```

项目下执行

```bash
commitizen init cz-conventional-changelog --save --save-exact
```

以后，凡是用到git commit命令，一律改为使用git cz 或者 给package.json添加脚本scripts

```json
{
  "commit": "git-cz"
}
```

### 2、项目中安装依赖

```bash
npm i cz-conventional-changelog -D
```

package.json中添加config字段，配置如下：

```json
{
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  }
}
```

给package.json添加脚本scripts

```json
{
  "commit": "npx git-cz"
}
```

## 二、配置

### 1、validate-commit-msg

安装

```bash
npm i validate-commit-msg ghooks -D
```

> validate-commit-msg 用于检查 Node 项目的 Commit message 是否符合格式

使用`ghooks`，把`validate-commit-msg`加为commit-msg时运行

```json
{
  "config": {
    "ghooks": {
      "commit-msg": "validate-commit-msg"
    },
    "validate-commit-msg": {
      "types": [ "feat", "fix", "docs", "style", "refactor", "perf", "test", "build", "ci", "chore", "revert" ],
      "scope": {
        "required": false,
        "allowed": [ "*" ],
        "validate": false,
        "multiple": false
      },
      "warnOnFail": false,
      "maxSubjectLength": 100,
      "subjectPattern": ".+",
      "subjectPatternErrorMsg": "subject does not match subject pattern!",
      "helpMessage": "",
      "autoFix": true
    }
}
```

### 2、lint-staged

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

### 3、新增标签tag

自动生成新的tag，并产生changlog

```bash
npm i standard-version -D
```

```json
{
  "release": "standard-version",
}
```

可以由”standard-version”自动加1生成tag,而且会更改package.json中的version字段。 也可以自己指定生成对应的tag

```bash
npm run release -- --release-as 1.0.0
```

### 4、生成changelog

当然也可以不加tag，自己生成changelog。给package.json添加脚本scripts

```bash
npm install conventional-changelog-cli -D
```

```json
{
"changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -s -r 0"
}
```

> -s –same-file 输出到指定文件 CHANGELOG.md -r 0 –release-count tag生成数量，0为重新生成整个变更日志，包含所有tag

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

### eslint代码检查

```bash
npm run eslint
```

### 自动changelog文件

```bash
npm run changelog
```
