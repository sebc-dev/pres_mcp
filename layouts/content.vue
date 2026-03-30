<template>
  <div class="slidev-layout content-layout">
    <div class="content-layout__grid" />
    <div class="content-layout__breadcrumb" v-if="$attrs.chapter">
      <span class="content-layout__chapter">{{ $attrs.chapter }}</span>
      <span class="content-layout__divider" v-if="$attrs.section">/</span>
      <span class="content-layout__section" v-if="$attrs.section">{{ $attrs.section }}</span>
    </div>
    <div class="content-layout__body">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.content-layout {
  position: relative;
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 100%;
  background: var(--theme-bg);
  padding: 0;
}

.content-layout__grid {
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32' width='32' height='32' fill='none' stroke='rgb(255 255 255 / 0.025)'%3e%3cpath d='M0 .5H31.5V32'/%3e%3c/svg%3e");
  background-position: 10px 10px;
  mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.4));
  -webkit-mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.4));
  pointer-events: none;
  z-index: 0;
}

.content-layout__breadcrumb {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  background: var(--theme-breadcrumb-bg);
  border-bottom: 1px solid var(--theme-breadcrumb-border);
  font-family: var(--theme-font-body);
  font-size: 0.875rem;
  flex-shrink: 0;
  margin: 0;
  width: 100%;
}

.content-layout__chapter {
  color: var(--theme-primary);
  font-weight: 600;
}

.content-layout__divider {
  color: var(--theme-text-light);
}

.content-layout__section {
  color: var(--theme-text-light);
}

.content-layout__body {
  position: relative;
  z-index: 1;
  flex: 1;
  padding: var(--theme-slide-padding);
  overflow: auto;
  color: var(--theme-text);
}

.content-layout__body :deep(h1) {
  font-family: var(--theme-font-title);
  font-size: 2rem;
  font-weight: 700;
  color: var(--theme-primary);
  margin: 0 0 1rem 0;
}

.content-layout__body :deep(h2) {
  font-family: var(--theme-font-title);
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--theme-primary-light);
  margin: 1.5rem 0 0.75rem 0;
}

.content-layout__body :deep(ul),
.content-layout__body :deep(ol) {
  margin: 0.5rem 0;
  padding-left: 1.5rem;
}

.content-layout__body :deep(li) {
  margin: 0.25rem 0;
}

.content-layout__body :deep(code) {
  background: #1a1a1e;
  padding: 0.125rem 0.375rem;
  border-radius: 0.25rem;
  font-size: 0.9em;
  color: #f3f3f3;
}
</style>
