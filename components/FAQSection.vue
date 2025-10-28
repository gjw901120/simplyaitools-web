<template>
  <section class="faq-section">
    <div class="simply-container">
      <div class="faq-header">
        <h2 class="faq-title">常见问题</h2>
      </div>
      
      <div class="faq-list">
        <div 
          v-for="(item, index) in faqItems" 
          :key="index"
          class="faq-item"
          :class="{ 'active': item.isOpen }"
        >
          <div class="faq-question" @click="toggleFAQ(index)">
            <h3 class="question-text">{{ item.question }}</h3>
            <div class="faq-icon">
              <i class="fas" :class="item.isOpen ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </div>
          </div>
          
          <div v-if="item.isOpen" class="faq-answer">
            <p>{{ item.answer }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

// FAQ数据
const faqItems = ref([
  {
    question: "Monica 是什么？",
    answer: "Monica 是一个由 ChatGPT API 提供支持的Chrome扩展，旨在成为您轻松聊天和撰写文案的个人 AI 助手。",
    isOpen: true
  },
  {
    question: "我该如何使用 Monica？",
    answer: "安装Monica扩展后，您可以在任何网页上使用Ctrl+M快捷键启动聊天界面，或者点击扩展图标开始对话。您可以询问任何问题、请求文案创作、翻译文本等。",
    isOpen: false
  },
  {
    question: "Monica 是免费的吗？",
    answer: "Monica提供免费版本，包含基础的AI对话功能。我们还提供付费的高级版本，包含更多高级功能和更高的使用限额。",
    isOpen: false
  },
  {
    question: "Monica 是如何工作的？",
    answer: "Monica 使用由 ChatGPT API 提供支持的先进人工智能技术，理解并回应您的聊天消息，以及根据提供的模板生成文案。此外，Monica 还可以翻译、改写和解释任何网页上的文本。",
    isOpen: false
  }
])

// 切换FAQ展开/收起状态
const toggleFAQ = (index) => {
  faqItems.value[index].isOpen = !faqItems.value[index].isOpen
}
</script>

<style scoped>
.faq-section {
  padding: 5rem 0;
  background: #ffffff;
  border-top: 1px solid #f3f4f6;
}

.faq-header {
  text-align: center;
  margin-bottom: 3rem;
}

.faq-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #1f2937;
  margin: 0;
  line-height: 1.2;
}

.faq-list {
  max-width: 800px;
  margin: 0 auto;
}

.faq-item {
  border-bottom: 1px solid #e5e7eb;
  transition: all 0.3s ease;
}

.faq-item:last-child {
  border-bottom: none;
}

.faq-question {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 0;
  cursor: pointer;
  transition: all 0.3s ease;
}

.faq-question:hover {
  background: #f9fafb;
  margin: 0 -2rem;
  padding-left: 2rem;
  padding-right: 2rem;
}

.question-text {
  font-size: 1.125rem;
  font-weight: 600;
  color: #1f2937;
  margin: 0;
  flex: 1;
  line-height: 1.5;
}

.faq-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2rem;
  height: 2rem;
  color: #6b7280;
  transition: all 0.3s ease;
}

.faq-icon i {
  font-size: 0.875rem;
}

.faq-item.active .faq-icon {
  color: #3b82f6;
  transform: rotate(180deg);
}

.faq-answer {
  padding: 0 0 1.5rem 0;
  animation: slideDown 0.3s ease-out;
}

.faq-answer p {
  font-size: 1rem;
  color: #4b5563;
  line-height: 1.6;
  margin: 0;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .faq-section {
    padding: 3rem 0;
  }
  
  .faq-title {
    font-size: 2rem;
  }
  
  .faq-question {
    padding: 1.25rem 0;
  }
  
  .faq-question:hover {
    margin: 0 -1rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }
  
  .question-text {
    font-size: 1rem;
  }
  
  .faq-answer {
    padding: 0 0 1.25rem 0;
  }
}

@media (max-width: 480px) {
  .faq-section {
    padding: 2rem 0;
  }
  
  .faq-title {
    font-size: 1.75rem;
  }
  
  .faq-header {
    margin-bottom: 2rem;
  }
}
</style>




