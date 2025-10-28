<template>
  <div class="runway-tool">
    <!-- 工具信息头部 -->
    <div class="tool-header">
      <div class="tool-avatar">
        <img src="/tools-logo/Runway.png" alt="Runway" />
      </div>
      <div class="tool-details">
        <h3>Runway</h3>
        <p>Utilize Runway to generate high-quality videos from text and images. Leverage Runway AI for seamless integration and immediately start creating your Runway masterpieces.</p>
      </div>
    </div>

    <!-- 主内容区域：左右布局 -->
    <div class="main-content">
      <!-- 左侧：参数配置面板 (1/3) -->
      <div class="config-panel">
        <div class="config-header">
          <h4>Video Generation Configuration</h4>
        </div>

        <form class="config-form" @submit.prevent="generateVideo">
          <!-- Prompt 输入 -->
          <div class="form-group">
            <label for="prompt">Video Description *</label>
            <textarea
              id="prompt"
              v-model="formData.prompt"
              placeholder="Descriptive text that guides AI video generation. Specify themes, actions, styles, and scenes. When used with images, describe how to animate or modify image content. Maximum 1800 characters."
              rows="4"
              maxlength="1800"
              required
            ></textarea>
            <div class="char-count">{{ formData.prompt.length }}/1800</div>
          </div>

          <!-- 时长选择 -->
          <div class="form-group">
            <label>Video Duration *</label>
            <div class="tab-group">
              <div class="tab-options">
                <button 
                  type="button"
                  class="tab-option"
                  :class="{ active: formData.duration === '5' }"
                  @click="formData.duration = '5'"
                >
                  <i class="fas fa-clock"></i>
                  5 seconds
                </button>
                <button 
                  type="button"
                  class="tab-option"
                  :class="{ active: formData.duration === '10' }"
                  @click="formData.duration = '10'"
                >
                  <i class="fas fa-clock"></i>
                  10 seconds
                </button>
              </div>
            </div>
          </div>

          <!-- 质量选择 -->
          <div class="form-group">
            <label>Video Quality *</label>
            <div class="tab-group">
              <div class="tab-options">
                <button 
                  type="button"
                  class="tab-option"
                  :class="{ active: formData.quality === '720p' }"
                  @click="formData.quality = '720p'"
                >
                  <i class="fas fa-hd-video"></i>
                  720p
                </button>
                <button 
                  type="button"
                  class="tab-option"
                  :class="{ active: formData.quality === '1080p' }"
                  @click="formData.quality = '1080p'"
                  :disabled="formData.duration === '10'"
                >
                  <i class="fas fa-video"></i>
                  1080p
                </button>
              </div>
            </div>
            <div v-if="formData.duration === '10'" class="form-help">
              10-second videos do not support 1080p resolution
            </div>
          </div>

          <!-- 参考图像上传 -->
          <div class="form-group">
            <label class="form-label">Reference Image</label>
            <UploadImage
              input-id="runway-image-upload"
              label=""
              upload-icon="fas fa-cloud-upload-alt"
              upload-text="Click to upload image"
              upload-hint="Supports JPG/PNG format, max 10MB"
              additional-hint="Optional reference image as the basis for the video. If provided, AI will create a video that animates or extends this image."
              theme-color="#3b82f6"
              :max-files="1"
              :max-file-size="10 * 1024 * 1024"
              accept="image/*"
              :multiple="false"
              @update:files="handleImageUpdate"
            />
          </div>

          <!-- 宽高比选择 -->
          <div class="form-group" v-if="!formData.imageFile">
            <label>Video Aspect Ratio *</label>
            <div class="select-wrapper">
              <select v-model="formData.aspectRatio" required>
                <option value="16:9">16:9 (Landscape)</option>
                <option value="4:3">4:3 (Standard)</option>
                <option value="1:1">1:1 (Square)</option>
                <option value="3:4">3:4 (Portrait)</option>
                <option value="9:16">9:16 (Mobile)</option>
              </select>
              <i class="fas fa-chevron-down"></i>
            </div>
          </div>

          <!-- 水印设置 -->
          <div class="form-group">
            <label for="waterMark">Watermark Text</label>
            <input
              id="waterMark"
              v-model="formData.waterMark"
              type="text"
              placeholder="kie.ai"
            />
            <div class="form-help">
              Video watermark text content. Empty string means no watermark, non-empty string will display the specified watermark text in the bottom-right corner of the video.
            </div>
          </div>

          <div class="form-actions">
            <button
              type="submit"
              class="btn-primary"
              :disabled="!formData.prompt || isGenerating"
            >
              <i v-if="isGenerating" class="fas fa-spinner fa-spin"></i>
              <i v-else class="fas fa-video"></i>
              {{ isGenerating ? 'Generating...' : 'Generate Video' }}
              <span class="price-tag">($2.8)</span>
            </button>
          </div>
        </form>
        
        <!-- 使用提示 -->
        <div class="tips-panel">
          <h4>💡 Usage Tips</h4>
          <ul class="tips-list">
            <li><strong>Detailed Description:</strong> Include details like scenes, actions, camera movement, lighting, etc.</li>
            <li><strong>Cinematic Terms:</strong> Use professional terms like "close-up", "wide shot", "tracking shot"</li>
            <li><strong>Style Specification:</strong> Such as "sci-fi style", "romantic comedy", "action film"</li>
            <li><strong>Duration Limit:</strong> 10-second videos do not support 1080p resolution</li>
            <li><strong>Reference Image:</strong> Providing an image URL can add animation effects to existing images</li>
          </ul>
        </div>
      </div>

      <!-- 右侧：视频展示区域 (2/3) -->
      <div class="result-panel">
        <div class="video-header">
          <h4>Video Preview</h4>
          <div class="video-actions" v-if="generatedVideos.length > 0">
            <button @click="clearResults" class="btn-secondary">
              <i class="fas fa-trash"></i> Clear
            </button>
          </div>
        </div>
        
        <!-- 视频展示区域 -->
        <div class="video-container">
          <div v-if="generatedVideos.length > 0" class="video-showcase">
            <div
              v-for="(video, index) in generatedVideos"
              :key="index"
              class="video-showcase-item"
            >
              <div class="video-player">
                <video controls :src="video.url" :poster="video.thumbnail">
                  Your browser does not support video playback
                </video>
              </div>
              <div class="video-details">
                <div class="video-meta">
                  <span class="video-duration">{{ video.duration }}</span>
                  <span class="video-resolution">{{ video.resolution }}</span>
                  <span class="video-ratio">{{ video.aspectRatio }}</span>
                </div>
                <div class="video-actions">
                  <button @click="downloadVideo(video)" class="btn-icon" title="Download">
                    <i class="fas fa-download"></i>
                  </button>
                  <button @click="shareVideo(video)" class="btn-icon" title="Share">
                    <i class="fas fa-share"></i>
                  </button>
                </div>
              </div>
            </div>
          </div>
          
          <!-- 空状态 -->
          <div v-else class="empty-state">
            <div class="empty-icon">
              <i class="fas fa-video"></i>
            </div>
            <h3>Waiting for Video Generation</h3>
            <p>Configure parameters and click "Generate Video" button to start creating your AI video</p>
            <div class="empty-features">
              <div class="feature-item">
                <i class="fas fa-magic"></i>
                <span>Text to Video</span>
              </div>
              <div class="feature-item">
                <i class="fas fa-image"></i>
                <span>Image to Video</span>
              </div>
              <div class="feature-item">
                <i class="fas fa-palette"></i>
                <span>Multiple Aspect Ratios</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch, inject } from 'vue'
