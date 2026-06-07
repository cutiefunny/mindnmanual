<script>
  // Gallery images list
  const galleryItems = [
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/ddcfd24cb198f.jpg', full: 'https://cdn.imweb.me/thumbnail/20230814/57cb2bf850093.jpg' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/5a48e1f3a4de8.jpg', full: 'https://cdn.imweb.me/thumbnail/20230814/803ac0a2db680.jpg' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/caa6c1597ec0b.jpg', full: 'https://cdn.imweb.me/thumbnail/20230814/86310c3e43a66.jpg' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230905/e4003af1e8a3f.jpg', full: 'https://cdn.imweb.me/thumbnail/20230905/deea7e2886c72.jpg' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/0a0b23b8866ac.jpg', full: 'https://cdn.imweb.me/thumbnail/20230814/08bfb93409f8b.jpg' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/868403fb9f6f3.png', full: 'https://cdn.imweb.me/thumbnail/20230814/4c4cd33544241.png' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/0d67ef165fe85.png', full: 'https://cdn.imweb.me/thumbnail/20230814/bb4c563295ed3.png' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/9cd2dacd19b72.jpg', full: 'https://cdn.imweb.me/thumbnail/20230814/e65f41687c185.jpg' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/b7816635c089a.png', full: 'https://cdn.imweb.me/thumbnail/20230814/886da6ee453ab.png' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/9126e95b4f9e4.jpg', full: 'https://cdn.imweb.me/thumbnail/20230814/8a7790b0abc32.jpg' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/1a52a9f03a332.png', full: 'https://cdn.imweb.me/thumbnail/20230814/f6c28461bcdc0.png' },
    { thumb: 'https://cdn.imweb.me/thumbnail/20230814/1fe6aae909c1f.jpg', full: 'https://cdn.imweb.me/thumbnail/20230814/cc18c69340deb.jpg' }
  ];

  // Client logos list
  const clients = [
    'https://cdn.imweb.me/thumbnail/20230816/6e11fd1031354.png',
    'https://cdn.imweb.me/thumbnail/20230816/50d72567a4a05.png',
    'https://cdn.imweb.me/thumbnail/20230816/1126d9ef25e47.png',
    'https://cdn.imweb.me/thumbnail/20230816/733363d5201f3.png',
    'https://cdn.imweb.me/thumbnail/20230816/1854ed30d5f85.png',
    'https://cdn.imweb.me/thumbnail/20230816/09f1856335942.png',
    'https://cdn.imweb.me/thumbnail/20230816/44b8105b6dda2.png',
    'https://cdn.imweb.me/thumbnail/20230816/e6201057e3b2a.png'
  ];

  // Partner logos list
  const partners = [
    'https://cdn.imweb.me/thumbnail/20230816/cdc431649f4f2.png',
    'https://cdn.imweb.me/thumbnail/20230816/c1f1c7ff37fab.png',
    'https://cdn.imweb.me/thumbnail/20230816/b6ff7d2535693.png',
    'https://cdn.imweb.me/thumbnail/20230816/32ce000537f2a.png'
  ];

  // Media appearances list
  const media = [
    'https://cdn.imweb.me/thumbnail/20230816/e1ba6f0256d94.png',
    'https://cdn.imweb.me/thumbnail/20230816/7daf8ff92f551.png',
    'https://cdn.imweb.me/thumbnail/20230816/1a7e5a8a51496.png'
  ];

  let displayCount = $state(6);
  let lightboxImage = $state(null);
  let activeTab = $state('clients'); // clients | partners | csr | media

  function showMoreImages() {
    if (displayCount < galleryItems.length) {
      displayCount += 6;
    } else {
      displayCount = 6;
    }
  }

  function openLightbox(imgUrl) {
    lightboxImage = imgUrl;
  }

  function closeLightbox() {
    lightboxImage = null;
  }

  function selectTab(tab) {
    activeTab = tab;
  }
</script>

