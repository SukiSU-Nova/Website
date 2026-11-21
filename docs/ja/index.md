---
layout: page
title: "SukiSU Nova"
pageClass: landing-page
sidebar: false
outline: false
aside: false
---

<script setup lang="ts">
const hero = {
  badge: '次世代の Android Root ソリューション',
  badgeHref: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU/releases',
  title: 'SukiSU Nova',
  subtitle: 'Android 向けのカーネルレベル Root 権限管理 — セキュリティと柔軟性を両立。',
  logo: '/logo.svg',
  primary: { label: 'はじめる', href: '/ja/guide/installation' },
  secondary: { label: 'リポジトリを見る', href: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU' },
  chips: ['LKM インストール', 'Zygisk 内蔵', 'テーマストア', 'Miuix / M3E', 'GKI & 非 GKI カーネル対応', 'KPM カーネルモジュール', 'オープンソース']
}

const overview = {
  title: 'ローダブルカーネルモジュール (LKM) としてインストール',
  columns: [
    {
      heading: 'インストール方法',
      rows: [
        { name: 'LKM', note: 'マネージャーがビルド済みモジュールで boot イメージにパッチを適用', label: '推奨', level: 'full' },
        { name: 'GKI カーネル', note: 'ビルド済みの AnyKernel3 カーネルをフラッシュ', label: 'ビルド済み', level: 'basic' },
        { name: '組み込み統合', note: '独自のカーネルソースに直接統合', label: '上級者向け', level: 'manual' }
      ]
    },
    {
      heading: 'ビルド済み LKM イメージ',
      rows: [
        { name: 'カーネル 5.10', note: 'android12 · android13', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'カーネル 5.15', note: 'android13 · android14', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'カーネル 6.1', note: 'android14', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'カーネル 6.6', note: 'android15', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'カーネル 6.12', note: 'android16', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'カーネル 6.18', note: 'android17', label: 'aarch64 · x86_64', level: 'full' }
      ]
    }
  ]
}

const features = {
  title: 'SukiSU Nova を選ぶ理由',
  description: 'Root、隠蔽、テーマ、モジュール体験をひとつのマネージャーに統合',
  items: [
    {
      title: 'Zygisk 内蔵',
      body: 'NyaZygisk がマネージャーに組み込まれており、起動直後から有効になります。別途 Zygisk モジュールを入れる必要はなく、ホーム画面で動作状況をそのまま確認できます。',
      icon: 'solar:shield-check-outline'
    },
    {
      title: 'テーマストア',
      body: 'マネージャー内からテーマの閲覧・インストール・投稿が可能。複数ミラーソース、作者認証、審査ワークフローに対応しています。',
      icon: 'tabler:palette'
    },
    {
      title: 'カーネル拡張センター',
      body: 'CPU ハードウェア識別・vvar 偽装、AVC ログ偽装、バージョン偽装、SUSFS sus_path UID フィルタリング、マウントビュー隠蔽を一箇所に集約。',
      icon: 'solar:settings-outline'
    },
    {
      title: 'Miuix / Material 3 Expressive',
      body: '2 種類の完全な UI スタイルをいつでも切り替え可能。Miuix は Material リデザインに追従し、スプラッシュ画面やステータスカードも全面刷新されています。',
      icon: 'tabler:brush'
    },
    {
      title: '一時的な Root 付与',
      body: '必要なときだけアプリに 1 回限りの Root 権限を与え、使用後はすぐ回収できます。常時権限を与える必要はありません。',
      icon: 'solar:lock-unlocked-outline'
    },
    {
      title: 'LKM インストール',
      body: 'ローダブルカーネルモジュールとしてインストールします。マネージャーが端末の KMI に一致するビルド済みモジュールで boot イメージをパッチするため、カーネルのコンパイルは不要です。',
      icon: 'solar:cpu-bolt-outline'
    },
    {
      title: '非 GKI カーネル対応',
      body: '4.x〜5.4 の非 GKI / プレ GKI カーネルを LTS モード付きでサポート（3.x は実験的対応）。',
      icon: 'arcticons:kernelsu'
    },
    {
      title: 'KPM カーネルモジュール対応',
      body: 'APatch から移植された KPM モジュール機能をサポート。',
      icon: 'solar:download-bold'
    },
    {
      title: 'App Profile',
      body: 'アプリ単位の Root 制御：ユーザーグループ、ケーパビリティ、SELinux ドメイン、マウント名前空間、再利用可能なテンプレートに対応。',
      icon: 'solar:shield-user-outline'
    }
  ]
}

const community = {
  title: 'コミュニティとサポート',
  items: [
    {
      title: 'リポジトリ',
      body: 'ソースコード、不具合報告、リリース一覧',
      href: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU',
      icon: 'M3 5a2 2 0 0 1 2-2h4l2 2h8a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5z'
    },
    {
      title: 'Telegram',
      body: 'アップデート情報、ディスカッション、テストビルド',
      href: 'https://t.me/sukiksu',
      icon: 'm20.665 3.717l-17.73 6.837c-1.21.486-1.203 1.161-.222 1.462l4.552 1.42l10.532-6.645c.498-.303.953-.14.579.192l-8.533 7.701l-.332 4.949c.485 0 .7-.223.971-.486l2.333-2.268l4.852 3.584c.895.493 1.538.239 1.761-.827l3.18-14.99c.326-1.307-.5-1.902-1.355-1.517Z'
    }
  ]
}

const banner = {
  title: '準備はできましたか？',
  body: 'インストールガイドに従って、あなたの端末で SukiSU Nova を動かしましょう。',
  cta: 'インストールガイドを見る'
}

const footer = {
  name: 'SukiSU Nova',
  description: 'カーネルレベル Root、内蔵 Zygisk、テーマストア、カーネル拡張センター',
  links: [
    {
      title: 'リソース',
      items: [
        { label: 'ドキュメント', href: '/ja/guide/' },
        { label: 'リポジトリ', href: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU' },
        { label: 'ダウンロード', href: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU/releases' },
        { label: '不具合報告', href: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU/issues' }
      ]
    },
    {
      title: 'コミュニティ',
      items: [
        { label: 'Telegram チャンネル', href: 'https://t.me/sukiksu' },
        { label: 'コントリビュート', href: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU/src/branch/main/CONTRIBUTING.md' },
        { label: 'ライセンス', href: 'https://git.zako.shirkneko.top:20004/shirkneko/SukiSU/src/branch/main/LICENSE' }
      ]
    }
  ],
  copyright: '© 2025-2026 Saksham. All rights reserved.',
  build: 'Built with VitePress'
}
</script>

<LandingPage
  :hero="hero"
  :overview="overview"
  :features="features"
  :community="community"
  :banner="banner"
  :footer="footer"
/>
