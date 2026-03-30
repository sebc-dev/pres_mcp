<template>
  <div class="slidev-layout chapter-layout">
    <div class="chapter-layout__bg">
      <div class="chapter-layout__grid" />
      <div class="chapter-layout__logo">
        <svg fill="currentColor" fill-rule="evenodd" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M15.688 2.343a2.588 2.588 0 00-3.61 0l-9.626 9.44a.863.863 0 01-1.203 0 .823.823 0 010-1.18l9.626-9.44a4.313 4.313 0 016.016 0 4.116 4.116 0 011.204 3.54 4.3 4.3 0 013.609 1.18l.05.05a4.115 4.115 0 010 5.9l-8.706 8.537a.274.274 0 000 .393l1.788 1.754a.823.823 0 010 1.18.863.863 0 01-1.203 0l-1.788-1.753a1.92 1.92 0 010-2.754l8.706-8.538a2.47 2.47 0 000-3.54l-.05-.049a2.588 2.588 0 00-3.607-.003l-7.172 7.034-.002.002-.098.097a.863.863 0 01-1.204 0 .823.823 0 010-1.18l7.273-7.133a2.47 2.47 0 00-.003-3.537z"/><path d="M14.485 4.703a.823.823 0 000-1.18.863.863 0 00-1.204 0l-7.119 6.982a4.115 4.115 0 000 5.9 4.314 4.314 0 006.016 0l7.12-6.982a.823.823 0 000-1.18.863.863 0 00-1.204 0l-7.119 6.982a2.588 2.588 0 01-3.61 0 2.47 2.47 0 010-3.54l7.12-6.982z"/></svg>
      </div>
    </div>
    <div class="chapter-layout__right">
      <div class="chapter-layout__text">
        <div class="chapter-layout__separator" />
        <h1 class="chapter-layout__title">{{ $attrs.chapter }}</h1>
        <p class="chapter-layout__subtitle" v-if="$attrs.section">{{ $attrs.section }}</p>
        <div class="chapter-layout__slot">
          <slot />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.chapter-layout {
  position: relative;
  height: 100%;
  width: 100%;
  background: var(--theme-bg);
}

.chapter-layout__bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 55%;
  height: 100%;
  overflow: hidden;
}

.chapter-layout__grid {
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32' width='32' height='32' fill='none' stroke='rgb(255 255 255 / 0.05)'%3e%3cpath d='M0 .5H31.5V32'/%3e%3c/svg%3e");
  background-position: 10px 10px;
  mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.6));
  -webkit-mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.6));
}

.chapter-layout__logo {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 130%;
  height: 130%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.04);
  pointer-events: none;
}

.chapter-layout__logo svg {
  width: 100%;
  height: 100%;
}

.chapter-layout__right {
  position: absolute;
  z-index: 1;
  top: 0;
  right: 0;
  display: flex;
  align-items: center;
  width: calc(50% + 30px);
  height: 100%;
  padding: var(--theme-slide-padding);
  padding-left: calc(var(--theme-slide-padding) + 40px);
  background: var(--theme-bg);
  clip-path: polygon(60px 0, 100% 0, 100% 100%, 0 100%);
}

.chapter-layout__text {
  max-width: 100%;
}

.chapter-layout__separator {
  width: var(--theme-separator-width);
  height: var(--theme-separator-thickness);
  background: var(--theme-accent);
  margin-bottom: 1.5rem;
}

.chapter-layout__title {
  font-family: var(--theme-font-title);
  font-size: 2.5rem;
  font-weight: 700;
  color: var(--theme-primary);
  margin: 0 0 0.75rem 0;
  line-height: 1.2;
}

.chapter-layout__subtitle {
  font-family: var(--theme-font-body);
  font-size: 1.25rem;
  color: var(--theme-text-light);
  margin: 0;
}

.chapter-layout__slot :deep(*) {
  margin-top: 1rem;
}
</style>
