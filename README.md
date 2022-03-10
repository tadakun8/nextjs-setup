# Setup Nextjs project

## Nextjs プロジェクトの作成

typescript を指定する

```
❯❯❯ npx create-next-app@latest nextjs-setup --ts
```

## 不要なファイルの削除

## pages/api

ディレクトリごと削除

## pages/\_app.tsx

```_app.tsx.diff
- import '../styles/globals.css'
-
function MyApp({ Component, pageProps }) {
 return <Component {...pageProps} />
}
```

## pages/index.tsx

```import type { NextPage } from 'next'
-import Head from 'next/head'
-import Image from 'next/image'
-import styles from '../styles/Home.module.css'

 const Home: NextPage = () => {
   return (
-    <div className={styles.container}>
-      <Head>
-        <title>Create Next App</title>
-        <meta name="description" content="Generated by create next app" />
-        <link rel="icon" href="/favicon.ico" />
-      </Head>
-
-      <main className={styles.main}>
-        <h1 className={styles.title}>
-          Welcome to <a href="https://nextjs.org">Next.js!</a>
-        </h1>
-
-        <p className={styles.description}>
-          Get started by editing{' '}
-          <code className={styles.code}>pages/index.tsx</code>
-        </p>
-
-        <div className={styles.grid}>
-          <a href="https://nextjs.org/docs" className={styles.card}>
-            <h2>Documentation &rarr;</h2>
-            <p>Find in-depth information about Next.js features and API.</p>
-          </a>
-
-          <a href="https://nextjs.org/learn" className={styles.card}>
-            <h2>Learn &rarr;</h2>
-            <p>Learn about Next.js in an interactive course with quizzes!</p>
-          </a>
-
-          <a
-            href="https://github.com/vercel/next.js/tree/canary/examples"
-            className={styles.card}
-          >
-            <h2>Examples &rarr;</h2>
-            <p>Discover and deploy boilerplate example Next.js projects.</p>
-          </a>
-
-          <a
-            href="https://vercel.com/new?utm_source=create-next-app&utm_medium=default-template&utm_campaign=create-next
-app"
-            className={styles.card}
-          >
-            <h2>Deploy &rarr;</h2>
-            <p>
-              Instantly deploy your Next.js site to a public URL with Vercel.
-            </p>
-          </a>
-        </div>
-      </main>
-
-      <footer className={styles.footer}>
-        <a
-          href="https://vercel.com?utm_source=create-next-app&utm_medium=default-template&utm_campaign=create-next-app"
-          target="_blank"
-          rel="noopener noreferrer"
-        >
-          Powered by{' '}
-          <span className={styles.logo}>
-            <Image src="/vercel.svg" alt="Vercel Logo" width={72} height={16} />
-          </span>
-        </a>
-      </footer>
-    </div>
+    <div>Hello world!</div>
   )
 }
```

### styles ディレクトリのファイル

ディレクトリごと削除

## eslint の導入

- `create-next-app`で既に導入されているため、改めて導入する必要はなし
- .eslintrc.json も自動作成される

```.eslintrc.json
{
  "extends": "next/core-web-vitals"
}
```

### lint コマンドの設定

- scripts に`lint-fix`を追加し、`lint`コマンドで自動で fix 出来ない Warning が出ていればそれも修正できるようにする

```
{
  ...,
  "scripts": {
    ...,
    "lint": "next lint",
    "lint-fix": "next lint --fix",
  },
  ...
}

```

## prettier の導入

```
❯❯❯ npm install -D prettier eslint-config-prettier
```

### eslint との併用設定

`.eslintrc.json`の最後に`prettier`と記載する

```.eslintrc.json
{
  "extends": [..., "prettier"]
}
```

### prettier の設定ファイルの追加

```
❯❯❯ echo "module.exports = {\\n\\tprintWidth: 120\\n}" > .prettierrc.js
```

### package.json に prettier での format コマンドを追加

```pacakge.json
{
  ...,
  "scripts": {
    ...
    "format": "prettier --write './**/*.{js,jsx,ts,tsx,json}'"
  },
  "dependencies": {
    ...
}
```

### vscode でセーブ時に eslint・prettier を走らせる

```
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  // Add those two lines:
  "editor.formatOnSave": true, // Tell VSCode to format files on save
  "editor.defaultFormatter": "esbenp.prettier-vscode" // Tell VSCode to use Prettier as default file formatter
}
```

## Chakra UI の導入

このリポジトリでは CSS フレームワークとして Chakra UI を利用します

```
❯❯❯ npm i @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^6 @chakra-ui/icons
```

### Chakra UI 書いてみる

```pages/_app.tsx
-import type { AppProps } from 'next/app'
+import type { AppProps } from "next/app";
+import { ChakraProvider } from "@chakra-ui/react";

 function MyApp({ Component, pageProps }: AppProps) {
-  return <Component {...pageProps} />
+  return (
+    <ChakraProvider>
+      <Component {...pageProps} />
+    </ChakraProvider>
+  );
 }

-export default MyApp
+export default MyApp;
```

```pages/index.tsx
-import type { NextPage } from 'next'
+import type { NextPage } from "next";
+import { Box, chakra } from "@chakra-ui/react";

 const Home: NextPage = () => {
   return (
-    <div>Hello world!</div>
-  )
-}
+    <Box>
+      <chakra.h1 color="tomato">Hello World</chakra.h1>
+    </Box>
+  );
+};

-export default Home
+export default Home;
```

## src ディレクトリ作成

Nextjs では`pages/`を`src/`に格納しても問題ありません

- https://nextjs.org/docs/advanced-features/src-directory

実装を進めていくと、`components/`や、`constants/`などのディレクトリをを適宜作成していくことになります。これらのディレクトリはそのままだと`pages/`と並列に作られますが、数が多くなってくると、プロジェクトルートにディレクトリが乱立します。乱立しても問題はないのですが、ぐちゃぐちゃしてるのは気分のいいものではないでしょう(少なくとも僕は)

そのモヤモヤをなくすために、`pages/`を含むアプリコードを`src/`配置します。

※ 改めて言いますが、`src`を作成しなくても問題はありません。

```
❯❯❯ mkdir src && mv pages src
```
