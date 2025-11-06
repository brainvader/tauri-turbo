# tauri-turbo

Tauri 2.0デスクトップアプリケーション向けに構築された、実験的なTurborepoです。共有ワークスペースにより、将来のReact Nativeモバイルクライアントと軽量なReact + Viteウェブプレビューをサポートし、UIとドメインロジックがプラットフォーム間で一貫性を保つことを目指します。

このリポジトリは、デザイン・トークン、ビジネス・ルール、開発者ツールにおけるシングルソース戦略を模索しつつ、デスクトップビルドを最優先の成果物として維持します。

## リポジトリのレイアウト

| Path                         | Role                                                                     | Status                |
| :--------------------------- | :----------------------------------------------------------------------- | :-------------------- |
| `apps/web`                   | ブラウザでデスクトップのUXフローをプレビューするためのReact + Viteシェル | ✅ Active             |
| `apps/docs`                  | Viteベースのスタックに移行予定のドキュメント・プレイグラウンド           | 🏗️ Under migration    |
| `packages/ui`                | クロスプラットフォームのReactコンポーネントライブラリ                    | 🚧 In progress        |
| `packages/eslint-config`     | マルチプラットフォーム・モノレポに合わせた集中型Linterルール             | ✅ Active             |
| `packages/typescript-config` | 一貫したコンパイラ・フラグを強制するための共有TSコンフィグ               | ✅ Active             |
| `(planned) apps/desktop`     | 共有パッケージを利用するTauri 2.0シェル (Rust + WebView UI)              | 📝 Not yet scaffolded |
| `(planned) apps/mobile`      | 共有ロジックとUIトークンを活用するReact Nativeアプリ                     | 📝 Not yet scaffolded |

## 前提条件

- **Node.js**: `18+` と **npm**: `10+` （将来的な`.nvmrc`に準拠）
- **Rustツールチェーン**: `cargo`と、OS固有のTauri 2の前提条件
  - **Windows**: Visual Studio Build Tools (Desktopワークロード)、WebView2ランタイム、および `x86_64-pc-windows-msvc` ターゲット
- **(Future) React Nativeツールチェーン**: モバイルワークスペースが追加された際には、Xcode、Android Studio、Expo CLIが必要

## はじめに

```bash
npm install
```

すべてのワークスペース・スクリプトはTurboを通じて調整されます：

- `npm run dev`: すべてのアプリの開発サーバーを実行します。
  - _(特定のワークスペースで実行するには `turbo run dev --filter=<workspace>` を使用)_
- `npm run build`: モノレポ全体で本番ビルドを作成します。
- `npm run lint`: 共有設定を使用してLinterを実行します。
- `npm run check-types`: すべてのパッケージ/アプリの型チェックを実行します。
- `npm run format`: Prettierを使用して `.ts`, `.tsx`, `.md` ファイルをフォーマットします。

## Web向けの作業

### React + Vite ウェブシェル

```bash
turbo run dev --filter=web
```

### ドキュメント・プレイグラウンド (VitePress/MDXへ移行中)

```bash
turbo run dev --filter=docs
```

## Tauri 2 デスクトップワークフロー (計画中)

デスクトップ・シェルは`apps/desktop`に配置され、Tauri経由で公開されるRustコマンドとともに`packages/ui`を利用します。

- `npm install`: Rustビルドの前にバインディングが生成されることを保証します。
- `turbo run dev --filter=desktop`: デスクトップ開発ウィンドウを起動します。
- `turbo run build --filter=desktop`: 配布可能なバイナリを生成します。

Tauri 2は、ReactフロントエンドをセキュアなWebViewにバンドルし、Rustコマンドがネイティブ統合を処理します。

## React Native ワークフロー (計画中)

モバイル・ワークスペースは、共有されたTypeScriptモデルとUIトークンを再利用します。

- `npx expo prebuild`: ネイティブ設定が変更されたときに実行します。
- `turbo run dev --filter=mobile`: Expo開発サーバーを起動します。
- `turbo run build --filter=mobile`: リリース・ビルドを作成します（EASまたはネイティブ・ツールチェーンを使用）。

Expoは、ビジネスロジックの重複なしに、React Nativeサーフェスを共有TypeScriptレイヤーと整合させます。

## 共有コード戦略

- **UI kit (`packages/ui`)**: `@repo/ui/desktop`、`@repo/ui/web`、`@repo/ui/mobile` のようなアダプターを持つ、プラットフォームに依存しないコンポーネント。
- **Configs**: 集中管理されたESLintとTypeScriptの設定が、Rust、Vite、React Nativeのターゲット全体で一貫したツール設定を強制します。
- **Turbo tasks**: ビルド、Linter、テストのパイプラインを調整し、プラットフォーム間のキャッシュとインクリメンタル・リビルドを可能にします。

## ロードマップ

- [ ] `apps/desktop` をTauri 2テンプレートでスキャフォールドし、Turboタスクを接続する。
- [ ] `packages/ui` を強化し、Viteシェルと将来のデスクトップ・ビルドがプリミティブを共有できるようにする。
- [ ] `apps/mobile` をExpo Routerを介して導入し、データ・アクセスとモデルを共有する。
- [ ] Turboリモート・キャッシングを使用して、クロスプラットフォーム・リリース・パイプライン（GitHub Actions）を自動化する。
- [ ] RustコマンドとJSサーフェスの間のセキュリティおよびネイティブ機能の境界を文書化する。

進捗状況はGitHub Issuesで追跡されています（スキャフォールディングが整い次第、プロジェクト・ボードが作成されます）。

## コントリビューション

このリポジトリは現在、実験的なものです。エリアをプレフィックスとするフィーチャー・ブランチを作成してください（例: `desktop/feature-name`、`mobile/prototype-map`）。プルリクエストには以下を含める必要があります：

- `npm run lint` と `npm run check-types` に合格すること。
- 新しいTurboパイプラインまたはネイティブ依存関係をこのREADMEに文書化すること。
- プラットフォーム固有のテスト・メモを含めること。

## 参考資料

- [Tauri 2 ドキュメント](https://beta.tauri.app/start/intro/introduction/)
- [React Native](https://reactnative.dev/)
- [Turborepo ドキュメント](https://turbo.build/repo/docs)
- [Expo](https://expo.dev/)

> One repo, shared primitives, native everywhere.
