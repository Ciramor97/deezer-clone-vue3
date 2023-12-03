<template>
  <div class="flex items-center w-full my-5">
    <RouterLink
      :to="pageUrl"
      :class="
        pageUrl === route.path
          ? 'border-l-4 border-l-[#EF5465] text-[#EF5465]'
          : 'text-[#FFFFFF]'
      "
      class="w-full hover:text-[#EF5465]"
      @mouseenter="isHover()"
      @mouseleave="isHover()"
    >
      <div class="flex items-center pl-3 mx-3">
        <img :width="iconSize" :src="`/images/icons/${icon}.png`" />
        <p class="pl-3.5 font-semibold text-base">{{ name }}</p>
      </div>
    </RouterLink>
  </div>
</template>

<script setup lang="ts">
import { toRefs, ref, watchEffect } from "vue";
import { RouterLink, useRoute } from "vue-router";
const route = useRoute();

const props = defineProps<{
  iconString: string;
  iconSize: number;
  pageUrl: string;
  name: string;
}>();

const { iconString, pageUrl, name } = toRefs(props);

let icon = ref<null | string>(null);

const isHover = () => {
  if (icon.value === iconString.value + "-red") {
    icon.value = iconString.value + "-white";
  } else if (icon.value === iconString.value + "-white") {
    icon.value = iconString.value + "-red";
  }
};
watchEffect(() => {
  if (route.path == pageUrl.value && icon.value === iconString.value + "-red")
    return;

  if (route.path == pageUrl.value) {
    icon.value = iconString.value + "-red";
  } else {
    icon.value = iconString.value + "-white";
  }
});
</script>

<style scoped></style>
