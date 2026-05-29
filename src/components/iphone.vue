<!-- eslint-disable check-file/filename-naming-convention -->
<script lang="ts" setup>
import { Comment, Text, getCurrentInstance, useSlots } from 'vue';

interface Props {
  width?: number;
  height?: number;
  src?: string;
  previewUrl?: string;
  mode?: 'auto' | 'slot' | 'iframe' | 'image';
  iframeAllow?: string;
}

const props = withDefaults(defineProps<Props>(), {
  width: 453,
  height: 882,
  mode: 'auto',
  iframeAllow: 'geolocation *',
});

const slots = useSlots();
const hasRenderableSlotContent = () => {
  const nodes = slots.default?.() ?? [];

  return nodes.some((node) => {
    if (node.type === Comment) {
      return false;
    }

    if (node.type === Text) {
      return String(node.children ?? '').trim().length > 0;
    }

    return true;
  });
};

const resolveContentMode = () => {
  if (props.mode !== 'auto') {
    return props.mode;
  }

  if (hasRenderableSlotContent()) {
    return 'slot';
  }

  if (props.previewUrl) {
    return 'iframe';
  }

  if (props.src) {
    return 'image';
  }

  return 'empty';
};

const clipPathId = `roundedCorners-${getCurrentInstance()?.uid ?? 'phone'}`;

// 这个 screen 定义的是屏幕可见区域和裁切区域，改这里会同时影响内容区和裁切边界。
const screen = {
  x: 22.5,
  y: 19.25,
  width: 428.1,
  height: 843.5,
  radius: 55.75,
} as const;
</script>