import { useRouter } from 'vue-router'
import UploadImage from './common/UploadImage.vue'

const router = useRouter()

// 注入父组件的函数
const addToUsageHistory = inject('addToUsageHistory')

// Form data
const formData = reactive({
  prompt: '',
  duration: '5',
  quality: '720p',
  imageFile: null,
  aspectRatio: '16:9',
  waterMark: ''
})

const isGenerating = ref(false)
const generatedVideos = ref([])

// Watch for duration and quality conflicts
watch([() => formData.duration, () => formData.quality], ([duration, quality]) => {
  if (duration === '10' && quality === '1080p') {
    formData.quality = '720p'
  }
})

// Handle image upload
const handleImageUpdate = (files) => {
  if (files && files.length > 0) {
    formData.imageFile = files[0]
  } else {
    formData.imageFile = null
  }
}

// Methods
const generateVideo = async () => {
  if (!formData.prompt) return
  
  // 添加到使用历史
  if (addToUsageHistory) {
    addToUsageHistory('Runway')
  }
  
  isGenerating.value = true
  
  try {
    // 模拟API调用
    await new Promise(resolve => setTimeout(resolve, 3000))
    
    // 模拟生成结果
    const newVideo = {
      id: Date.now(),
      url: 'https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4',
      thumbnail: 'https://via.placeholder.com/320x180/3b82f6/ffffff?text=Runway+Video',
      duration: `${formData.duration}秒`,
      resolution: formData.quality,
      aspectRatio: formData.aspectRatio,
      prompt: formData.prompt,
      createdAt: new Date().toISOString()
    }
    
    generatedVideos.value.unshift(newVideo)
    
    // 重置表单
    formData.prompt = ''
    formData.imageFile = null
    formData.waterMark = ''
    
  } catch (error) {
    console.error('生成视频失败:', error)
  } finally {
    isGenerating.value = false
  }
}

