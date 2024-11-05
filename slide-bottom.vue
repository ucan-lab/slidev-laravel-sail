<template>
  <footer
    v-if="$nav.currentLayout !== 'cover'"
    class="absolute bottom-0 left-0 right-0 flex justify-between items-center"
  >
    <span>{{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}</span>
    <span>【サクッと!!】Laravel ローカル開発環境構築ハンズオン</span>
    <img src="/miraito.jpg" class="h-10">
  </footer>
</template>

<style>
footer {
  background-color: #F6D247;
  color: #1e1e1e;
  width: 100%;
  padding-left: 0.5rem;
}
</style>