<template>
  <svg
      class="phone-frame"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      :width="props.width"
      :height="props.height"
      :viewBox="`0 0 ${props.width+30} ${props.height}`"
  >
    <!-- 外壳阴影层，先铺底，制造手机壳厚度感 -->
    <path
        d="M2 73C2 32.6832 34.6832 0 75 0H377C417.317 0 450 32.6832 450 73V809C450 849.317 417.317 882 377 882H75C34.6832 882 2 849.317 2 809V73Z"
        class="phone-frame-shadow"
    />
    <!-- 左侧按键 -->
    <path
        d="M0 171C0 170.448 0.447715 170 1 170H3V204H1C0.447715 204 0 203.552 0 203V171Z"
        class="phone-frame-shadow"
    />
    <!-- 左侧按键 -->
    <path
        d="M1 234C1 233.448 1.44772 233 2 233H3.5V300H2C1.44772 300 1 299.552 1 299V234Z"
        class="phone-frame-shadow"
    />
    <!-- 左侧按键 -->
    <path
        d="M1 319C1 318.448 1.44772 318 2 318H3.5V385H2C1.44772 385 1 384.552 1 384V319Z"
        class="phone-frame-shadow"
    />
    <!-- 右侧按键（位置右移20） -->
    <path
        d="M450 279H452C452.552 279 453 279.448 453 280V384C453 384.552 452.552 385 452 385H450V279Z"
        class="phone-frame-shadow"
    />
    <!-- 机身主体底色 -->
    <path
        d="M6 74C6 35.3401 37.3401 4 76 4H376C414.66 4 446 35.3401 446 74V808C446 846.66 414.66 878 376 878H76C37.3401 878 6 846.66 6 808V74Z"
        class="phone-frame-body"
    />
    <!-- 听筒/顶部开孔（位置右移10，保持居中） -->
    <path
        opacity="0.5"
        d="M184 5H268V5.5C268 6.60457 267.105 7.5 266 7.5H186C184.895 7.5 184 6.60457 184 5.5V5Z"
        class="phone-frame-speaker"
    />
    <!-- 屏幕边框区域，内容会贴在这里显示 -->
    <path
        d="M21.25 75C21.25 44.2101 46.2101 19.25 77 19.25H375C405.79 19.25 430.75 44.2101 430.75 75V807C430.75 837.79 405.79 862.75 375 862.75H77C46.2101 862.75 21.25 837.79 21.25 807V75Z"
        class="phone-frame-screen"
    />
    <!-- 图片模式：直接用静态图铺满屏幕区域 -->
    <image
        v-if="resolveContentMode() === 'image'"
        x="21.25"
        y="19.25"
        width="389.5"
        height="843.5"
        preserveAspectRatio="xMidYMid slice"
        :clip-path="`url(#${clipPathId})`"
        :href="src"
    />
    <!-- slot / iframe 模式：把真实 HTML 内容放进屏幕区域 -->
    <foreignObject
        v-else-if="resolveContentMode() === 'slot' || resolveContentMode() === 'iframe'"
        :x="screen.x-20"
        :y="screen.y"
        :width="screen.width"
        :height="screen.height"
        :clip-path="`url(#${clipPathId})`"
    >
      <div xmlns="http://www.w3.org/1999/xhtml" class="phone-screen">
        <!-- slot 模式：直接渲染父组件传入的内容 -->
        <slot v-if="resolveContentMode() === 'slot'" />
        <!-- iframe 模式：加载 uniapp 的 H5 预览地址 -->
        <iframe
            v-else-if="resolveContentMode() === 'iframe'"
            class="phone-screen-frame"
            :src="previewUrl"
            :allow="props.iframeAllow"
            title="uniapp mobile preview"
        />
      </div>
    </foreignObject>

    <path
        d="M154 48.5C154 38.2827 162.283 30 172.5 30H259.5C269.717 30 278 38.2827 278 48.5C278 58.7173 269.717 67 259.5 67H172.5C162.283 67 154 58.7173 154 48.5Z"
        class="phone-frame-notch"
    />
    <path
        d="M249 48.5C249 42.701 253.701 38 259.5 38C265.299 38 270 42.701 270 48.5C270 54.299 265.299 59 259.5 59C253.701 59 249 54.299 249 48.5Z"
        class="phone-frame-notch"
    />
    <path
        d="M254 48.5C254 45.4624 256.462 43 259.5 43C262.538 43 265 45.4624 265 48.5C265 51.5376 262.538 54 259.5 54C256.462 54 254 51.5376 254 48.5Z"
        class="phone-frame-accent"
    />
    <!-- 裁切路径：屏幕内容最终按这个圆角矩形裁边 -->
    <defs>
      <clipPath :id="clipPathId">
        <rect
            :x="screen.x"
            :y="screen.y"
            :width="screen.width"
            :height="screen.height"
            :rx="screen.radius"
            :ry="screen.radius"
        />
      </clipPath>
    </defs>
  </svg>
</template>

<style scoped>
.phone-frame {

  display: block;
  flex: none;
  --phone-frame-shadow: #e5e5e5;
  --phone-frame-body: #ffffff;
  --phone-frame-speaker: #f5f5f5;
  --phone-frame-notch: #2b2a2a;
  --phone-frame-accent: #e5e5e5;
  --phone-screen-background: #ffffff;
}

@media (prefers-color-scheme: dark) {
  .phone-frame {
    --phone-frame-shadow: #404040;
    --phone-frame-body: #262626;
    --phone-frame-speaker: #262626;
    --phone-frame-notch: #262626;
    --phone-frame-accent: #404040;
    --phone-screen-background: #111111;
  }
}

.phone-frame-shadow {
  fill: var(--phone-frame-shadow);
}

.phone-frame-body {
  fill: var(--phone-frame-body);
}

.phone-frame-speaker {
  fill: var(--phone-frame-speaker);
}

.phone-frame-screen {
  fill: var(--phone-frame-shadow);
  stroke: var(--phone-frame-shadow);
  stroke-width: 0.5;
}

.phone-frame-notch {
  fill: var(--phone-frame-notch);
}

.phone-frame-accent {
  fill: var(--phone-frame-accent);
}

.phone-screen {
  width: 100%;
  height: 100%;
  overflow: hidden;
  border-radius: 55.75px;
  background: var(--phone-screen-background);
}

.phone-screen-frame {
  display: block;
  width: 100%;
  height: 100%;
  margin-left: 11px;
  border: 0;
  background: var(--phone-screen-background);
}
</style>

