<template>
  <div
    :class="`go-captcha gc-wrapper ${localConfig.showTheme && 'gc-theme'}`"
    :style="wrapperStyles"
    v-show="hasDisplayWrapperState"
    ref="rootRef"
  >
    <div class="gc-header">
      <span>{{ localConfig.title }}</span>
      <div class="gc-icon-block">
        <close-icon
          :width="localConfig.iconSize"
          :height="localConfig.iconSize"
          @click="handler.closeEvent"
        />
        <refresh-icon
          :width="localConfig.iconSize"
          :height="localConfig.iconSize"
          @click="handler.refreshEvent"
        />
      </div>
    </div>
    <div
      :class="`gc-body ${innerTips.type == 'SUCCESS' && innerTips.show ? 'shine-effect-tr-bl' : ''}`"
      ref="containerRef"
      :style="imageStyles"
    >
      <div class="gc-loading">
        <loading-icon />
      </div>
      <img
        class="gc-picture"
        v-show="hasDisplayImageState"
        :style="imageStyles"
        :src="localData.image"
        alt=""
      />
      <Tips
        v-model:show="innerTips.show"
        :content="innerTips.content"
        :type="innerTips.type"
      />
      <div
        class="gc-tile"
        ref="tileRef"
        :style="thumbStyles"
      >
        <img
          v-show="hasDisplayThumbImageState"
          :src="localData.thumb"
          alt=""
        />
      </div>
    </div>
    <div class="gc-footer">
      <div class="gc-drag-slide-bar" ref="dragBarRef">
        <div class="gc-drag-line" />
        <div
          class="gc-drag-block"
          :class="!hasDisplayImageState && 'disabled'"
          ref="dragBlockRef"
          @mousedown="handler.dragEvent"
          :style="{left: handler.state.dragLeft + 'px'}"
        >
          <div
            class="gc-drag-block-inline"
            @touchstart="handler.dragEvent"
          >
            <arrows-icon />
          </div>
        </div>
    </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import {computed, nextTick, onUnmounted, onMounted, reactive, ref, toRaw, watch} from "vue"
import {SlideConfig, defaultConfig} from "./meta/config";

import CloseIcon from "../../assets/icons/close-icon.vue";
import RefreshIcon from "../../assets/icons/refresh-icon.vue";
import LoadingIcon from "../../assets/icons/loading-icon.vue";
import ArrowsIcon from "../../assets/icons/arrows-icon.vue";

import {defaultSlideData, SlideData} from "./meta/data";
import {SlideEvent} from "./meta/event";
import {SlideExpose} from "./meta/expose";
import {useHandler} from "./hooks/handler";
import Tips from "../base/Tips.vue";

const innerTips = reactive({
  show: false,
  content: '',
  type: 'SUCCESS',
})

// @ts-ignore
const props = withDefaults(
    defineProps<{
      config?: SlideConfig;
      events?: SlideEvent,
      data: SlideData,
      [x: string]: any,
    }>(),
    {
      config: defaultConfig,
      events: () => ({} as SlideEvent),
      data: defaultSlideData,
    },
)

const { data, events, config } = props;
const localData = reactive<SlideData>({...defaultSlideData(), ...toRaw(data)})
const localEvent = reactive<SlideEvent>({...toRaw(events)})
const localConfig = reactive<SlideConfig>({...defaultConfig(), ...toRaw(config)})

watch(() => props.data, (newData, _) => {
  Object.assign(localData, newData)
},{ deep: true });

watch(() => props.events, (newData, _) => {
  Object.assign(localEvent, newData)
},{ deep: true })

watch(() => props.config, (newData, _) => {
  Object.assign(localConfig, newData)
},{ deep: true })

const rootRef = ref<any>(null)
const dragBarRef = ref<any>(null)
const containerRef = ref<any>(null)
const dragBlockRef = ref<any>(null)
const tileRef = ref<any>(null)

const handler = useHandler(
    localData,
    localEvent,
    localConfig,
    rootRef,
    containerRef,
    tileRef,
    dragBlockRef,
    dragBarRef,
    () => {
      localData.thumb = ''
      localData.image = ''
      localData.thumbX = 0
      localData.thumbY = 0
      localData.thumbWidth = 0
      localData.thumbHeight = 0
    }
);

const wrapperStyles = computed(() => {
  const hPadding = localConfig.horizontalPadding || 0
  const vPadding = localConfig.verticalPadding || 0
  const width = (localConfig.width || 0) + ( hPadding * 2) + (localConfig.showTheme ? 2 : 0)

  return {
    width:  width+ "px",
    paddingLeft: hPadding + "px",
    paddingRight: hPadding + "px",
    paddingTop: vPadding + "px",
    paddingBottom: vPadding + "px",
  }
})

const thumbStyles = computed(() => {
  return {
    width: localData.thumbWidth + "px",
    height: localData.thumbHeight + "px",
    top: localData.thumbY + "px",
    left: handler.state.thumbLeft + "px"
  }
})

const imageStyles = computed(() => {
  return {
    width: localConfig.width + "px",
    height: localConfig.height + "px"
  }
})

const hasDisplayImageState = computed(() => {
  return localData.image && localData.image.length > 0
})

const hasDisplayThumbImageState = computed(() => {
  return localData.thumb && localData.thumb.length > 0
})

const hasDisplayWrapperState = computed(() => {
  return (localConfig.width || 0) > 0 || (localConfig.height || 0) > 0
})

const fn = (event: any) => event.preventDefault()
onMounted(async () => {
  await nextTick();
  dragBlockRef.value && dragBlockRef.value.addEventListener('dragstart', fn);
});

onUnmounted(() => {
  dragBlockRef.value && dragBlockRef.value.removeEventListener('dragstart', fn);
})

defineExpose<SlideExpose>({
  reset: handler.resetData,
  clear: handler.clearData,
  refresh: handler.refresh,
  close: handler.close,
  setTips: (show: boolean, content: string, type: string) => {
    innerTips.show = show
    innerTips.content = content
    innerTips.type = type
  }
});
</script>

<style lang="less">
.go-captcha {
  .gc-tile {
    position: absolute;
    z-index: 2;
    cursor: pointer;

    img {
      display: block;
      cursor: pointer;
      width: 100%;
      height: 100%;
    }
  }
}
</style>
