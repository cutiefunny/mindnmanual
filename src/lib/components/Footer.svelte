<footer class="footer">
  <div class="container footer-container">
    <!-- Footer top side -->
    <div class="footer-top">
      <div class="footer-logo">
        <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/c6311f1b65a9e.png" alt="마인드앤매뉴얼 로고" />
      </div>
      
      <div class="footer-links">
        <a href="/?mode=policy" class="footer-link">이용약관</a>
        <span class="link-divider">|</span>
        <a href="#privacy" class="footer-link active">개인정보처리방침</a>
      </div>
    </div>

    <div class="footer-divider"></div>

    <!-- Footer main content -->
    <div class="footer-bottom">
      <div class="footer-company-info">
        <p class="company-name">(주)마인드앤매뉴얼</p>
        <div class="info-grid">
          <p><span>대표자 :</span> 박진우</p>
          <p><span>사업자등록번호 :</span> 355-81-01136</p>
          <p><span>주소 :</span> 서울특별시 마포구 독막로6길 8, 2층(합정동)</p>
          <p><span>이메일 :</span> mind_manual@naver.com</p>
          <p><span>TEL :</span> 1660-3707</p>
          <p><span>FAX :</span> 02-6954-1135</p>
        </div>
      </div>

      <div class="footer-social-copyright">
        <div class="social-links">
          <!-- KakaoTalk -->
          <a href="http://pf.kakao.com/_hyxfuG" target="_blank" rel="noopener noreferrer" class="social-icon" aria-label="Kakao Talk Link">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 3c-4.97 0-9 3.185-9 7.115 0 2.557 1.707 4.8 4.27 6.054-.188.702-.68 2.531-.777 2.947-.118.5-.422 1.67.202 1.25.437-.296 2.512-1.748 3.518-2.446C10.74 18.068 11.36 18.1 12 18.1c4.97 0 9-3.186 9-7.115C21 6.185 16.97 3 12 3z"/>
            </svg>
          </a>
          <!-- Instagram -->
          <a href="https://www.instagram.com/mind_manual/" target="_blank" rel="noopener noreferrer" class="social-icon" aria-label="Instagram Link">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="2" width="20" height="20" rx="5" ry="5"/>
              <path d="M16 11.37A4 4 0 1112.63 8 4 4 0 0116 11.37zM17.5 6.5h.01"/>
            </svg>
          </a>
          <!-- Naver Blog -->
          <a href="https://blog.naver.com/mind_manual" target="_blank" rel="noopener noreferrer" class="social-icon blog" aria-label="Naver Blog Link">
            <span class="blog-text">B</span>
          </a>
        </div>
        
        <p class="copyright">
          COPYRIGHT ⓒ 2023 (주)마인드앤매뉴얼. All Rights Reserved.
        </p>
      </div>
    </div>
  </div>
</footer>

<style>
  .footer {
    background-color: #1a1a1a;
    color: #a0a0a0;
    padding: 60px 0;
    font-size: 13px;
  }

  .footer-container {
    display: flex;
    flex-direction: column;
    gap: 30px;
  }

  .footer-top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 20px;
  }

  .footer-logo img {
    height: 36px;
    width: auto;
    filter: brightness(0) invert(1);
    opacity: 0.8;
  }

  .footer-links {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .footer-link {
    color: #a0a0a0;
    font-weight: 500;
  }

  .footer-link:hover, .footer-link.active {
    color: #ffffff;
  }

  .link-divider {
    color: #444444;
  }

  .footer-divider {
    height: 1px;
    background-color: #2a2a2a;
    width: 100%;
  }

  .footer-bottom {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    flex-wrap: wrap;
    gap: 40px;
  }

  .footer-company-info {
    max-width: 650px;
  }

  .company-name {
    font-size: 15px;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 15px;
  }

  .info-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px 24px;
    line-height: 1.6;
  }

  .info-grid p span {
    color: #666666;
    margin-right: 4px;
  }

  .footer-social-copyright {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 20px;
  }

  .social-links {
    display: flex;
    gap: 12px;
  }

  .social-icon {
    width: 38px;
    height: 38px;
    border-radius: 50%;
    background-color: #2a2a2a;
    color: #a0a0a0;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all var(--transition-speed);
  }

  .social-icon:hover {
    background-color: var(--primary);
    color: #ffffff;
    transform: translateY(-2px);
  }

  .social-icon.blog {
    font-weight: 900;
    font-family: sans-serif;
  }

  .blog-text {
    font-size: 16px;
  }

  .copyright {
    font-size: 11px;
    color: #666666;
  }

  /* Responsive screen */
  @media (max-width: 768px) {
    .footer-bottom {
      flex-direction: column;
      align-items: flex-start;
      gap: 30px;
    }

    .info-grid {
      grid-template-columns: 1fr;
      gap: 8px;
    }

    .footer-social-copyright {
      align-items: flex-start;
    }
  }
</style>
