# 使用 Angular D3.js 和 d3-org-chart 创建组织结构图

## 1. 背景

本人最近在工作中接到一个业务需求：为一些彼此有关联的配置数据创建可视化图表。这些配置数据本身分为不同的类型，不同类型的配置数据之间存在层级关系。用户主要的需求有：

- 选中某个配置数据项时，高亮显示与其相关联的配置数据项。
- 某一类型的配置数据项在渲染的时候包含**子配置项**的链接。点击该链接的时候，加载子配置数据项。

实现上述的用户需求一般要用到前端可视化图表库，比较流行的开源图表库有 [Apache ECharts](https://echarts.apache.org/zh/index.html)，[D3.js](https://d3js.org/) 和 [Chart.js](https://www.chartjs.org/) 等。一开始我觉得这个需求应该很好实现，但是看了相关文档之后发现上述图表库并没有提供开箱即用的层级图。ECharts 的[树图](https://echarts.apache.org/examples/zh/index.html#chart-type-tree)虽然能展示层级关系，但在样式上和后续交互上不太容易做定制化，最终没有采用这个方案。

经过调研，在 GitHub 上比较受欢迎的组织结构图图表库 [OrgChart](https://github.com/dabeng/OrgChart) 和 [org-chart](https://github.com/bumbeishvili/org-chart) 中选择了 `org-chart`。主要是考虑到了后续实现的需要：org-chart 基于 D3.js 支持定制化高亮显示相关节点，同时样式也更美观。

## 2. 项目搭建

演示项目基于 `Angular 17` 搭建，并使用了 `Bootstrap 5` 的样式。

- 安装 `Angular CLI`

```sh
npm install -g @angular/cli

ng version # Angular CLI: 17.1.1
```

- 创建项目

```sh
ng new ng-d3-org-chart --style=scss --ssr=false
```

- 添加 Bootstrap 和 Font Awesome

```sh
npm install bootstrap @fortawesome/fontawesome-free
```

更新 `angular.json` 中的 styles 和 scripts 配置

```json
{
  "architect": {
    "build": {
      "options": {
        "styles": ["node_modules/bootstrap/scss/bootstrap.scss", "node_modules/@fortawesome/fontawesome-free/css/all.min.css", "src/styles.scss"],
        "scripts": ["node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"]
      }
    }
  }
}
```

- 添加 ESLint 和 Prettier

```sh
ng add @angular-eslint/schematics
npm install -D prettier
```

- 添加 Husky 和 lint-staged

```sh
npx mrm@2 lint-staged
```

同时在 `package.json` 中添加相应配置项

```json
{
  "lint-staged": {
    "**/*.{js,jsx,ts,tsx,html}": "eslint --fix",
    "**/*.{css,scss,json,md}": "prettier --write"
  }
}
```

- 添加 d3 和 d3-org-chart 依赖包

```sh
npm i d3 d3-org-chart
npm i -D @types/d3 @types/d3-org-chart
```
