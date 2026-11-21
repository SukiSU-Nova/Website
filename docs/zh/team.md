---
layout: page
title: "团队"
sidebar: false
outline: false
aside: false
---

<script setup>
import { VPTeamPage, VPTeamPageTitle, VPTeamMembers } from 'vitepress/theme'

const coreMembers = [
  {
    avatar: '/avatars/shirkneko.png',
    name: 'Shirk Neko',
    title: '创始人 & 核心开发者',
    desc: 'SukiSU-Nova 创建者',
    links: [
      {
        icon: { svg: '<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M3 5a2 2 0 0 1 2-2h4l2 2h8a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5z" fill="currentColor"/></svg>' },
        link: 'https://git.zako.shirkneko.top:20004/shirkneko'
      },
      { icon: 'telegram', link: 'https://t.me/shirkneko' }
    ]
  },
  {
    avatar: 'https://github.com/shiinasaku.png',
    name: 'サクシイナ',
    title: '核心成员',
    links: [
      { icon: 'github', link: 'https://github.com/shiinasaku' },
      { icon: 'x', link: 'https://x.com/typeflu' },
      { icon: 'telegram', link: 'https://t.me/SakuShiina' }
    ]
  },
  {
    avatar: 'https://github.com/MiRinChan.png',
    name: 'bbB猫 北凛琪',
    title: '首席设计师',
    links: [
      { icon: 'github', link: 'https://github.com/mirinchan' },
      { icon: 'telegram', link: 'https://t.me/bbbneko' }
    ]
  },
  {
    avatar: 'https://github.com/HSSkyBoy.png',
    name: 'NkBe',
    title: '开发者 & 维护者',
    links: [
      { icon: 'github', link: 'https://github.com/HSSkyBoy' },
      { icon: 'telegram', link: 'https://t.me/NP_NBCN' }
    ]
  },
  {
    avatar: 'https://github.com/OukaroMF.png',
    name: 'OukaroMF',
    title: '设计师',
    links: [
      { icon: 'github', link: 'https://github.com/oukaromf' },
      { icon: 'telegram', link: 'https://t.me/mfnotmtf' }
    ]
  },
  {
    avatar: 'https://github.com/xiaomichael.png',
    name: '小 小w',
    title: '内核维护',
    links: [
      { icon: 'github', link: 'https://github.com/xiaomichael' },
      { icon: 'telegram', link: 'https://t.me/xiaoxiao1221w' }
    ]
  }
]
</script>

<VPTeamPage>
  <VPTeamPageTitle>
    <template #title>
      认识我们的团队
    </template>
    <template #lead>
      一支充满热情的开发者、设计师与维护者团队，致力于打造新一代 Android Root 权限管理方案。
    </template>
  </VPTeamPageTitle>
  <VPTeamMembers size="medium" :members="coreMembers" />
</VPTeamPage>

<style scoped>
:deep(.VPTeamPage) {
  padding-top: 32px;
  padding-bottom: 96px;
}

:deep(.VPTeamPageTitle) {
  margin-bottom: 48px;
}

:deep(.VPTeamPageTitle .title) {
  font-size: 48px;
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: -0.02em;
  margin-bottom: 20px;
}

:deep(.VPTeamPageTitle .lead) {
  font-size: 20px;
  line-height: 1.6;
  color: var(--vp-c-text-2);
  max-width: 680px;
  margin: 0 auto;
}

:deep(.VPTeamMembers.medium .container) {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 32px;
  max-width: 1152px;
  margin: 0 auto;
}

:deep(.VPTeamMembers .VPTeamMembersItem) {
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 16px;
  padding: 32px 24px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

:deep(.VPTeamMembers .VPTeamMembersItem::before) {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, var(--vp-c-brand-1), var(--vp-c-brand-2));
  opacity: 0;
  transition: opacity 0.3s ease;
}

:deep(.VPTeamMembers .VPTeamMembersItem:hover) {
  transform: translateY(-8px);
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.15);
  border-color: var(--vp-c-brand-1);
}

:deep(.VPTeamMembers .VPTeamMembersItem:hover::before) {
  opacity: 1;
}

:deep(.VPTeamMembers .profile) {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

:deep(.VPTeamMembers .avatar) {
  width: 96px;
  height: 96px;
  border-radius: 50%;
  margin-bottom: 20px;
  border: 3px solid var(--vp-c-brand-1);
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

:deep(.VPTeamMembers .VPTeamMembersItem:hover .avatar) {
  transform: scale(1.05);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
}

:deep(.VPTeamMembers .name) {
  font-size: 20px;
  font-weight: 600;
  line-height: 1.3;
  margin-bottom: 4px;
  color: var(--vp-c-text-1);
}

:deep(.VPTeamMembers .title) {
  font-size: 14px;
  font-weight: 500;
  line-height: 1.5;
  color: var(--vp-c-brand-1);
  margin-bottom: 8px;
}

:deep(.VPTeamMembers .desc) {
  font-size: 13px;
  line-height: 1.6;
  color: var(--vp-c-text-2);
  margin-bottom: 16px;
}

:deep(.VPTeamMembers .links) {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 16px;
}

:deep(.VPTeamMembers .link) {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background: var(--vp-c-bg);
  color: var(--vp-c-text-2);
  transition: all 0.3s ease;
  border: 1px solid var(--vp-c-divider);
}

:deep(.VPTeamMembers .link:hover) {
  background: var(--vp-c-brand-1);
  color: white;
  border-color: var(--vp-c-brand-1);
  transform: translateY(-3px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

:deep(.VPTeamMembers .link svg) {
  width: 20px;
  height: 20px;
}

@media (max-width: 768px) {
  :deep(.VPTeamPage) {
    padding-top: 48px;
    padding-bottom: 64px;
  }

  :deep(.VPTeamPageTitle .title) {
    font-size: 36px;
  }

  :deep(.VPTeamPageTitle .lead) {
    font-size: 18px;
  }

  :deep(.VPTeamMembers.medium .container) {
    grid-template-columns: 1fr;
    gap: 24px;
  }

  :deep(.VPTeamMembers .VPTeamMembersItem) {
    padding: 24px 20px;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  :deep(.VPTeamMembers.medium .container) {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
