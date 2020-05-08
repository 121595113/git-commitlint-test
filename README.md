## 配置

### 全局安装

```bash
npm install -g commitizen
```

### 项目下执行

```bash
commitizen init cz-conventional-changelog --save --save-exact
```

## 使用

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
