# 前端规范

## 一、全局安装依赖

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
  "src/**.{js,vue}": [
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
"changelog": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0"
}
```

> -s –same-file 输出到指定文件 CHANGELOG.md -r 0 –release-count tag生成数量，0为重新生成整个变更日志，包含所有tag

## 综合使用

```bash
npm i
git add --all
npm run commit
```

## 自动生成版本号

```bash
npm run release #-- --release-as 1.0.0
```

## eslint代码检查

```bash
npm run eslint
```

## 自动changelog文件

```bash
npm run changelog
```