const downloadVideo = (video) => {
  // 实现下载逻辑
  console.log('下载视频:', video)
}

const shareVideo = (video) => {
  // 实现分享逻辑
  console.log('分享视频:', video)
}

const clearResults = () => {
  generatedVideos.value = []
}
</script>

<style scoped>
.runway-tool {
  width: 100%;
  height: 100%;
  padding: 20px;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.main-content {
  display: flex;
  flex: 1;
  min-height: 0;
  gap: 20px;
}

.tool-header {
  display: flex;
  align-items: center;
  padding-bottom: 20px;
  border-bottom: 1px solid #e2e8f0;
  margin-bottom: 20px;
}

.tool-avatar {
  width: 48px;
  height: 48px;
  border-radius: 8px;
  overflow: hidden;
  margin-right: 16px;
}

.tool-avatar img {
  width: 48px;
  height: 48px;
  object-fit: cover;
}

.tool-details h3 {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
  color: #1f2937;
}

.tool-details p {
  margin: 0;
  font-size: 14px;
  color: #6b7280;
}

/* 配置面板 */
.config-panel {
  width: 35%;
  background: #f8fafc;
  border-radius: 12px;
  padding: 20px;
  border: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.config-header {
  padding: 0 0 20px 0;
  border-bottom: 1px solid #e2e8f0;
  margin-bottom: 20px;
}

.config-header h4 {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: #1e293b;
}

.config-form {
  flex: 1;
  overflow-y: auto;
}

.cost-info {
  display: flex;
  align-items: center;
  gap: 8px;
}

.cost-badge {
  background: linear-gradient(135deg, #3b82f6, #1d4ed8);
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
}

.cost-label {
  font-size: 12px;
  color: #64748b;
}

/* 表单样式 */
.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  font-size: 14px;
  font-weight: 500;
  color: #374151;
  margin-bottom: 8px;
}

.form-group input,
.form-group textarea,
.form-group select {
  width: 100%;
  padding: 12px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 14px;
  transition: border-color 0.2s ease;
  box-sizing: border-box;
}

.form-group input:focus,
.form-group textarea:focus,
.form-group select:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.form-group textarea {
  resize: vertical;
  min-height: 80px;
}

.char-count {
  text-align: right;
  font-size: 12px;
  color: #64748b;
  margin-top: 4px;
}

/* 设置选项卡样式 */
.settings-tabs {
  display: flex;
  gap: 20px;
}

.tab-group {
  flex: 1;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  overflow: hidden;
  background: white;
}

.tab-header {
  background: #f8fafc;
  padding: 12px 16px;
  border-bottom: 1px solid #e5e7eb;
}

.tab-title {
  font-size: 14px;
  font-weight: 600;
  color: #374151;
}

.tab-content {
  padding: 16px;
}

.tab-options {
  display: flex;
  gap: 8px;
}

.tab-option {
  flex: 1;
  padding: 12px 16px;
  border: 1px solid #e5e7eb;
  background: white;
  color: #64748b;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  font-weight: 500;
}

.tab-option:hover:not(:disabled) {
  border-color: #3b82f6;
  background: #f8fafc;
  color: #3b82f6;
}

.tab-option.active {
  background: #3b82f6;
  color: white;
  border-color: #3b82f6;
}

.tab-option:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  background: #f9fafb;
}

.tab-warning {
  margin-top: 12px;
  padding: 8px 12px;
  background: #fef3c7;
  border: 1px solid #f59e0b;
  border-radius: 6px;
  color: #92400e;
  font-size: 12px;
  display: flex;
  align-items: center;
  gap: 6px;
}

.tab-warning i {
  color: #f59e0b;
}

.radio-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.radio-item {
  display: flex;
  align-items: center;
  cursor: pointer;
  padding: 12px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  transition: all 0.2s ease;
}

.radio-item:hover {
  border-color: #3b82f6;
  background: #f8fafc;
}

.radio-item input[type="radio"] {
  margin-right: 12px;
  width: auto;
}

.radio-label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  color: #374151;
}

