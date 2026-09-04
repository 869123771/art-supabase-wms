<template>
  <div class="wms-workbench business-workspace-page art-full-height">
    <BusinessWorkspaceHeader
      eyebrow="WAREHOUSE OPERATIONS"
      title="仓储运营工作台"
      description="围绕收货、上架、库存、拣选与发运建立统一作业视图；当前已完成应用接入，业务能力将按实施路线逐步启用。"
      icon="ri:warehouse-line"
      :tags="[
        { label: '仓内执行', type: 'primary' },
        { label: 'MDM 协同', type: 'success' },
        { label: '架构准备阶段', type: 'info' }
      ]"
      :metrics="metrics"
    />

    <div class="wms-workbench__content">
      <ArtSectionCard
        class="wms-workbench__flow-card"
        title="仓内作业链路"
        subtitle="以一条可追踪链路组织入库、库存和出库作业，后续启用后将在这里呈现实时节点状态。"
        preserve-content-structure
      >
        <template #actions><ElTag type="success" effect="plain" round>边界已定义</ElTag></template>
        <ol class="operation-flow" aria-label="仓内作业流程">
          <li v-for="(stage, index) in operationStages" :key="stage.title">
            <span class="operation-flow__index">{{ String(index + 1).padStart(2, '0') }}</span>
            <span class="operation-flow__icon"><ArtSvgIcon :icon="stage.icon" /></span>
            <div
              ><strong>{{ stage.title }}</strong
              ><small>{{ stage.description }}</small></div
            >
          </li>
        </ol>
        <div class="ownership-band">
          <div
            ><span class="ownership-band__label">MDM 提供</span
            ><strong>仓库 · 库位 · 物料统一身份</strong></div
          >
          <ArtSvgIcon icon="ri:arrow-right-line" />
          <div
            ><span class="ownership-band__label is-wms">WMS 负责</span
            ><strong>库存余额 · 收发存流水 · 作业状态</strong></div
          >
        </div>
      </ArtSectionCard>

      <ArtSectionCard
        class="wms-workbench__readiness-card"
        title="上线准备"
        subtitle="当前接入状态与下一步建设重点。"
        preserve-content-structure
      >
        <ul class="readiness-list">
          <li v-for="item in readinessItems" :key="item.label">
            <span class="readiness-list__icon"><ArtSvgIcon icon="ri:check-line" /></span>
            <div
              ><strong>{{ item.label }}</strong
              ><small>{{ item.description }}</small></div
            >
            <ElTag size="small" type="success" effect="light" round>已完成</ElTag>
          </li>
        </ul>
        <div class="next-step">
          <span><ArtSvgIcon icon="ri:flag-line" /></span>
          <div
            ><small>NEXT MILESTONE</small><strong>建立仓库、库区与库位模型</strong
            ><p>先完成仓网基础与库存台账，再接收入库和出库作业。</p></div
          >
        </div>
      </ArtSectionCard>

      <ArtSectionCard
        class="wms-workbench__roadmap-card"
        title="能力实施路线"
        subtitle="按数据基础、核心执行、运营优化三个阶段推进，减少跨域返工。"
        preserve-content-structure
      >
        <div class="roadmap-grid">
          <article v-for="(phase, index) in roadmap" :key="phase.title">
            <div class="roadmap-grid__heading"
              ><span>{{ index + 1 }}</span
              ><div
                ><small>{{ phase.code }}</small
                ><strong>{{ phase.title }}</strong></div
              ></div
            >
            <p>{{ phase.description }}</p>
            <div class="roadmap-grid__tags"
              ><ElTag v-for="item in phase.items" :key="item" size="small" effect="plain">{{
                item
              }}</ElTag></div
            >
          </article>
        </div>
      </ArtSectionCard>
    </div>
  </div>
</template>

