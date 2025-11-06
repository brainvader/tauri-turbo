tauri-turbo

主にTauri 2.0デスクトップアプリケーションのために構築された実験的なTurborepoです。共有ワークスペースは、将来のReact Nativeモバイルクライアントと軽量なReact + Viteウェブプレビューをサポートし、UIとドメインロジックがプラットフォーム間で整合性を保つようにします。

このリポジトリは、デザイン・トークン、ビジネス・ルール、開発者ツールにおけるシングル・ソース戦略を模索しつつ、デスクトップ・ビルドを最優先の成果物として維持します。

Repository layout

Path Role Status
apps/web ブラウザでデスクトップのUXフローをプレビューするためのReact + Viteシェル Active
apps/docs Viteベースのスタックに移行予定のドキュメント・プレイグラウンド Under migration
packages/ui クロスプラットフォームのReactコンポーネントライブラリ（デスクトップ、ウェブ、モバイルのアダプターを含む） In progress
packages/eslint-config マルチプラットフォーム・モノレポに合わせた集中型Linterルール Active
packages/typescript-config 一貫したコンパイラ・フラグを強制するための共有TSコンフィグ Active
(planned) apps/desktop 共有パッケージを利用するTauri 2.0シェル (Rust + WebView UI) Not yet scaffolded
(planned) apps/mobile 共有ロジックとUIトークンを活用するReact Nativeアプリ Not yet scaffolded

Prerequisites

    Node.js 18+ と npm 10+（将来的な.nvmrcに準拠）

    cargoを含むRustツールチェーンと、OS固有のTauri 2の前提条件

        Windowsの場合: Visual Studio Build Tools (Desktopワークロード)、WebView2ランタイム、および x86_64-pc-windows-msvc ターゲット

    (Future) React Nativeツールチェーン: モバイルワークスペースが追加された際には、Xcode、Android Studio、Expo CLIが必要

Getting started

Bash

npm install

すべてのワークスペース・スクリプトはTurboを通じて調整されます：

    npm run dev – すべてのアプリの開発サーバーを実行（<workspace>で絞り込むには turbo run dev --filter=<workspace> を使用）

    npm run build – モノレポ全体で本番ビルドを作成

    npm run lint – 共有設定を使用してLinterを実行

    npm run check-types – すべてのパッケージ/アプリの型チェックを実行

    npm run format – Prettierを使用して.ts、.tsx、.mdファイルをフォーマット

Working on web-facing surfaces

Bash

# React + Vite ウェブシェル

turbo run dev --filter=web

# ドキュメント・プレイグラウンド (VitePress/MDXへ移行中)

turbo run dev --filter=docs

Tauri 2 desktop workflow (planned)

デスクトップ・シェルはapps/desktopに配置され、Tauri経由で公開されるRustコマンドとともにpackages/uiを利用します。

    npm install – Rustビルドの前にバインディングが生成されることを保証

    turbo run dev --filter=desktop – デスクトップ開発ウィンドウを起動

    turbo run build --filter=desktop – 配布可能なバイナリを生成

    Tauri 2は、ReactフロントエンドをセキュアなWebViewにバンドルし、Rustコマンドがネイティブ統合を処理します。

React Native workflow (planned)

モバイル・ワークスペースは、共有されたTypeScriptモデルとUIトークンを再利用します。

    ネイティブ設定が変更されたときには npx expo prebuild を実行

    turbo run dev --filter=mobile でExpo開発サーバーを起動

    turbo run build --filter=mobile でリリース・ビルドを作成（EASまたはネイティブ・ツールチェーンを使用）

    Expoは、ビジネスロジックの重複なしに、React Nativeサーフェスを共有TypeScriptレイヤーと整合させます。

Shared code strategy

    UI kit (packages/ui): @repo/ui/desktop、@repo/ui/web、@repo/ui/mobile のようなアダプターを持つ、プラットフォームに依存しないコンポーネント。

    Configs: 集中管理されたESLintとTypeScriptの設定が、Rust、Vite、React Nativeのターゲット全体で一貫したツール設定を強制します。

    Turbo tasks: ビルド、Linter、テストのパイプラインを調整し、プラットフォーム間のキャッシュとインクリメンタル・リビルドを可能にします。

Roadmap

    Tauri 2テンプレートでapps/desktopをスキャフォールドし、Turboタスクを接続します。

    Viteシェルと将来のデスクトップ・ビルドがプリミティブを共有できるようにpackages/uiを強化します。

    Expo Routerを介してapps/mobileを導入し、データ・アクセスとモデルを共有します。

    Turboリモート・キャッシングを使用して、クロスプラットフォーム・リリース・パイプライン（GitHub Actions）を自動化します。

    RustコマンドとJSサーフェスの間のセキュリティおよびネイティブ機能の境界を文書化します。

進捗状況はGitHub Issuesで追跡されています（スキャフォールディングが整い次第、プロジェクト・ボードが作成されます）。

Contributing

このリポジトリは現在、実験的なものです。エリアをプレフィックスとするフィーチャー・ブランチを作成してください（例: desktop/feature-name、mobile/prototype-map）。プルリクエストには以下を含める必要があります：

    npm run lint と npm run check-types に合格すること

    新しいTurboパイプラインまたはネイティブ依存関係をこのREADMEに文書化すること

    プラットフォーム固有のテスト・メモを含めること

Resources

    Tauri 2 ドキュメント

    React Native

    Turborepo ドキュメント

    Expo

    One repo, shared primitives, native everywhere.