<section id="activities" class="activities-section py-section">
  <div class="container">
    <div class="section-header animate-slide-up">
      <span class="sub-title">Major Activities</span>
      <h2 class="title">주요 활동</h2>
      <p class="desc-text">생명을 배우는 기적의 순간들과 마인드앤매뉴얼의 파트너들을 소개합니다.</p>
    </div>

    <div class="activities-split">
      <!-- Left: Dynamic Image Gallery -->
      <div class="gallery-wrapper animate-slide-up">
        <h3 class="side-title">교육 현장 갤러리</h3>
        <div class="gallery-grid">
          {#each galleryItems.slice(0, displayCount) as item}
            <button 
              class="gallery-item-card" 
              onclick={() => openLightbox(item.full)}
              aria-label="교육 현장 이미지 확대 보기"
            >
              <img src={item.thumb} alt="교육 현장 이미지" loading="lazy" />
              <div class="gallery-hover-overlay">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <circle cx="11" cy="11" r="8"/>
                  <path d="M21 21l-4.35-4.35M11 8v6M8 11h6"/>
                </svg>
              </div>
            </button>
          {/each}
        </div>
        
        <div class="text-center show-more-wrapper">
          <button class="btn-outline" onclick={showMoreImages}>
            {displayCount >= galleryItems.length ? '접기' : '더보기'}
          </button>
        </div>
      </div>

      <!-- Right: Collapsible panels and directories -->
      <div class="partners-wrapper animate-slide-up" style="animation-delay: 0.2s">
        <!-- Tab Navigation Buttons -->
        <div class="tab-nav">
          <button 
            class="tab-btn {activeTab === 'clients' ? 'active' : ''}" 
            onclick={() => selectTab('clients')}
          >
            주요 고객사
          </button>
          <button 
            class="tab-btn {activeTab === 'partners' ? 'active' : ''}" 
            onclick={() => selectTab('partners')}
          >
            파트너사
          </button>
          <button 
            class="tab-btn {activeTab === 'csr' ? 'active' : ''}" 
            onclick={() => selectTab('csr')}
          >
            사회공헌활동
          </button>
          <button 
            class="tab-btn {activeTab === 'media' ? 'active' : ''}" 
            onclick={() => selectTab('media')}
          >
            미디어
          </button>
        </div>

        <!-- Tab Contents -->
        <div class="tab-content-box">
          {#if activeTab === 'clients'}
            <div class="logo-grid animate-fade-in">
              {#each clients as logo}
                <div class="logo-card">
                  <img src={logo} alt="고객사 로고" />
                </div>
              {/each}
            </div>
          {:else if activeTab === 'partners'}
            <div class="logo-grid animate-fade-in">
              {#each partners as logo}
                <div class="logo-card">
                  <img src={logo} alt="파트너사 로고" />
                </div>
              {/each}
            </div>
          {:else if activeTab === 'csr'}
            <div class="csr-content-card animate-fade-in">
              <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" class="heart-icon">
                <path d="M20.84 4.61a5.5 5.5 0 00-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 00-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 000-7.78z"/>
              </svg>
              <h4>소방재난본부 '서울안전한마당' 참가</h4>
              <p>마인드앤매뉴얼은 서울 소방재난본부가 개최하는 서울안전한마당에 공식 참가하여 시민들을 대상으로 스마트 CPR 및 심폐소생술 교육과 체험 부스를 운영하였습니다.</p>
              
              <div class="csr-divider"></div>
              
              <h4>한국심장재단 '한 걸음 더 걷기 대회' 지원</h4>
              <p>한국심장재단이 주최하는 심장병 환우 돕기 걷기 대회에 참가하여 시민 교육 부스를 운영하고 안전 인력과 실습 교구재를 지원하는 등 공헌활동을 활발히 실천하고 있습니다.</p>
            </div>
          {:else if activeTab === 'media'}
            <div class="logo-grid animate-fade-in">
              {#each media as logo}
                <div class="logo-card">
                  <img src={logo} alt="미디어 노출 로고" />
                </div>
              {/each}
            </div>
          {/if}
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Lightbox Modal -->
{#if lightboxImage}
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <div class="lightbox-overlay" onclick={closeLightbox}>
    <button class="lightbox-close" onclick={closeLightbox} aria-label="Close image popup">
      <svg width="30" height="30" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M18 6L6 18M6 6l12 12" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
    <div onclick={(e) => e.stopPropagation()} class="lightbox-img-wrapper">
      <img src={lightboxImage} alt="확대된 이미지" class="lightbox-img" />
    </div>
  </div>
{/if}

<style>
  .activities-section {
    background-color: var(--bg-white);
  }

  .section-header {
    margin-bottom: 50px;
    text-align: center;
  }

  .sub-title {
    font-size: 13px;
    font-weight: 600;
    color: var(--primary);
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 8px;
    display: inline-block;
  }

  .title {
    font-size: 36px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 12px;
  }

  .desc-text {
    font-size: 16px;
    color: var(--text-muted);
  }

  .activities-split {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 48px;
  }

  .side-title {
    font-size: 20px;
    font-weight: 700;
    margin-bottom: 24px;
    color: var(--dark);
  }

  /* Left column: Grid Gallery */
  .gallery-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }

  .gallery-item-card {
    position: relative;
    border-radius: 12px;
    overflow: hidden;
    aspect-ratio: 4/3;
    cursor: pointer;
    background-color: var(--bg-light);
    border: 1px solid var(--border-color);
    padding: 0;
    width: 100%;
    display: block;
  }

  .gallery-item-card img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform var(--transition-speed);
  }

  .gallery-item-card:hover img {
    transform: scale(1.06);
  }

  .gallery-hover-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(255, 121, 0, 0.4);
    opacity: 0;
    transition: opacity var(--transition-speed);
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffffff;
  }

  .gallery-item-card:hover .gallery-hover-overlay {
    opacity: 1;
  }

  .show-more-wrapper {
    margin-top: 24px;
  }

  .btn-outline {
    background-color: transparent;
    border: 1px solid #cccccc;
    color: var(--text);
    padding: 10px 28px;
    border-radius: 30px;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    transition: all var(--transition-speed);
  }

  .btn-outline:hover {
    border-color: var(--primary);
    color: var(--primary);
    background-color: rgba(255, 121, 0, 0.05);
  }

  /* Right column: Tabs & Info directory */
  .tab-nav {
    display: flex;
    border-bottom: 2px solid var(--border-color);
    margin-bottom: 24px;
    overflow-x: auto;
    scrollbar-width: none;
  }

  .tab-nav::-webkit-scrollbar {
    display: none;
  }

  .tab-btn {
    background: none;
    border: none;
    padding: 12px 18px;
    font-size: 15px;
    font-weight: 600;
    color: var(--text-muted);
    cursor: pointer;
    position: relative;
    white-space: nowrap;
    transition: color var(--transition-speed);
  }

  .tab-btn.active {
    color: var(--primary);
  }

  .tab-btn.active::after {
    content: '';
    position: absolute;
    bottom: -2px;
    left: 0;
    width: 100%;
    height: 2px;
    background-color: var(--primary);
  }

  .logo-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
  }

  .logo-card {
    background-color: var(--bg-light);
    border: 1px solid var(--border-color);
    border-radius: 12px;
    padding: 12px;
    aspect-ratio: 16/9;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform var(--transition-speed);
  }

  .logo-card:hover {
    transform: translateY(-3px);
    border-color: var(--primary);
  }

  .logo-card img {
    max-width: 90%;
    max-height: 90%;
    object-fit: contain;
  }

  .csr-content-card {
    background-color: var(--bg-light);
    border: 1px solid var(--border-color);
    border-radius: 20px;
    padding: 30px;
    color: var(--text);
  }

  .heart-icon {
    color: var(--primary);
    margin-bottom: 15px;
  }

  .csr-content-card h4 {
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 8px;
    color: var(--dark);
  }

  .csr-content-card p {
    font-size: 14px;
    line-height: 1.6;
    color: var(--text-muted);
    margin-bottom: 20px;
  }

  .csr-divider {
    height: 1px;
    background-color: #e5e5e5;
    margin: 20px 0;
  }

  /* Lightbox popup styles */
  .lightbox-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.9);
    backdrop-filter: blur(8px);
    z-index: 1100;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
  }

  .lightbox-close {
    position: absolute;
    top: 24px;
    right: 24px;
    background: none;
    border: none;
    color: #ffffff;
    cursor: pointer;
    z-index: 1102;
  }

  .lightbox-img-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    max-width: 90%;
    max-height: 85%;
  }

  .lightbox-img {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
    border-radius: 8px;
  }

  /* Responsive layout */
  @media (max-width: 992px) {
    .activities-split {
      grid-template-columns: 1fr;
      gap: 40px;
    }

    .gallery-grid {
      grid-template-columns: repeat(4, 1fr);
    }
  }

  @media (max-width: 640px) {
    .gallery-grid {
      grid-template-columns: repeat(2, 1fr);
    }

    .logo-grid {
      grid-template-columns: repeat(2, 1fr);
    }

    .title {
      font-size: 28px;
    }
  }
</style>
