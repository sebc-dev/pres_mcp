<template>
  <div class="slidev-layout cover-title">
    <div
      class="cover-title__bg"
      :style="{ backgroundImage: `url(${$attrs.image || ''})` }"
    />
    <div class="cover-title__overlay" />
    <div class="cover-title__content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.cover-title {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.cover-title__bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
}

.cover-title__overlay {
  position: absolute;
  inset: 0;
  background: var(--theme-cover-overlay);
}

.cover-title__content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  padding: var(--theme-slide-padding);
  text-align: center;
  color: var(--theme-cover-text);
}

.cover-title__content :deep(h1) {
  font-family: var(--theme-font-title);
  font-size: 3.5rem;
  font-weight: 700;
  line-height: 1.2;
  margin: 0;
}

.cover-title__content :deep(h2) {
  font-family: var(--theme-font-body);
  font-size: 1.5rem;
  font-weight: 400;
  margin-top: 1rem;
  opacity: 0.85;
}

.cover-title__content :deep(p) {
  font-size: 1.25rem;
  opacity: 0.8;
  margin-top: 0.5rem;
}
</style>