<script setup lang="ts">
  import BusinessWorkspaceHeader, {
    type BusinessWorkspaceMetric
  } from '@/components/business/business-workspace-header/index.vue'
  import ArtSectionCard from '@/components/core/surfaces/art-section-card/index.vue'
  import ArtSvgIcon from '@/components/core/base/art-svg-icon/index.vue'

  defineOptions({ name: 'WmsWorkbench' })

  const metrics: BusinessWorkspaceMetric[] = [
    {
      label: '应用运行时',
      value: '已接入',
      description: '独立子仓与平台壳层',
      icon: 'ri:links-line',
      tone: 'success'
    },
    {
      label: '菜单与权限',
      value: '已注册',
      description: '管理员角色可访问',
      icon: 'ri:shield-keyhole-line',
      tone: 'success'
    },
    {
      label: '主数据协同',
      value: '待配置',
      description: '仓库、库位与物料',
      icon: 'ri:database-2-line',
      tone: 'warning'
    },
    {
      label: '业务作业',
      value: '待启用',
      description: '当前不展示模拟数据',
      icon: 'ri:play-circle-line',
      tone: 'info'
    }
  ]

  const operationStages = [
    { title: '预约', description: '到货计划', icon: 'ri:calendar-check-line' },
    { title: '收货', description: '数量交接', icon: 'ri:inbox-archive-line' },
    { title: '上架', description: '推荐库位', icon: 'ri:stack-line' },
    { title: '拣选', description: '任务执行', icon: 'ri:hand-coin-line' },
    { title: '复核', description: '差异控制', icon: 'ri:checkbox-multiple-line' },
    { title: '发运', description: '装车交接', icon: 'ri:truck-line' }
  ]

  const readinessItems = [
    { label: '独立模块仓库', description: '版本与发布边界已隔离' },
    { label: '平台运行时', description: '共享登录、主题与布局' },
    { label: '菜单权限', description: '应用入口与角色授权已登记' }
  ]

  const roadmap = [
    {
      code: 'FOUNDATION',
      title: '数据基础',
      description: '先建立稳定的仓网模型与库存身份，成为所有作业的共同坐标。',
      items: ['仓库网络', '库存台账', '批次序列']
    },
    {
      code: 'EXECUTION',
      title: '核心执行',
      description: '打通入库、库内与出库闭环，让每个任务可分派、可追踪。',
      items: ['入库作业', '库内作业', '出库作业']
    },
    {
      code: 'OPTIMIZATION',
      title: '运营优化',
      description: '基于真实流水形成库存健康、周转效率和作业绩效指标。',
      items: ['库存预警', '效率分析', '策略优化']
    }
  ]
</script>

