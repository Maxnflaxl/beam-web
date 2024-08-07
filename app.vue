<script lang="ts" setup>
import { UserInteractionEvents, eventBus } from "~/utils/emitter";
import { scrollToComponent } from "~/utils/scrollToComponent";

const localePath = useLocalePath();
const router = useRouter();

defineRobotMeta();

const scrollToIdOnPage = async (
  pageName: string,
  componentId: string,
  eventToWait: UserInteractionEvents,
) => {
  const targetComponent = document.getElementById(componentId);
  if (!targetComponent) {
    const pageLoadedAndMounted = () => {
      eventBus.off(eventToWait, pageLoadedAndMounted);
      setTimeout(() => {
        const targetComponent = document.getElementById(componentId);
        scrollToComponent(targetComponent);
      }, 1);
    };
    eventBus.on(eventToWait, pageLoadedAndMounted);
    await router.push(localePath(pageName));
  } else {
    scrollToComponent(targetComponent);
  }
};

// Scroll to "Where to buy" called but unable to scroll, redirect to homepage
const whereToBuyScroll = async () =>
  await scrollToIdOnPage(
    "/",
    "targetComponentBuy",
    UserInteractionEvents.BUY_WHERE_COMPONENT_READY,
  );
const roadmapScroll = async () =>
  await scrollToIdOnPage(
    "/",
    "targetComponentRoadmap",
    UserInteractionEvents.ROADMAP_COMPONENT_READY,
  );

const hardforkScroll = async () =>
  await scrollToIdOnPage(
    "/",
    "targetComponentHardfork",
    UserInteractionEvents.Hardfork_COMPONENT_READY,
  );

onMounted(() => {
  eventBus.on(UserInteractionEvents.SCROLL_TO_WHERE_TO_BUY, whereToBuyScroll);
  eventBus.on(UserInteractionEvents.SCROLL_TO_ROADMAP, roadmapScroll);
  eventBus.on(UserInteractionEvents.SCROLL_TO_HARDFORK, hardforkScroll);
});

onUnmounted(() => {
  eventBus.off(UserInteractionEvents.SCROLL_TO_WHERE_TO_BUY, whereToBuyScroll);
  eventBus.off(UserInteractionEvents.SCROLL_TO_ROADMAP, roadmapScroll);
  eventBus.off(UserInteractionEvents.SCROLL_TO_HARDFORK, hardforkScroll);
});
</script>

<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>

<style>
@font-face {
  font-family: "ProximaNova";
  src:
    url("/fonts/ProximaNova-Regular.woff2") format("woff2"),
    url("/fonts/ProximaNova-Regular.woff") format("woff");
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: "ProximaNova-Italic";
  src:
    url("/fonts/ProximaNova-RegularIt.woff2") format("woff2"),
    url("/fonts/ProximaNova-RegularIt.woff") format("woff");
  font-weight: normal;
  font-style: italic;
  font-display: swap;
}

@font-face {
  font-family: "ProximaNova-Bold";
  src:
    url("/fonts/ProximaNova-Bold.woff2") format("woff2"),
    url("/fonts/ProximaNova-Bold.woff") format("woff");
  font-weight: bold;
  font-style: normal;
  font-display: swap;
}

:root {
  --beam-blue: #99ecff;
  --beam-blue-dark: #0b76ff;
  --beam-purple: #9d6eff;
  --beam-purple-dark: #ab37e6;
  --beam-pink: #a69eff;
  --beam-green: #39fff2;
  --beam-green-dark: #00e2c2;

  @apply min-h-screen
      min-w-full
      overflow-x-hidden
      font-medium
      not-italic
      text-[#fff]
      antialiased;

  ::selection {
    @apply bg-[#39FFF2] text-[#042548];
  }
}

.custom-swipe-arrow {
  @apply absolute z-10 cursor-pointer md:from-[#041D3C] h-full w-7 md:w-10 flex items-center transition-opacity;

  &.swiper-button-disabled {
    @apply opacity-0 pointer-events-none;
  }

  &.next-button {
    @apply right-0 md:bg-gradient-to-l justify-end transition-opacity;
  }

  &.prev-button {
    @apply left-0 md:bg-gradient-to-r justify-start transition-opacity;
  }
}

.background-radial-defaults {
  @apply bg-[length:1024px_576px] md:bg-[length:1280px_720px] lg:bg-[length:1600px_900px] xl:bg-[length:1920px_1080px] bg-[center_top] bg-no-repeat;
}

@keyframes moveGradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.specialGradient {
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  animation: moveGradient 5s linear infinite;

  &.specialGradientPurple {
    background-image: radial-gradient(
      78.77% 78.77% at 71.71% 30.77%,
      #f7f2ff 0%,
      #d8bfff 67.21%,
      #c8afff 76.04%,
      #a789ff 84.9%,
      #8b5dff 94.79%
    );
  }

  &.specialGradientGreen {
    background-image: radial-gradient(
      78.77% 78.77% at 71.71% 30.77%,
      #ffffff 0%,
      #b9ffc9 67.21%,
      #94e8b1 76.04%,
      #5ae69d 84.9%,
      #2cb277 94.79%
    );
  }

  &.specialGradientBlue {
    background-image: radial-gradient(
      78.77% 78.77% at 71.71% 30.77%,
      #f0fcff 0%,
      #9bedff 67.21%,
      #98ecff 76.04%,
      #5be1ff 84.9%,
      #00bae2 94.79%
    );
  }
}
</style>
