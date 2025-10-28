<template>
  <section class="image-gallery">
    <div class="simply-container">
      <div class="gallery-header">
        <h2 class="gallery-title">产品展示</h2>
        <p class="gallery-description">探索我们的AI工具和功能</p>
      </div>
      
      <div class="gallery-grid">
        <div class="gallery-item" v-for="(item, index) in galleryItems" :key="index">
          <div class="image-wrapper">
            <img :src="item.image" :alt="item.title" class="gallery-image" />
            <div class="image-overlay">
              <h3 class="image-title">{{ item.title }}</h3>
              <p class="image-description">{{ item.description }}</p>
            </div>
          </div>
        </div>
      </div>
      
      <div class="gallery-stats">
        <div class="stat-item">
          <div class="stat-number">100+</div>
          <div class="stat-label">AI工具</div>
        </div>
        <div class="stat-item">
          <div class="stat-number">50+</div>
          <div class="stat-label">AI模型</div>
        </div>
        <div class="stat-item">
          <div class="stat-number">5</div>
          <div class="stat-label">集成平台</div>
        </div>
        <div class="stat-item">
          <div class="stat-number">10K+</div>
          <div class="stat-label">用户信赖</div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const galleryItems = ref([
  {
    image: 'https://images.unsplash.com/photo-1677442136019-21780ecad995?w=500&h=300&fit=crop',
    title: 'AI聊天助手',
    description: '智能对话，提升工作效率'
  },
  {
    image: 'https://images.unsplash.com/photo-1677442136019-21780ecad995?w=500&h=300&fit=crop',
    title: '图像生成',
    description: '创意设计，一键生成'
  },
  {
    image: 'https://images.unsplash.com/photo-1677442136019-21780ecad995?w=500&h=300&fit=crop',
    title: '代码助手',
    description: '编程辅助，提高开发效率'
  },
  {
    image: 'https://images.unsplash.com/photo-1677442136019-21780ecad995?w=500&h=300&fit=crop',
    title: '文档分析',
    description: '智能解读，快速理解'
  },
  {
    image: 'https://images.unsplash.com/photo-1677442136019-21780ecad995?w=500&h=300&fit=crop',
    title: '翻译工具',
    description: '多语言支持，准确翻译'
  },
  {
    image: 'https://images.unsplash.com/photo-1677442136019-21780ecad995?w=500&h=300&fit=crop',
    title: '数据分析',
    description: '智能洞察，数据驱动'
  }
])
</script>

<style scoped>
.image-gallery {
  padding: 80px 0;
  background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
}

.gallery-header {
  text-align: center;
  margin-bottom: 60px;
}

.gallery-title {
  font-size: 2.5rem;
  font-weight: 800;
  color: #1a1a1a;
  margin-bottom: 1rem;
}

.gallery-description {
  font-size: 1.2rem;
  color: #6b7280;
  max-width: 600px;
  margin: 0 auto;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
  margin-bottom: 60px;
}

.gallery-item {
  position: relative;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.gallery-item:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.15);
}

.image-wrapper {
  position: relative;
  width: 100%;
  height: 250px;
}

.gallery-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.image-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(transparent, rgba(0, 0, 0, 0.8));
  padding: 30px 20px 20px;
  color: white;
  transform: translateY(100%);
  transition: transform 0.3s ease;
}

.gallery-item:hover .image-overlay {
  transform: translateY(0);
}

.image-title {
  font-size: 1.3rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.image-description {
  font-size: 0.9rem;
  opacity: 0.9;
  margin: 0;
}

.gallery-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 40px;
  text-align: center;
}

.stat-item {
  padding: 20px;
}

.stat-number {
  font-size: 3rem;
  font-weight: 800;
  color: #667eea;
  margin-bottom: 0.5rem;
}

.stat-label {
  font-size: 1.1rem;
  color: #6b7280;
  font-weight: 500;
}

@media (max-width: 768px) {
  .gallery-grid {
    grid-template-columns: 1fr;
    gap: 20px;
  }
  
  .gallery-title {
    font-size: 2rem;
  }
  
  .gallery-stats {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .stat-number {
    font-size: 2.5rem;
  }
}
</style>
