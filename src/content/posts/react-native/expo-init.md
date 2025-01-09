---
title: ReactNative项目初始化
published: 2024-12-23
description: 初始化配置项目
tags: [ReactNative]
category: ReactNative
draft: false
lang: zh_CN
---

# 初始化项目

1.  `pnpm create expo -t` 选根据需要择模板 - 以 default 模板为示例

2.  **配置 prettier 格式化 和 eslint**

    - 安装 prettier `pnpm add -D prettier prettier-plugin-tailwindcss` prettier-plugin-tailwindcss 用于格式化 tailwindcss
    - 项目根目录添加文件`.prettierrc`

      ```bash
      {
        "semi": true,
        "trailingComma": "all",
        "singleQuote": true,
        "printWidth": 80,
        "tabWidth": 2,
        "bracketSpacing": true,
        "arrowParens": "avoid",
        "plugins": ["prettier-plugin-tailwindcss"]
      }
      ```

    - 项目根目录添加文件：`.prettierignore` 用于排除格式化目录

      ```bash
      node_modules/
      ios/
      android/
      assets/
      .expo/
      ```

    - 配置 eslint : 运行 `pnpm create @eslint/config@latest` 根据引导完成配置, 然后修改 `eslint.config.mjs` 添加部分规则

      ```js
      // eslint.config.mjs
      import globals from 'globals';
      import pluginJs from '@eslint/js';
      import tseslint from 'typescript-eslint';
      import pluginReact from 'eslint-plugin-react';

      /** @type {import('eslint').Linter.Config[]} */
      export default [
        { languageOptions: { globals: globals.browser } },
        pluginJs.configs.recommended,
        ...tseslint.configs.recommended,
        pluginReact.configs.flat.recommended,
        {
          files: ['src/**/*.{js,mjs,cjs,ts,jsx,tsx}'],
          ignores: ['**/*.config.js'],
          rules: {
            'import/order': 'off',
            'react/react-in-jsx-scope': 'off',
            'react/display-name': 'off',
            '@typescript-eslint/no-explicit-any': 'off',
            '@typescript-eslint/no-unused-vars': 'off',
          },
        },
      ];
      ```

3.  **配置 [NativeWind](https://www.nativewind.dev/) (tailwindcss)**

    - 运行 `npx expo install nativewind tailwindcss react-native-reanimated react-native-safe-area-context`
    - 运行 `npx tailwindcss init` 创建 `tailwind.config.js` 文件

      ```js
      /** @type {import('tailwindcss').Config} */
      module.exports = {
        // NOTE: Update this to include the paths to all of your component files.
        content: ['./app/**/*.{js,jsx,ts,tsx}', './src/**/*.{js,jsx,ts,tsx}'],
        presets: [require('nativewind/preset')],
        theme: {
          extend: {},
        },
        plugins: [],
      };
      ```

    - 根目录创建 `global.css` 文件, 注入 tailwindcss 代码

      ```css
      @tailwind base;
      @tailwind components;
      @tailwind utilities;
      ```

    - 修改 `babel.config.js` 添加预设 运行: `npx expo customize babel.config.js`

      ```js
      module.exports = function (api) {
        api.cache(true);
        return {
          presets: [
            ['babel-preset-expo', { jsxImportSource: 'nativewind' }],
            'nativewind/babel',
          ],
        };
      };
      ```

    - 修改 `metro.config.js` 添加预设 运行: `npx expo customize metro.config.js`

      ```js
      const { getDefaultConfig } = require('expo/metro-config');
      const { withNativeWind } = require('nativewind/metro');

      const config = getDefaultConfig(__dirname);

      module.exports = withNativeWind(config, { input: './global.css' });
      ```

    - 在 `app/_layout.js` 中导入 `global.css`
