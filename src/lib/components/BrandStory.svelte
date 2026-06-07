<section id="brand" class="brand-section">
  <!-- Dark overlay to ensure contrast -->
  <div class="brand-overlay"></div>
  
  <div class="container brand-content">
    <div class="brand-logo-wrapper animate-fade-in">
      <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/a6cbfec30bc86.png" alt="Mind & Manual Logo" class="brand-logo-white" />
    </div>
    
    <div class="brand-divider"></div>
    
    <div class="brand-text-wrapper animate-slide-up">
      <h2 class="brand-statement">
        우리는 누군가를 돕겠다는 적극적인 <span class="highlight">마음(mind)</span>과<br class="desktop-only" />
        정확한 매뉴얼에 따른 <span class="highlight">행동(manual)</span>은<br />
        소중한 생명을 살릴 수 있는 기적을 만든다고 믿습니다.
      </h2>
      
      <p class="brand-paragraph">
        마인드앤매뉴얼은 안전하고 건강한 개인과 지역사회를 위하여 전문적인 보건교육서비스를 제공합니다.<br />
        관련 전문기관들과 연대하여 콘텐츠를 개발하고, 다양한 지역사회 공헌활동을 전개하는 등<br />
        창의적이고 선진적인 방식으로 브랜드를 키워나가고 있습니다.
      </p>
    </div>
  </div>
</section>

<style>
  .brand-section {
    position: relative;
    width: 100%;
    min-height: 560px;
    padding: 100px 0;
    background-image: url('https://cdn.imweb.me/thumbnail/20230809/6dc1cebf3b4f9.png');
    background-size: cover;
    background-position: center;
    background-attachment: fixed; /* Parallax effect */
    display: flex;
    align-items: center;
    justify-content: center;
  }

  /* Fallback for mobile devices that do not support fixed attachment */
  @media (max-width: 768px) {
    .brand-section {
      background-attachment: scroll;
      padding: 80px 0;
      min-height: auto;
    }
  }

  .brand-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    z-index: 1;
  }

  .brand-content {
    position: relative;
    z-index: 2;
    text-align: center;
    color: #ffffff;
    max-width: 900px;
  }

  .brand-logo-wrapper {
    margin-bottom: 30px;
    display: flex;
    justify-content: center;
  }

  .brand-logo-white {
    width: 180px;
    height: auto;
  }

  .brand-divider {
    width: 40px;
    height: 2px;
    background-color: var(--primary);
    margin: 0 auto 35px;
  }

  .brand-statement {
    font-size: 26px;
    font-weight: 600;
    line-height: 1.6;
    margin-bottom: 30px;
    word-break: keep-all;
  }

  .brand-statement .highlight {
    color: var(--primary);
    font-weight: 700;
  }

  .brand-paragraph {
    font-size: 16px;
    line-height: 1.8;
    color: rgba(255, 255, 255, 0.75);
    font-weight: 300;
    word-break: keep-all;
  }

  /* Responsive styling */
  @media (max-width: 768px) {
    .brand-statement {
      font-size: 18px;
    }

    .brand-paragraph {
      font-size: 14px;
    }

    .brand-logo-white {
      width: 140px;
    }

    .desktop-only {
      display: none;
    }
  }
</style>
