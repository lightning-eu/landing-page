<script setup lang="ts">
const route = useRoute()
const formatDate = (value?: string) => (value ? new Date(value).toLocaleDateString() : '')
const readingTime = (post: { readingTime?: number; meta?: { readingTime?: number } }) =>
  post.readingTime ?? post.meta?.readingTime

const { data: doc } = await useAsyncData(`journal-${route.params.slug}`, async () =>
  queryCollection('blog').path(`/blog/${route.params.slug}`).first()
)
</script>

<template>
  <section class="mx-auto max-w-3xl py-10">
    <NuxtLink class="text-sm text-white/60 transition hover:text-white" to="/journals">
      ← Back to Journals
    </NuxtLink>
    <div v-if="doc">
      <div class="mt-6">
        <p class="text-xs uppercase tracking-[0.35em] text-white/50">Journal</p>
        <h1 class="mt-4 text-4xl font-semibold tracking-tight sm:text-5xl">{{ doc.title }}</h1>
        <p class="mt-4 text-base text-white/70">{{ doc.description }}</p>
        <div class="mt-4 flex items-center gap-4 text-xs uppercase tracking-[0.2em] text-white/40">
          <span>{{ formatDate(doc.date ?? doc.meta?.date) }}</span>
          <span v-if="readingTime(doc)">{{ readingTime(doc) }} min read</span>
        </div>
      </div>
      <article class="prose prose-invert mt-10 max-w-none prose-p:text-white/80 prose-a:text-white">
        <ContentRenderer :value="doc" />
      </article>
    </div>
    <div v-else class="mt-10 rounded-3xl border border-white/10 bg-white/5 p-6 text-sm text-white/70">
      Journal not found.
    </div>
  </section>
</template>
