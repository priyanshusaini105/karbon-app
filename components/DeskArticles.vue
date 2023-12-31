<script lang="ts" setup>
const props = defineProps<{
  // FIXME: import type from @storipress/karbon/helper
  desk: any
  infiniteScroll?: boolean
  isDeskPage?: boolean
}>()

const { preload, createLoadMore } = useArticleLoader({
  preload: 4,
  chunk: props.infiniteScroll ? 16 : 0,
  condition: [{ type: 'desk', key: 'id', value: props.desk.id }],
})
</script>

<template>
  <section v-if="preload.length || isDeskPage" class="mt-16 grid gap-y-8">
    <div>
      <div class="m-auto grid w-full max-w-[1400px] px-5 lg:px-8">
        <h2 v-if="isDeskPage" class="max-w-[85%] font-sans text-[2rem] font-bold">Desk Name：{{ desk.name }}</h2>
        <div
          v-else
          class="flex items-baseline justify-between pb-2"
          :class="{
            'border-b-[.5px] border-black dark:border-neutral-600': !isDeskPage,
          }"
        >
          <h2 class="max-w-[85%] font-serif text-[2rem] font-bold hover:underline">
            <NuxtLink :to="desk.url">Desk Name：{{ desk.name }}</NuxtLink>
          </h2>
          <div class="relative">
            <NuxtLink :to="desk.url" class="more-hover text-xs uppercase tracking-[.044rem]">See all</NuxtLink>
          </div>
        </div>
      </div>
    </div>

    <div class="m-auto grid w-full max-w-[1400px] gap-8 px-5 md:grid-cols-2 lg:grid-cols-4 lg:px-8">
      <FrontPageCard
        v-for="article in preload"
        :key="article.id"
        :article="article"
        class-content="mt-4 text-left"
        class-title="text-2xl leading-7"
        class-blurb="hidden"
        class-author="justify-start"
        class-meta="mt-2 flex-col"
      />
    </div>

    <InfiniteScroll
      v-if="props.infiniteScroll"
      v-slot="articles"
      :source="createLoadMore"
      class="m-auto grid w-full max-w-[1400px] gap-8 px-5 md:grid-cols-2 lg:grid-cols-4 lg:px-8"
    >
      <template v-for="article in articles.items" :key="article.id">
        <FrontPageCard
          :article="article"
          class-content="mt-4 text-left"
          class-title="text-2xl leading-7"
          class-blurb="hidden"
          class-author="justify-start"
          class-meta="mt-2 flex-col"
        />
      </template>
    </InfiniteScroll>
  </section>
</template>

<style lang="scss" scoped>
.more-hover {
  @apply hover:after:absolute hover:after:top-[1.1rem] hover:after:block hover:after:h-1.5 hover:after:w-full hover:after:bg-sky-800 hover:after:mix-blend-multiply;
  @apply dark:hover:after:bg-orange-300 dark:hover:after:mix-blend-lighten;
}
</style>
