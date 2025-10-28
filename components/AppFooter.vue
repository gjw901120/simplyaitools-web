<template>
  <footer class="app-footer">
    <div class="simply-container">
      <div class="footer-content">
        <!-- Left Section - Company Info -->
        <div class="footer-section">
          <div class="logo">
            <i class="fas fa-robot"></i>
            <span>SimplyAI</span>
          </div>
          <p class="company-description">
            Your intelligent AI assistant for work, learning, and creativity.
            Powered by advanced artificial intelligence technology.
          </p>
          <div class="social-links">
            <a href="#" class="social-link"><i class="fab fa-twitter"></i></a>
            <a href="#" class="social-link"><i class="fab fa-linkedin"></i></a>
            <a href="#" class="social-link"><i class="fab fa-github"></i></a>
            <a href="#" class="social-link"><i class="fab fa-discord"></i></a>
          </div>
        </div>

        <!-- Middle Sections - Links -->
        <div class="footer-section">
          <h4>Applications</h4>
          <ul class="footer-links">
            <li><a href="#">Browser Extension</a></li>
            <li><a href="#">Desktop App</a></li>
            <li><a href="#">Mobile App</a></li>
            <li><a href="#">Monica Code</a></li>
            <li><a href="#">VS Code Extension</a></li>
          </ul>
        </div>

        <div class="footer-section">
          <h4>Learn More</h4>
          <ul class="footer-links">
            <li><a href="#">Learning Center</a></li>
            <li><a href="#">Help Center</a></li>
            <li><a href="#">Pricing</a></li>
            <li><a href="#">Monica Voice</a></li>
            <li><a href="#">Change Shortcuts</a></li>
          </ul>
        </div>

        <div class="footer-section">
          <h4>About</h4>
          <ul class="footer-links">
            <li><a href="#">Privacy Policy</a></li>
            <li><a href="#">Terms & Conditions</a></li>
            <li><a href="#">Affiliate Program</a></li>
            <li><a href="#">Careers</a></li>
            <li><a href="#">Contact Us</a></li>
          </ul>
        </div>

        <!-- Right Section - Platform Info -->
        <div class="footer-section">
          <h4>Platform Support</h4>
          <div class="platform-support">
            <div class="platform-group">
              <strong>Desktop</strong>
              <span>Windows & Mac</span>
            </div>
            <div class="platform-group">
              <strong>Mobile</strong>
              <span>Android & iOS</span>
            </div>
            <div class="platform-group">
              <strong>Browser</strong>
              <span>Chrome & Edge</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Bottom Section -->
      <div class="footer-bottom">
        <div class="footer-bottom-content">
          <div class="copyright">
            <p>&copy; 2025 BUTTERFLY EFFECT PTE. LTD. All rights reserved.</p>
            <p class="trademark">
              CHATGPT is a registered trademark of OPENAI OPCO, LLC.
            </p>
          </div>
          <div class="footer-actions">
            <select v-model="selectedLanguage" class="language-select">
              <option value="en">English</option>
              <option value="zh">中文</option>
              <option value="ja">日本語</option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup>
const selectedLanguage = ref('en')
</script>

<style scoped>
.app-footer {
  background: #f8fafc;
  color: #374151;
  padding: 60px 0 20px;
}

.footer-content {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr 1fr 1.5fr;
  gap: 40px;
  margin-bottom: 40px;
}

.footer-section h4 {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 1.5rem;
  color: #1f2937;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 1.5rem;
  font-weight: bold;
  color: #667eea;
  margin-bottom: 1rem;
}

.logo i {
  font-size: 1.8rem;
}

.company-description {
  color: #6b7280;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.social-links {
  display: flex;
  gap: 1rem;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  background: #e5e7eb;
  border-radius: 8px;
  color: #6b7280;
  text-decoration: none;
  transition: all 0.3s ease;
}

.social-link:hover {
  background: #667eea;
  color: white;
  transform: translateY(-2px);
}

.footer-links {
  list-style: none;
}

.footer-links li {
  margin-bottom: 0.75rem;
}

.footer-links a {
  color: #6b7280;
  text-decoration: none;
  transition: color 0.3s ease;
  font-size: 0.9rem;
}

.footer-links a:hover {
  color: #667eea;
}

.platform-support {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.platform-group {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.platform-group strong {
  color: #1f2937;
  font-size: 0.9rem;
}

.platform-group span {
  color: #6b7280;
  font-size: 0.85rem;
}

.footer-bottom {
  border-top: 1px solid #e5e7eb;
  padding-top: 20px;
}

.footer-bottom-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.copyright p {
  color: #6b7280;
  font-size: 0.85rem;
  margin: 0;
}

.trademark {
  font-size: 0.8rem !important;
  opacity: 0.7;
}

.language-select {
  background: #ffffff;
  color: #374151;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  padding: 0.5rem;
  font-size: 0.9rem;
}

@media (max-width: 1024px) {
  .footer-content {
    grid-template-columns: 1fr 1fr;
    gap: 30px;
  }
}

@media (max-width: 768px) {
  .footer-content {
    grid-template-columns: 1fr;
    gap: 30px;
  }

  .footer-bottom-content {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
}
</style>