<template>
  <main class="container mx-auto max-w-[1600px] p-4 flex flex-col lg:flex-row lg:flex-wrap justify-center items-start gap-8">
    <!-- 控制面板 -->
    <div class="w-full lg:flex-1 flex flex-col p-6 bg-white rounded-xl shadow-lg backdrop-blur-sm bg-opacity-95">
      <!-- 图标选择器 -->
      <div class="flex gap-3 items-center mb-4">
        <input 
          type="text" 
          v-model="iconName"
          @input="loadIcon"
          placeholder="输入图标名称，例如 logos:chrome"
          class="flex-1 px-4 py-2.5 border border-gray-200 rounded-lg focus:border-green-500 focus:ring-2 focus:ring-green-500/20 outline-none transition-all duration-300 hover:border-green-400 text-sm bg-gray-50"
        />
        <a 
          href="https://yesicon.app/" 
          target="_blank"
          class="px-4 py-2.5 bg-gradient-to-r from-green-500 to-green-600 text-white rounded-lg hover:from-green-600 hover:to-green-700 transition-all duration-300 text-sm font-medium shadow-sm hover:shadow whitespace-nowrap"
        >图标库</a>
      </div>

      <!-- 背景设置 -->
      <div class="flex gap-3 mb-4">
        <label 
          for="inputBgImage" 
          class="flex-1 px-4 py-2.5 bg-gradient-to-r from-green-500 to-green-600 text-white rounded-lg hover:from-green-600 hover:to-green-700 transition-all duration-300 cursor-pointer text-center text-sm font-medium shadow-sm hover:shadow"
        >上传背景图片</label>
        <input type="file" id="inputBgImage" accept="image/*" @change="updatePreview('bg', $event)" class="hidden">
        <label 
          for="inputSquareImage" 
          class="flex-1 px-4 py-2.5 bg-gradient-to-r from-green-500 to-green-600 text-white rounded-lg hover:from-green-600 hover:to-green-700 transition-all duration-300 cursor-pointer text-center text-sm font-medium shadow-sm hover:shadow"
        >上传图标图片</label>
        <input type="file" id="inputSquareImage" accept="image/*" @change="updatePreview('square', $event)" class="hidden">
        <a 
          href="https://zhangyu1818.github.io/appicon-forge/" 
          target="_blank"
          class="px-4 py-2.5 bg-gradient-to-r from-green-500 to-green-600 text-white rounded-lg hover:from-green-600 hover:to-green-700 transition-all duration-300 text-sm font-medium shadow-sm hover:shadow whitespace-nowrap"
        >图标下载站</a>
      </div>

      <!-- 颜色设置 -->
      <div class="grid grid-cols-2 gap-4 mb-4">
        <div class="flex items-center gap-3">
          <label class="whitespace-nowrap text-sm font-medium text-gray-700" for="inputTextColor">标题颜色</label>
          <input 
            type="color" 
            id="inputTextColor"
            v-model="state.textColor"
            @input="updatePreview('textColor', $event)"
            class="w-full h-8 rounded-lg cursor-pointer border border-gray-200"
          >
        </div>
        <div class="flex items-center gap-3">
          <label class="whitespace-nowrap text-sm font-medium text-gray-700" for="inputWatermarkColor">水印颜色</label>
          <input 
            type="color"
            id="inputWatermarkColor"
            v-model="state.watermarkColor"
            @input="updatePreview('watermarkColor', $event)"
            class="w-full h-8 rounded-lg cursor-pointer border border-gray-200"
          >
        </div>
      </div>

      <!-- 背景模糊设置 -->
      <div class="flex flex-col sm:flex-row items-center gap-4 mb-4">
        <div class="w-full sm:flex-[6] flex items-center gap-3">
          <label class="whitespace-nowrap text-sm font-medium text-gray-700" for="inputBgBlur">背景模糊</label>
          <input 
            type="range"
            id="inputBgBlur"
            min="0"
            max="20"
            v-model="state.bgBlur"
            @input="updatePreview('bgBlur', $event)"
            class="flex-1 h-2 bg-gray-200 rounded-full appearance-none cursor-pointer focus:outline-none focus:ring-2 focus:ring-green-500/20"
          >
        </div>
        <div class="w-full sm:flex-[4] flex items-center gap-3">
          <label class="whitespace-nowrap text-sm font-medium text-gray-700" for="inputBgColor">背景颜色</label>
          <input 
            type="color"
            id="inputBgColor"
            v-model="state.bgColor"
            @input="updatePreview('bgColor', $event)"
            class="w-full h-8 rounded-lg cursor-pointer border border-gray-200"
          >
        </div>
      </div>

      <!-- 图标和阴影设置 -->
      <div 
        class="flex flex-col gap-3 overflow-hidden transition-all duration-300 ease-out"
        :class="state.squareImageUrl ? 'mb-3 max-h-[300px] sm:max-h-[200px] opacity-100' : 'max-h-0 opacity-0'"
      >
        <!-- 图标控制 -->
        <div class="flex flex-col sm:flex-row gap-3">
          <div class="w-full sm:flex-1 flex items-center gap-2">
            <label class="whitespace-nowrap" for="inputSquareSize">图标大小</label>
            <input 
              type="range"
              id="inputSquareSize"
              min="200"
              max="500"
              v-model="state.squareSize"
              @input="updatePreview('squareSize', $event)"
              class="flex-1 h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            >
          </div>
          <div class="w-full sm:flex-1 flex items-center gap-2">
            <label class="whitespace-nowrap" for="inputRotation">图标旋转</label>
            <input 
              type="range"
              id="inputRotation"
              min="0"
              max="360"
              v-model="state.rotation"
              @input="updatePreview('rotation', $event)"
              class="flex-1 h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            >
          </div>
        </div>

        <!-- 阴影控制 -->
        <div class="flex flex-col sm:flex-row gap-3">
          <div class="w-full sm:flex-[6] flex items-center gap-2">
            <label class="whitespace-nowrap" for="inputShadowStrength">图标阴影大小</label>
            <input 
              type="range"
              id="inputShadowStrength"
              min="0"
              max="100"
              v-model.number="state.shadowStrength"
              @input="updatePreview('shadowStrength', $event)"
              class="flex-1 h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            >
          </div>
          <div class="w-full sm:flex-[4] flex items-center gap-2">
            <label class="whitespace-nowrap" for="inputShadowColor">图标阴影颜色</label>
            <input 
              type="color"
              id="inputShadowColor"
              :value="state.shadowColor.startsWith('rgba') ? '#000000' : state.shadowColor"
              @input="updatePreview('shadowColor', $event)"
              class="w-full h-6 rounded cursor-pointer"
            >
          </div>
        </div>

        <!-- 图标背景控制 -->
        <div class="flex flex-col sm:flex-row gap-3">
          <div class="w-full sm:flex-[6] flex items-center gap-2">
            <label class="whitespace-nowrap" for="inputIconBgSize">图标背景大小</label>
            <input 
              type="range"
              id="inputIconBgSize"
              min="0"
              max="20"
              v-model="state.iconBgSize"
              @input="updatePreview('iconBgSize', $event)"
              class="flex-1 h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
            >
          </div>
          <div class="w-full sm:flex-[4] flex items-center gap-2">
            <label class="whitespace-nowrap" for="inputIconColor">图标背景颜色</label>
            <input 
              type="color"
              id="inputIconColor"
              v-model="state.iconColor"
              @input="updatePreview('iconColor', $event)"
              class="w-full h-6 rounded cursor-pointer"
            >
          </div>
        </div>
      </div>

      <!-- 文本设置 -->
      <textarea 
        id="inputText"
        :value="''"
        @input="updatePreview('text', $event)"
        placeholder="输入标题"
        rows="2"
        class="w-full min-h-[60px] px-4 py-3 border border-gray-200 rounded-lg focus:border-green-500 focus:ring-2 focus:ring-green-500/20 outline-none transition-all duration-300 hover:border-green-400 resize-y mb-4 bg-gray-50 text-sm"
      ></textarea>

      <!-- 水印设置 -->
      <div class="flex items-center gap-3 mb-4">
        <input 
          type="text"
          id="inputWatermark"
          @input="updatePreview('watermark', $event)"
          placeholder="输入水印"
          class="w-full px-4 py-2.5 border border-gray-200 rounded-lg focus:border-green-500 focus:ring-2 focus:ring-green-500/20 outline-none transition-all duration-300 hover:border-green-400 text-sm bg-gray-50"
        >
      </div>

      <!-- 操作按钮 -->
      <div class="flex gap-3">
        <button 
          @click="saveWebp"
          class="flex-1 px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-colors text-sm"
        >
          保存图片
        </button>
        <ImageUploader canvas-id="canvasPreview" />
        <button 
          @click="openSettings"
          class="px-4 py-2 bg-gray-100 text-gray-700 rounded-lg hover:bg-gray-200 transition-colors"
          title="设置"
        >
          <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"/>
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/>
          </svg>
        </button>
      </div>
    </div>

    <!-- 画布预览 -->
    <div class="relative w-full lg:flex-[2] overflow-hidden">
      <canvas 
        id="canvasPreview" 
        width="1000" 
        height="500" 
        @dragover.prevent="handleCanvasDragOver"
        @dragleave.prevent="handleCanvasDragLeave"
        @drop.prevent="handleCanvasDrop" 
        class="w-full h-auto rounded-lg shadow-md"
      ></canvas>
      <!-- 图标区高亮 -->
      <div
        v-if="dragHighlight === 'icon'"
        class="pointer-events-none absolute left-1/2 top-1/2"
        :style="{
          width: '200px',
          height: '200px',
          transform: 'translate(-50%, -50%)',
          border: '3px dashed #22c55e',
          borderRadius: '24px',
          boxSizing: 'border-box',
          zIndex: 10
        }"
      ></div>
      <!-- 背景区高亮 -->
      <div
        v-if="dragHighlight === 'bg'"
        class="pointer-events-none absolute inset-0"
        style="border: 3px dashed #22c55e; border-radius: 16px; box-sizing: border-box; z-index: 9;"
      ></div>
    </div>

    <!-- 设置模态框 -->
    <SettingsModal
      v-model="showSettings"
    />
  </main>
