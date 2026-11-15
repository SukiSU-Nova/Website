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
  badge: '你的下一个 Android Root 方案',
  badgeHref: 'https://github.com/SukiSU-Nova/SukiSU-Nova/releases',
  title: 'SukiSU Nova',
  subtitle: '面向 Android 的内核级 Root 权限管理，安全与灵活兼得。',
  logo: '/logo.svg',
  primary: { label: '开始使用', href: '/zh/guide/installation' },
  secondary: { label: '访问 GitHub', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova' },
  chips: ['LKM 安装', '内置 Zygisk', '主题商店', 'Miuix / M3E', '支持 GKI 与非 GKI 内核', 'KPM 内核模块', '开源']
}

const overview = {
  title: '以可加载内核模块 (LKM) 方式安装',
  columns: [
    {
      heading: '安装方式',
      rows: [
        { name: 'LKM', note: '由管理器使用预构建模块修补你的 boot 镜像', label: '推荐', level: 'full' },
        { name: 'GKI 内核', note: '刷入预构建的 AnyKernel3 内核', label: '预构建', level: 'basic' },
        { name: '内置集成', note: '集成到你自己的内核源码', label: '进阶', level: 'manual' }
      ]
    },
    {
      heading: '预构建 LKM 镜像',
      rows: [
        { name: '内核 5.10', note: 'android12 · android13', label: 'aarch64 · x86_64', level: 'full' },
        { name: '内核 5.15', note: 'android13 · android14', label: 'aarch64 · x86_64', level: 'full' },
        { name: '内核 6.1', note: 'android14', label: 'aarch64 · x86_64', level: 'full' },
        { name: '内核 6.6', note: 'android15', label: 'aarch64 · x86_64', level: 'full' },
        { name: '内核 6.12', note: 'android16', label: 'aarch64 · x86_64', level: 'full' },
        { name: '内核 6.18', note: 'android17', label: 'aarch64 · x86_64', level: 'full' }
      ]
    }
  ]
}

const features = {
  title: '为什么选择 SukiSU Nova',
  description: '把 Root、伪装、主题与模块体验收进同一个管理器',
  items: [
    {
      title: '内置 Zygisk',
      body: '管理器内嵌 NyaZygisk，开机即生效，不必再单独安装 Zygisk 模块；首页直接显示运行状态。',
      icon: 'solar:shield-check-outline'
    },
    {
      title: '主题商店',
      body: '在管理器里浏览、安装、投稿主题；支持多来源镜像、作者授权与审核流程。',
      icon: 'tabler:palette'
    },
    {
      title: '内核扩展中心',
      body: 'CPU 硬件识别与 vvar 伪装、AVC 日志伪装、版本伪装、SUSFS sus_path UID 过滤与挂载视图隐藏，集中在一个入口。',
      icon: 'solar:settings-outline'
    },
    {
      title: 'Miuix / Material 3 Expressive',
      body: '两套完整界面随时切换；Miuix 同步 Material 重设计，开屏与状态卡全面重写。',
      icon: 'tabler:brush'
    },
    {
      title: '临时 Root 授权',
      body: '需要时给应用一次性的 Root，用完即收回，不必长期放权。',
      icon: 'solar:lock-unlocked-outline'
    },
    {
      title: 'LKM 安装',
      body: '以可加载内核模块方式安装。管理器会用与你设备 KMI 匹配的预构建模块修补 boot 镜像，无需自行编译内核',
      icon: 'solar:cpu-bolt-outline'
    },
    {
      title: '非 GKI 内核支持',
      body: '支持 4.x-5.4 的非 GKI / 预 GKI 内核并提供 LTS 模式 (3.x 为实验性)',
      icon: 'arcticons:kernelsu'
    },
    {
      title: '支持 KPM 内核模块',
      body: '移植自 APatch 的 KPM 模块支持',
      icon: 'solar:download-bold'
    },
    {
      title: 'App Profile',
      body: '按应用控制 Root：用户组、权能、SELinux 域、挂载命名空间，以及可复用的模板',
      icon: 'solar:shield-user-outline'
    },
    {
      title: '越狱模式',
      body: '设备以 Permissive SELinux 启动时，可通过 Magica 获取 Root，并支持开机自动越狱',
      icon: 'solar:lock-unlocked-outline'
    }
  ]
}

const community = {
  title: '社区与支持',
  items: [
    {
      title: 'GitHub',
      body: '源码、问题反馈与版本发布',
      href: 'https://github.com/SukiSU-Nova/SukiSU-Nova',
      icon: 'M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5c.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34c-.46-1.16-1.11-1.47-1.11-1.47c-.91-.62.07-.6.07-.6c1 .07 1.53 1.03 1.53 1.03c.87 1.52 2.34 1.07 2.91.83c.09-.65.35-1.09.63-1.34c-2.22-.25-4.55-1.11-4.55-4.92c0-1.11.38-2 1.03-2.71c-.1-.25-.45-1.29.1-2.64c0 0 .84-.27 2.75 1.02c.79-.22 1.63-.33 2.47-.33c.84 0 1.68.11 2.47.33c1.91-1.29 2.75-1.02 2.75-1.02c.55 1.35.2 2.39.1 2.64c.65.71 1.03 1.6 1.03 2.71c0 3.82-2.34 4.66-4.57 4.91c.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2Z'
    },
    {
      title: 'Telegram',
      body: '更新动态、讨论与测试版',
      href: 'https://t.me/sukiksu',
      icon: 'm20.665 3.717l-17.73 6.837c-1.21.486-1.203 1.161-.222 1.462l4.552 1.42l10.532-6.645c.498-.303.953-.14.579.192l-8.533 7.701l-.332 4.949c.485 0 .7-.223.971-.486l2.333-2.268l4.852 3.584c.895.493 1.538.239 1.761-.827l3.18-14.99c.326-1.307-.5-1.902-1.355-1.517Z'
    }
  ]
}

const banner = {
  title: '准备好了吗？',
  body: '跟随安装指南，在你的设备上运行 SukiSU Nova。',
  cta: '查看安装指南'
}

const footer = {
  name: 'SukiSU Nova',
  description: '内核级 Root、内置 Zygisk、主题商店与内核扩展中心',
  links: [
    {
      title: '资源',
      items: [
        { label: '文档', href: '/zh/guide/' },
        { label: 'GitHub 仓库', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova' },
        { label: '下载', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/releases' },
        { label: '问题反馈', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/issues' }
      ]
    },
    {
      title: '社区',
      items: [
        { label: 'Telegram 频道', href: 'https://t.me/sukiksu' },
        { label: 'GitHub 讨论区', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/discussions' },
        { label: '参与贡献', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/blob/main/CONTRIBUTING.md' },
        { label: '许可协议', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/blob/main/LICENSE' }
      ]
    }
  ],
  copyright: '© 2025-2026 Saksham. 保留所有权利。',
  build: '使用 VitePress 构建'
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