.radio-label small {
  color: #ef4444;
  font-size: 12px;
}

.select-wrapper {
  position: relative;
}

.select-wrapper select {
  appearance: none;
  padding-right: 40px;
}

.select-wrapper i {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #64748b;
  pointer-events: none;
}

.form-help {
  font-size: 12px;
  color: #64748b;
  margin-top: 4px;
  line-height: 1.4;
}

.form-actions {
  margin-top: 24px;
  padding-bottom: 20px;
}

.btn-primary {
  width: 100%;
  padding: 16px;
  background: linear-gradient(135deg, #3b82f6, #1d4ed8);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  transition: all 0.3s ease;
}

.btn-primary:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(59, 130, 246, 0.3);
}

.btn-primary:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

.price-tag {
  font-size: 12px;
  opacity: 0.8;
  margin-left: 4px;
}

/* 结果面板 */
.result-panel {
  width: 65%;
  background: #ffffff;
  border-radius: 12px;
  padding: 20px;
  border: 1px solid #e2e8f0;
  min-height: 400px;
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.video-header {
  padding: 20px;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.video-header h4 {
  font-size: 16px;
  font-weight: 600;
  color: #1e293b;
  margin: 0;
}

.video-container {
  flex: 1;
  padding: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 80vh;
}

/* 视频展示区域 */
.video-showcase {
  width: 100%;
  max-width: 100%;
}

.video-showcase-item {
  width: 100%;
  background: #f8fafc;
  border-radius: 8px;
  overflow: hidden;
  margin-bottom: 16px;
}

.video-player {
  width: 100%;
  aspect-ratio: 16/9;
  background: #000;
}

.video-player video {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.video-details {
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.video-meta {
  display: flex;
  gap: 12px;
  font-size: 12px;
  color: #64748b;
}

.video-actions {
  display: flex;
  gap: 8px;
}

.btn-secondary,
.btn-icon {
  padding: 8px 12px;
  border: 1px solid #e2e8f0;
  background: white;
  color: #64748b;
  border-radius: 6px;
  cursor: pointer;
  font-size: 12px;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  gap: 6px;
}

.btn-secondary:hover,
.btn-icon:hover {
  background: #e2e8f0;
  color: #475569;
}

/* 空状态 */
.empty-state {
  text-align: center;
  color: #64748b;
  max-width: 500px;
  min-height: 60vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.empty-icon {
  font-size: 72px;
  color: #cbd5e1;
  margin-bottom: 24px;
}

.empty-state h3 {
  font-size: 28px;
  font-weight: 600;
  color: #374151;
  margin: 0 0 12px 0;
}

.empty-state p {
  font-size: 16px;
  margin: 0 0 32px 0;
  line-height: 1.6;
}

.empty-features {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 12px;
  justify-content: center;
  font-size: 16px;
  color: #64748b;
  padding: 8px 16px;
  background: rgba(59, 130, 246, 0.1);
  border-radius: 8px;
  transition: all 0.3s ease;
}

.feature-item:hover {
  background: rgba(59, 130, 246, 0.15);
  transform: translateY(-2px);
}

.feature-item i {
  color: #3b82f6;
  width: 20px;
  font-size: 18px;
}

/* 提示面板 */
.tips-panel {
  background: white;
}

.tips-panel h4 {
  font-size: 14px;
  font-weight: 600;
  color: #1e293b;
  margin: 0 0 12px 0;
}

.tips-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.tips-list li {
  font-size: 12px;
  color: #64748b;
  margin-bottom: 8px;
  line-height: 1.5;
}

.tips-list strong {
  color: #374151;
}

/* 响应式设计 */
@media (max-width: 1024px) {
  .main-content {
    flex-direction: column;
  }
  
  .config-panel,
  .result-panel {
    width: 100%;
  }
}

@media (max-width: 768px) {
  .runway-tool {
    padding: 16px;
  }
  
  .tool-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }
  
  .config-header {
    padding: 0 0 16px 0;
  }
  
  .settings-tabs {
    flex-direction: column;
    gap: 16px;
  }
  
  .tab-options {
    flex-direction: column;
  }
  
  .tab-option {
    width: 100%;
  }
  
  .empty-features {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .feature-item {
    flex: 1;
    min-width: 120px;
  }
}
</style>
