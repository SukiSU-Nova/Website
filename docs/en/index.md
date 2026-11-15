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
  badge: 'Your next Android Root solution',
  badgeHref: 'https://github.com/SukiSU-Nova/SukiSU-Nova/releases',
  title: 'SukiSU Nova',
  subtitle: 'Kernel-level root access management for Android — security and flexibility in one.',
  logo: '/logo.svg',
  primary: { label: 'Get Started', href: '/guide/installation' },
  secondary: { label: 'Visit GitHub', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova' },
  chips: ['LKM Installation', 'Built-in Zygisk', 'Theme Store', 'Miuix / M3E', 'GKI & non-GKI kernel support', 'KPM Kernel Modules', 'Open Source']
}

const overview = {
  title: 'Install as a Loadable Kernel Module (LKM)',
  columns: [
    {
      heading: 'Installation Methods',
      rows: [
        { name: 'LKM', note: 'The manager patches your boot image with a prebuilt module', label: 'Recommended', level: 'full' },
        { name: 'GKI Kernel', note: 'Flash a prebuilt AnyKernel3 kernel', label: 'Prebuilt', level: 'basic' },
        { name: 'Built-in Integration', note: 'Integrate into your own kernel source', label: 'Advanced', level: 'manual' }
      ]
    },
    {
      heading: 'Prebuilt LKM Images',
      rows: [
        { name: 'Kernel 5.10', note: 'android12 · android13', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'Kernel 5.15', note: 'android13 · android14', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'Kernel 6.1', note: 'android14', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'Kernel 6.6', note: 'android15', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'Kernel 6.12', note: 'android16', label: 'aarch64 · x86_64', level: 'full' },
        { name: 'Kernel 6.18', note: 'android17', label: 'aarch64 · x86_64', level: 'full' }
      ]
    }
  ]
}

const features = {
  title: 'Why SukiSU Nova',
  description: 'Root, hiding, themes and module experiences — all in one manager',
  items: [
    {
      title: 'Built-in Zygisk',
      body: 'NyaZygisk is embedded in the manager and takes effect right after boot — no separate Zygisk module needed; running status is shown right on the home screen.',
      icon: 'solar:shield-check-outline'
    },
    {
      title: 'Theme Store',
      body: 'Browse, install and submit themes right in the manager; supports multiple mirror sources, author authorization and a review workflow.',
      icon: 'tabler:palette'
    },
    {
      title: 'Kernel Extension Center',
      body: 'CPU hardware identification and vvar spoofing, AVC log spoofing, version spoofing, SUSFS sus_path UID filtering and mount view hiding — all in one place.',
      icon: 'solar:settings-outline'
    },
    {
      title: 'Miuix / Material 3 Expressive',
      body: 'Two complete UI styles to switch anytime; Miuix follows the Material redesign with a fully rewritten splash screen and status cards.',
      icon: 'tabler:brush'
    },
    {
      title: 'Temporary Root Grant',
      body: 'Grant an app one-time root when needed and revoke it right after — no permanent privilege required.',
      icon: 'solar:lock-unlocked-outline'
    },
    {
      title: 'LKM Installation',
      body: 'Install as a loadable kernel module. The manager patches the boot image with a prebuilt module matching your device KMI — no kernel compiling required.',
      icon: 'solar:cpu-bolt-outline'
    },
    {
      title: 'Non-GKI Kernel Support',
      body: 'Supports non-GKI / pre-GKI kernels from 4.x to 5.4, with LTS mode (3.x is experimental).',
      icon: 'arcticons:kernelsu'
    },
    {
      title: 'KPM Kernel Module Support',
      body: 'KPM module support ported from APatch.',
      icon: 'solar:download-bold'
    },
    {
      title: 'App Profile',
      body: 'Per-app root control: user groups, capabilities, SELinux domain, mount namespace, plus reusable templates.',
      icon: 'solar:shield-user-outline'
    },
    {
      title: 'Jailbreak Mode',
      body: 'When the device boots with Permissive SELinux, root can be obtained via Magica, with automatic jailbreak on boot supported.',
      icon: 'solar:lock-unlocked-outline'
    }
  ]
}

const community = {
  title: 'Community & Support',
  items: [
    {
      title: 'GitHub',
      body: 'Source code, issue reports and releases',
      href: 'https://github.com/SukiSU-Nova/SukiSU-Nova',
      icon: 'M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5c.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34c-.46-1.16-1.11-1.47-1.11-1.47c-.91-.62.07-.6.07-.6c1 .07 1.53 1.03 1.53 1.03c.87 1.52 2.34 1.07 2.91.83c.09-.65.35-1.09.63-1.34c-2.22-.25-4.55-1.11-4.55-4.92c0-1.11.38-2 1.03-2.71c-.1-.25-.45-1.29.1-2.64c0 0 .84-.27 2.75 1.02c.79-.22 1.63-.33 2.47-.33c.84 0 1.68.11 2.47.33c1.91-1.29 2.75-1.02 2.75-1.02c.55 1.35.2 2.39.1 2.64c.65.71 1.03 1.6 1.03 2.71c0 3.82-2.34 4.66-4.57 4.91c.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2Z'
    },
    {
      title: 'Telegram',
      body: 'Updates, discussions and beta builds',
      href: 'https://t.me/sukiksu',
      icon: 'm20.665 3.717l-17.73 6.837c-1.21.486-1.203 1.161-.222 1.462l4.552 1.42l10.532-6.645c.498-.303.953-.14.579.192l-8.533 7.701l-.332 4.949c.485 0 .7-.223.971-.486l2.333-2.268l4.852 3.584c.895.493 1.538.239 1.761-.827l3.18-14.99c.326-1.307-.5-1.902-1.355-1.517Z'
    }
  ]
}

const banner = {
  title: 'Ready to dive in?',
  body: 'Follow the installation guide and get SukiSU Nova running on your device.',
  cta: 'View Installation Guide'
}

const footer = {
  name: 'SukiSU Nova',
  description: 'Kernel-level root, built-in Zygisk, theme store and kernel extension center',
  links: [
    {
      title: 'Resources',
      items: [
        { label: 'Documentation', href: '/guide/' },
        { label: 'GitHub Repository', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova' },
        { label: 'Downloads', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/releases' },
        { label: 'Issue Reports', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/issues' }
      ]
    },
    {
      title: 'Community',
      items: [
        { label: 'Telegram Channel', href: 'https://t.me/sukiksu' },
        { label: 'GitHub Discussions', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/discussions' },
        { label: 'Contributing', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/blob/main/CONTRIBUTING.md' },
        { label: 'License', href: 'https://github.com/SukiSU-Nova/SukiSU-Nova/blob/main/LICENSE' }
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
