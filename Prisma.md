
環境設定
```
クイックスタートの場合
$ curl -L https://pris.ly/quickstart | tar -xz --strip=2 quickstart-master/typescript/starter

依存関係のDL
$ cd starter
$ npm install

一から構築
$ npm init -y
package.jsonの作成

$ npm install prisma typescript ts-node @types/node --save-dev
--save-devオプション
ローカルインストールするためのコマンド
（対になる-gがグローバルインストール）
自動で package.jsonの devDependencies に追記される。
そして dependencies には追記されない。

https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch-typescript-mysql
```

コンソール画面へのクエリと結果表示  
Prismaではscript.tsを操作する
```
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

// A `main` function so that you can use async/await
async function main() {
  const allUsers = await prisma.user.findMany({
    include: { posts: true },
  })
  // use `console.dir` to print nested objects
  console.dir(allUsers, { depth: null })
}

main()
  .catch(e => {
    throw e
  })
  .finally(async () => {
    await prisma.$disconnect()
  })

実行
$ npm run dev

結果
[
  { id: 1, email: 'sarah@prisma.io', name: 'Sarah', posts: [] },
  {
    id: 2,
    email: 'maria@prisma.io',
    name: 'Maria',
    posts: [ [Object] ]
  }
] { depth: null }
```