<style scoped lang="scss">
  .wms-workbench {
    overflow: auto;

    &__content {
      display: grid;
      flex: 1;
      grid-template-columns: minmax(0, 1.8fr) minmax(300px, 0.8fr);
      gap: 12px;
      min-height: 0;
    }

    &__flow-card,
    &__readiness-card,
    &__roadmap-card {
      padding: 18px;
    }

    &__roadmap-card {
      grid-column: 1 / -1;
    }
  }

  .operation-flow {
    display: grid;
    grid-template-columns: repeat(6, minmax(0, 1fr));
    padding: 22px 8px 20px;
    margin: 0;
    list-style: none;

    li {
      position: relative;
      display: grid;
      justify-items: center;
      min-width: 0;
      text-align: center;

      &:not(:last-child)::after {
        position: absolute;
        top: 25px;
        left: calc(50% + 27px);
        width: calc(100% - 54px);
        height: 1px;
        content: '';
        background: var(--el-border-color);
      }
    }

    &__index {
      position: absolute;
      top: -13px;
      font-size: 9px;
      font-weight: 700;
      color: var(--el-text-color-placeholder);
      letter-spacing: 0.08em;
    }

    &__icon {
      z-index: 1;
      display: grid;
      place-items: center;
      width: 50px;
      height: 50px;
      margin-bottom: 10px;
      font-size: 21px;
      color: var(--el-color-primary);
      background: var(--el-color-primary-light-9);
      border: 1px solid var(--el-color-primary-light-7);
      border-radius: 14px;
    }

    strong,
    small {
      display: block;
    }

    strong {
      font-size: 13px;
      color: var(--el-text-color-primary);
    }

    small {
      margin-top: 3px;
      font-size: 11px;
      color: var(--el-text-color-secondary);
    }
  }

  .ownership-band {
    display: grid;
    grid-template-columns: minmax(0, 1fr) auto minmax(0, 1fr);
    gap: 16px;
    align-items: center;
    padding: 14px 18px;
    background: var(--el-fill-color-lighter);
    border: 1px solid var(--el-border-color-lighter);
    border-radius: var(--el-border-radius-base);

    div {
      display: grid;
      gap: 4px;
    }

    > svg {
      color: var(--el-text-color-placeholder);
    }

    &__label {
      width: fit-content;
      padding: 2px 7px;
      font-size: 10px;
      font-weight: 700;
      color: var(--el-color-success-dark-2);
      background: var(--el-color-success-light-9);
      border-radius: 99px;
    }

    &__label.is-wms {
      color: var(--el-color-primary);
      background: var(--el-color-primary-light-9);
    }

    strong {
      font-size: 12px;
      line-height: 1.5;
    }
  }

  .readiness-list {
    display: grid;
    gap: 2px;
    padding: 0;
    margin: 0;
    list-style: none;

    li {
      display: grid;
      grid-template-columns: auto minmax(0, 1fr) auto;
      gap: 10px;
      align-items: center;
      padding: 10px 0;
    }

    li + li {
      border-top: 1px solid var(--el-border-color-lighter);
    }

    &__icon {
      display: grid;
      place-items: center;
      width: 28px;
      height: 28px;
      color: var(--el-color-success);
      background: var(--el-color-success-light-9);
      border-radius: 8px;
    }

    strong,
    small {
      display: block;
    }

    strong {
      font-size: 13px;
    }

    small {
      margin-top: 2px;
      font-size: 11px;
      color: var(--el-text-color-secondary);
    }
  }

  .next-step {
    display: flex;
    gap: 12px;
    padding: 14px;
    margin-top: 12px;
    background: color-mix(in srgb, var(--el-color-warning) 8%, var(--el-bg-color));
    border: 1px solid var(--el-color-warning-light-7);
    border-radius: var(--el-border-radius-base);

    > span {
      display: grid;
      flex: 0 0 34px;
      place-items: center;
      width: 34px;
      height: 34px;
      color: var(--el-color-warning-dark-2);
      background: var(--el-color-warning-light-9);
      border-radius: 9px;
    }

    small,
    strong {
      display: block;
    }

    small {
      font-size: 9px;
      font-weight: 700;
      color: var(--el-color-warning-dark-2);
      letter-spacing: 0.1em;
    }

    strong {
      margin-top: 3px;
      font-size: 13px;
    }

    p {
      margin: 4px 0 0;
      font-size: 11px;
      line-height: 1.55;
      color: var(--el-text-color-secondary);
    }
  }

  .roadmap-grid {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    overflow: hidden;
    border: 1px solid var(--el-border-color-lighter);
    border-radius: var(--el-border-radius-base);

    article {
      min-width: 0;
      padding: 16px 18px;
    }

    article + article {
      border-left: 1px solid var(--el-border-color-lighter);
    }

    article p {
      min-height: 38px;
      margin: 10px 0 12px;
      font-size: 12px;
      line-height: 1.6;
      color: var(--el-text-color-secondary);
    }

    &__heading {
      display: flex;
      gap: 10px;
      align-items: center;
    }

    &__heading > span {
      display: grid;
      place-items: center;
      width: 32px;
      height: 32px;
      font-size: 12px;
      font-weight: 700;
      color: var(--el-color-primary);
      background: var(--el-color-primary-light-9);
      border-radius: 9px;
    }

    &__heading small,
    &__heading strong {
      display: block;
    }

    &__heading small {
      font-size: 9px;
      color: var(--el-text-color-placeholder);
      letter-spacing: 0.08em;
    }

    &__heading strong {
      margin-top: 2px;
      font-size: 14px;
    }

    &__tags {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
    }
  }

  @media (width <= 1024px) {
    .wms-workbench__content {
      grid-template-columns: 1fr;
    }

    .wms-workbench__roadmap-card {
      grid-column: auto;
    }
  }

  @media (width <= 720px) {
    .operation-flow {
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 18px 8px;
    }

    .operation-flow li::after {
      display: none;
    }

    .ownership-band,
    .roadmap-grid {
      grid-template-columns: 1fr;
    }

    .ownership-band > svg {
      transform: rotate(90deg);
    }

    .roadmap-grid article + article {
      border-top: 1px solid var(--el-border-color-lighter);
      border-left: 0;
    }
  }
</style>
