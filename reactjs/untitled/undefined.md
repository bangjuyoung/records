# 기본 설정

### nextjs typescript 설치&#x20;

React framework&#x20;

```bash
yarn create next-app --typescript
```

### material-ui 설치

React UI 라이브러

```
yarn add @mui/material @emotion/react @emotion/styled
```

#### server rendering with nextjs&#x20;

* [https://github.com/mui-org/material-ui/tree/HEAD/examples/nextjs-with-typescript](https://github.com/mui-org/material-ui/tree/HEAD/examples/nextjs-with-typescript)

### ESLint + Prettier 설치

[https://paulintrognon.fr/blog/typescript-prettier-eslint-next-js](https://paulintrognon.fr/blog/typescript-prettier-eslint-next-js)

#### ESLint

javascript 문법 검사

```
yarn add --dev @typescript-eslint/eslint-plugin

// .eslintrc.json
{
  "plugins": ["@typescript-eslint"],
  "extends": [
    "next/core-web-vitals",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    // I suggest you add those two rules:
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "error"
  }
}
```

#### Prettier

코딩 스타일 지정

```
yarn add --dev prettier eslint-config-prettier

// .prettierrc.json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "tabWidth": 2,
  "useTabs": false
}

// .eslintrc.json
{
  // ...
  "extends": [
    "next/core-web-vitals",
    "plugin:@typescript-eslint/recommended",
    "prettier" // Add "prettier" last. This will turn off eslint rules conflicting with prettier. This is not what will format our code.
  ],
  // ...
}
```

### Husky + Lint-staged 설치

커밋 시  prettier 와 eslint 를 먼저 실행하여 에러시 커밋되지 않는다.

[https://paulintrognon.fr/blog/typescript-prettier-eslint-next-js](https://paulintrognon.fr/blog/typescript-prettier-eslint-next-js)&#x20;

#### lint-staged

커밋 시 에러, 린팅, 코드 포맷을 체크한다.

```
yarn add --dev husky

// husky 사용
yarn husky install

// git hook 추가
yarn husky add .husky/pre-commit "yarn tsc --noEmit && yarn eslint --fix . && yarn prettier --write ."

```

#### Lint staged

필요할 때 린트를 실행할 수 있게 설정한다.

```
yarn add --dev lint-staged

// 설정
// lint-staged.config.js 생성
module.exports = {
  // Type check TypeScript files
  '**/*.(ts|tsx)': () => 'yarn tsc --noEmit',

  // Lint then format TypeScript and JavaScript files
  '**/*.(ts|tsx|js)': (filenames) => [
    `yarn eslint --fix ${filenames.join(' ')}`,
    `yarn prettier --write ${filenames.join(' ')}`,
  ],

  // Format MarkDown and JSON
  '**/*.(md|json)': (filenames) =>
    `yarn prettier --write ${filenames.join(' ')}`,
}

```

#### pre-commit hook 설정

커밋 시 prettier + eslint 를 먼저 실행한다.

```
// .husky/pre-commit

#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

yarn lint-staged # Replace the last line with "yarn lint-staged"

```
