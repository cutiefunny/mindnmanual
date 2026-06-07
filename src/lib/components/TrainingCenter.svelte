<script>
  import { onMount, onDestroy } from 'svelte';

  const seoulImages = [
    'https://cdn.imweb.me/thumbnail/20230814/366074ab24b97.png',
    'https://cdn.imweb.me/thumbnail/20230814/6f63f82222f6e.png',
    'https://cdn.imweb.me/thumbnail/20230814/49e6606d78601.png',
    'https://cdn.imweb.me/thumbnail/20230814/9af84bfee06f5.png'
  ];

  let currentSeoulImg = $state(0);
  let intervalId;

  function nextSeoulImg() {
    currentSeoulImg = (currentSeoulImg + 1) % seoulImages.length;
  }

  onMount(() => {
    intervalId = setInterval(nextSeoulImg, 4000);
  });

  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });
</script>

<section id="training-center" class="training-section py-section">
  <div class="container">
    <div class="section-header animate-slide-up">
      <span class="sub-title">Training Centers</span>
      <h2 class="title">트레이닝 센터</h2>
      <p class="desc-text">최신식 실습 교구재와 쾌적한 환경을 갖춘 교육 공간을 안내해 드립니다.</p>
    </div>

    <div class="centers-grid">
      <!-- Seoul Center -->
      <div class="center-card animate-slide-up">
        <div class="center-image-wrapper">
          <div class="center-slider">
            {#each seoulImages as img, idx}
              <img 
                src={img} 
                alt="서울 합정 교육 센터" 
                class="center-img {idx === currentSeoulImg ? 'active' : ''}" 
              />
            {/each}
          </div>
          <div class="center-tag status-active">교육 진행 중</div>
        </div>
        <div class="center-details">
          <div class="center-title-row">
            <span class="center-location-badge">서울 본원</span>
            <h3 class="center-title">마인드앤매뉴얼 서울 (합정동 교육관)</h3>
          </div>
          <p class="center-desc">
            마포구 합정동에 위치한 본원으로, 대형 단체 교육이 가능한 쾌적하고 넓은 실습 공간과 기가비트 네트워크 IoT 시스템이 구비되어 실시간 디지털 피드백 CPR 교육이 가능합니다.
          </p>
          <div class="center-info-list">
            <div class="info-item">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="info-icon">
                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0118 0z"/>
                <circle cx="12" cy="10" r="3"/>
              </svg>
              <span>서울특별시 마포구 독막로6길 8, 2층</span>
            </div>
            <div class="info-item">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="info-icon">
                <path d="M22 16.92v3a2 2 0 01-2.18 2 19.79 19.79 0 01-8.63-3.07 19.5 19.5 0 01-6-6 19.79 19.79 0 01-3.07-8.67A2 2 0 014.11 2h3a2 2 0 012 1.72 12.84 12.84 0 00.7 2.81 2 2 0 01-.45 2.11L8.09 9.91a16 16 0 006 6l1.27-1.27a2 2 0 012.11-.45 12.84 12.84 0 002.81.7A2 2 0 0122 16.92z"/>
              </svg>
              <span>1660-3707</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Busan Center (Coming Soon) -->
      <div class="center-card animate-slide-up" style="animation-delay: 0.2s">
        <div class="center-image-wrapper">
          <img src="https://cdn.imweb.me/thumbnail/20230814/ef2af6cfc1a7e.png" alt="부산 교육 센터 예정지" class="center-img active" />
          <div class="coming-soon-overlay">
            <div class="coming-soon-text">
              <h3>COMING SOON</h3>
              <p>부산 지역에서도 곧 마인드앤매뉴얼을 만나보실 수 있습니다.</p>
            </div>
          </div>
          <div class="center-tag status-pending">준비 중</div>
        </div>
        <div class="center-details">
          <div class="center-title-row">
            <span class="center-location-badge badge-pending">부산 지사</span>
            <h3 class="center-title">마인드앤매뉴얼 부산 교육관</h3>
          </div>
          <p class="center-desc">
            부울경 지역의 수강생분들에게 편리하고 신속한 교육을 전달해 드리고자 영남권 교육관 오픈을 준비하고 있습니다. 더 가까이서 뵙도록 하겠습니다.
          </p>
          <div class="center-info-list">
            <div class="info-item">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="info-icon">
                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0118 0z"/>
                <circle cx="12" cy="10" r="3"/>
              </svg>
              <span>부산광역시 주요 거점 오픈 예정</span>
            </div>
            <div class="info-item">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="info-icon">
                <path d="M22 16.92v3a2 2 0 01-2.18 2 19.79 19.79 0 01-8.63-3.07 19.5 19.5 0 01-6-6 19.79 19.79 0 01-3.07-8.67A2 2 0 014.11 2h3a2 2 0 012 1.72 12.84 12.84 0 00.7 2.81 2 2 0 01-.45 2.11L8.09 9.91a16 16 0 006 6l1.27-1.27a2 2 0 012.11-.45 12.84 12.84 0 002.81.7A2 2 0 0122 16.92z"/>
              </svg>
              <span>고객상담 대표번호 1660-3707</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .training-section {
    background-color: var(--bg-light);
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

  .centers-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 32px;
  }

  .center-card {
    background-color: var(--bg-white);
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.04);
    display: flex;
    flex-direction: column;
  }

  .center-image-wrapper {
    position: relative;
    width: 100%;
    aspect-ratio: 16/10;
    overflow: hidden;
    background-color: var(--bg-light);
  }

  .center-slider {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .center-img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0;
    transition: opacity 0.8s ease;
  }

  .center-img.active {
    opacity: 1;
  }

  .coming-soon-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.65);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
    text-align: center;
    color: #ffffff;
    z-index: 2;
  }

  .coming-soon-text h3 {
    font-size: 24px;
    font-weight: 700;
    color: var(--primary);
    margin-bottom: 8px;
    letter-spacing: 1.5px;
  }

  .coming-soon-text p {
    font-size: 14px;
    color: rgba(255, 255, 255, 0.8);
  }

  .center-tag {
    position: absolute;
    top: 20px;
    right: 20px;
    padding: 6px 14px;
    border-radius: 30px;
    font-size: 11px;
    font-weight: 700;
    color: #ffffff;
    z-index: 3;
  }

  .status-active {
    background-color: #2ec4b6;
  }

  .status-pending {
    background-color: #e63946;
  }

  .center-details {
    padding: 30px;
    display: flex;
    flex-direction: column;
    flex-grow: 1;
  }

  .center-title-row {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 15px;
  }

  .center-location-badge {
    background-color: rgba(255, 121, 0, 0.1);
    color: var(--primary);
    font-size: 12px;
    font-weight: 600;
    padding: 4px 10px;
    border-radius: 4px;
  }

  .center-location-badge.badge-pending {
    background-color: rgba(230, 57, 70, 0.1);
    color: #e63946;
  }

  .center-title {
    font-size: 20px;
    font-weight: 700;
    color: var(--dark);
  }

  .center-desc {
    font-size: 14px;
    line-height: 1.6;
    color: var(--text-muted);
    margin-bottom: 24px;
    word-break: keep-all;
  }

  .center-info-list {
    margin-top: auto;
    display: flex;
    flex-direction: column;
    gap: 12px;
    border-top: 1px solid var(--border-color);
    padding-top: 20px;
  }

  .info-item {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 13px;
    color: #555555;
  }

  .info-icon {
    color: var(--primary);
    flex-shrink: 0;
  }

  /* Responsive view */
  @media (max-width: 992px) {
    .centers-grid {
      grid-template-columns: 1fr;
      gap: 24px;
    }
  }

  @media (max-width: 640px) {
    .title {
      font-size: 28px;
    }

    .center-title {
      font-size: 18px;
    }

    .center-details {
      padding: 20px;
    }
  }
</style>