</template>

<script>
import { state, updatePreview, saveWebp, drawSquareImage, initialize } from '../assets/script.js';
import { defaultConfig } from '../config';
import ImageUploader from './ImageUploader.vue';
import SettingsModal from './SettingsModal.vue';

export default {
  components: {
    ImageUploader,
    SettingsModal
  },
  data() {
    return {
      state,
      defaultConfig,
      iconName: '',
      iconUrl: null,
      showSettings: false,
      dragHighlight: null
    };
  },
  mounted() {
    this.loadStyles();
    initialize();
    
    // Add click outside listener
    document.addEventListener('click', this.handleClickOutside);
  },
  unmounted() {
    // Remove click outside listener
    document.removeEventListener('click', this.handleClickOutside);
  },
  methods: {
    loadStyles() {
      defaultConfig.fontStyles.forEach(url => {
        const link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = url;
        document.head.appendChild(link);
      });
    },
    updatePreview,
    saveWebp,
    loadIcon() {
      if (this.iconName) {
        this.iconUrl = `https://api.iconify.design/${this.iconName}.svg`;
        this.selectIcon();
      } else {
        this.iconUrl = null;
        state.squareImageUrl = null;
      }
    },
    selectIcon() {
      if (this.iconUrl) {
        fetch(this.iconUrl)
          .then(response => response.blob())
          .then(blob => {
            const file = new File([blob], 'icon.svg', { type: 'image/svg+xml' });
            state.squareImageUrl = URL.createObjectURL(file);
            updatePreview('square', { target: { files: [file] } });
          })
          .catch(error => {
            console.error('加载图标时出错:', error);
            this.showSuccessPopup('加载图标时出错: ' + error.message, false);
          });
      }
    },
    drawSquareImage,
    getDropArea(event) {
      const canvas = event.target;
      const rect = canvas.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;
      const centerX = rect.width / 2;
      const centerY = rect.height / 2;
      const centerRadius = 100;
      const distanceToCenter = Math.sqrt(
        Math.pow(x - centerX, 2) + Math.pow(y - centerY, 2)
      );
      return distanceToCenter < centerRadius ? 'icon' : 'bg';
    },
    handleCanvasDragOver(event) {
      this.dragHighlight = this.getDropArea(event);
    },
    handleCanvasDragLeave() {
      this.dragHighlight = null;
    },
    handleCanvasDrop(event) {
      this.dragHighlight = null;
      const file = event.dataTransfer.files[0];
      if (!file || !file.type.startsWith('image/')) return;
      const area = this.getDropArea(event);
      this.updatePreview(area === 'icon' ? 'square' : 'bg', { target: { files: [file] } });
    },
    selectFont(fontValue) {
      state.selectedFont = fontValue;
      state.isFontMenuOpen = false;
      this.updatePreview('font', { target: { value: fontValue } });
    },
    handleClickOutside(event) {
      const dropdown = document.querySelector('.relative');
      if (dropdown && !dropdown.contains(event.target)) {
        state.isFontMenuOpen = false;
      }
    },
    openSettings() {
      this.showSettings = true;
    }
  }
};
</script>
