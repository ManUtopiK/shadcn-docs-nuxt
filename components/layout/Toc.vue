<template>
  <template v-if="toc?.links.length">
    <UiScrollArea
      v-if="!isSmall"
      orientation="vertical"
      class="z-30 hidden h-[calc(100vh-6.5rem)] overflow-y-auto lg:block md:block"
      type="hover"
    >
      <p class="mb-2 text-base font-semibold">
        {{ title }}
      </p>
      <LayoutTocTree
        :links="toc.links.filter((x: any) => x.id !== 'hide-toc')"
        :level="0"
        :class="[links.length && 'border-b pb-5']"
      />
      <div v-if="links" class="pt-5 text-muted-foreground">
        <NuxtLink
          v-for="(link, i) in links"
          :key="i"
          :to="link.to"
          :target="link.target"
          class="w-full flex gap-1 underline-offset-4 [&:not(:first-child)]:pt-3 hover:underline"
        >
          <SmartIcon
            v-if="link.icon"
            :name="link.icon"
            class="mr-1 self-center"
          />
          {{ link.title }}
          <Icon name="lucide:arrow-up-right" class="ml-auto self-center text-muted-foreground" size="13" />
        </NuxtLink>
      </div>
    </UiScrollArea>
    <UiCollapsible
      v-else
      v-model:open="isOpen"
      class="block w-full text-sm lg:hidden"
      :class="{ 'border-b': border }"
    >
      <UiCollapsibleTrigger class="w-full flex px-4 py-3 text-left font-medium">
        {{ title }}
        <Icon
          name="lucide:chevron-right"
          class="ml-auto self-center transition-all"
          :class="[isOpen && 'rotate-90']"
        />
      </UiCollapsibleTrigger>
      <UiCollapsibleContent>
        <LayoutTocTree :links="toc.links" :level="0" class="mx-4 mb-3 border-l pl-4 text-sm" />
      </UiCollapsibleContent>
    </UiCollapsible>
  </template>
</template>

<script setup lang="ts">
defineProps<{ isSmall: boolean }>()

const { toc } = useContent()
const { title, links } = useConfig().value.toc
const { border } = useConfig().value.header
const isOpen = ref(false)
</script>
