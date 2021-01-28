# 1、创建项目
mkdir projectName

# 2、初始化项目
npm init

# 3、初始化git
git init

# 4、全局安装
npm i eslint prettier typescript -g

# 5、初始化tsconfig.json
tsc init

# 6、安装vscode的eslint&prettier插件

# 7、配置eslint
eslint --init
module.exports = {
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint"
    ],
    "rules": {
    }
};

# 8、安装相关依赖
# typescript相关
npm i typescript -D
# eslint相关【使用eslint --init可选择自动安装】
# @typescript-eslint/eslint-plugin：为TypeScript代码库提供lint规则
# @typescript-eslint/parse：ESLint解析器，它利用TypeScript ESTree允许ESLint整理TypeScript源代码
npm i eslint @typescript-eslint/eslint-plugin @typescript-eslint/parse -D
# prettier相关
# eslint-config-prettie：关闭eslint所有不必要的规则或可能与Prettier冲突的规则。
# eslint-plugin-prettier：prettier规则作为格式化规则
npm i prettier eslint-config-prettier eslint-plugin-prettier -D

# 9、编辑prettier配置文件
touch .prettierrc.js
module.exports = {
  printWidth: 120 /** 超过120自动换行 */,
  tabWidth: 2 /** tab缩进的空格数 */,
  useTabs: false /** 使用tab，而不是空隔 */,
  semi: true /** 每一句结尾添加； */,
  singleQuote: true /** 使用 '' */,
  trailingComma: 'all' /** 不能有尾随逗号 */,
  bracketSpacing: true /**对象添加空格 */,
  jsxBracketSameLine: true /** > 单独在最后一行 */,
  arrowParens: 'always' /**尖头函数，一个参数也要用括号包裹 */,
  insertPragma: false /**插入一个特殊的@format标记 */,
}
  
# 10、eslint结合prettier
# 编辑eslint配置文件
{
		...
    "extends": [
        ...
        "plugin:prettier/recommended"
    ],
    ...
}
  
# 11、配置vscode文件，文件保存自动格式化代码
mkdir .vscode # 记住一定要到根目录不然vscode读取不到，此处翻过🚗
touch settings.json 
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
  "source.fixAll.tslint": true,
  "source.fixAll.eslint": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "git.ignoreLimitWarning": true
}
  
 # 12、代码提交自动检测修复
 # husky：git钩子https://www.npmjs.com/package/husky
 # lint-staged：在提交代码之前运行时，可以确保没有错误进入存储库并强制执行代码样式。
 npm i husky lint-staged -D
 # 配置package.json
 {
 	...,
 	"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "src/*.{ts,tsx}": [
      "eslint --fix --max-warnings 0"
    ],
    "src/*.{js,jsx}": [
      "eslint --fix --max-warnings 0"
    ]
  }
 }
 
 # 13、想验证提交信息是否符合规则
  npm install  @commitlint/config-conventional @commitlint/cli -D 
  touch .commitlintrc.js
  module.exports={extends:['@commitlint/config-conventional'],rules:{}};
  
  # 配置package.json
  "commit-msg": "commitlint -e $GIT_PARAMS"