<script lang="ts" setup>
import { UserInteractionEvents, eventBus } from "~/utils/emitter";

defineProps({
  isNextStep: {
    type: Boolean,
    required: false,
  },
});

const localePath = useLocalePath();

const targetComponentGetStarted = ref<HTMLElement | null>(null);

const scrollToGetStarted = () => scrollToComponent(targetComponentGetStarted);

onMounted(() =>
  eventBus.on(UserInteractionEvents.SCROLL_TO_GET_STARTED, scrollToGetStarted)
);

onUnmounted(() =>
  eventBus.off(UserInteractionEvents.SCROLL_TO_GET_STARTED, scrollToGetStarted)
);
</script>

<template>
  <LayoutWrapper :center="true">
    <div ref="targetComponentGetStarted">
      <LayoutTitle
        :title="
          !isNextStep ? $t('getstarted.title') : $t('getstarted.nextsteps')
        "
      />
      <div
        :class="`grid sm:grid-cols-2 ${
          !isNextStep ? 'lg:grid-cols-3' : ''
        } items-center gap-6 md:gap-10`"
      >
        <FeaturesCard
          v-if="!isNextStep"
          link="https://documentation.beam.mw/#atomic-swaps"
          image="download/get"
          :title="$t('getstarted.steps.1.title')"
          :link-name="$t('getstarted.steps.1.link')"
          :text="$t('getstarted.steps.1.description')"
          :button-link="localePath('downloads')"
          accent-color="beam-blue"
          :wide-icon="true"
          :as-image="false"
        >
          <LayoutButton
            accent-color="beam-blue"
            class="w-50"
            :button-link="localePath('downloads')"
            :big="true"
          >
            <Icon
              class="w-[18px] h-[18px] select-none pointer-events-none"
              name="layout/flat-beam-animated"
              :as-image="true"
              :lazy="true"
            />
            {{ $t("getstarted.steps.1.button") }}
          </LayoutButton>
        </FeaturesCard>
        <FeaturesCard
          image="get-started/beam-currency"
          :title="$t('getstarted.steps.2.title')"
          :text="$t('getstarted.steps.2.description')"
          accent-color="beam-green"
          :wide-icon="true"
        >
          <LayoutButton
            accent-color="beam-green"
            class="w-50"
            as="button"
            :big="true"
            @click="
              eventBus.emit(UserInteractionEvents.SCROLL_TO_WHERE_TO_BUY, {})
            "
          >
            <Icon
              class="w-[18px] h-[18px] select-none pointer-events-none"
              name="get-started/buy"
            />
            {{ $t("getstarted.steps.2.button") }}
          </LayoutButton>
        </FeaturesCard>
        <FeaturesCard
          image="hero/rocket"
          :title="$t('getstarted.steps.3.title')"
          :text="$t('getstarted.steps.3.description')"
          button-name="Explore Beam"
          accent-color="beam-pink"
          :wide-icon="true"
          :as-image="false"
        >
          <LayoutButton
            accent-color="beam-pink"
            class="w-50"
            :button-link="localePath('dapps')"
            :big="true"
          >
            <Icon
              class="w-[18px] h-[18px] select-none pointer-events-none"
              name="get-started/explore"
            />
            {{ $t("getstarted.steps.3.button") }}
          </LayoutButton>
        </FeaturesCard>
      </div>
      <FaqSection :is-next-step="isNextStep" />
    </div>
  </LayoutWrapper>
</template>