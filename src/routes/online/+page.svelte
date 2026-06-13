<script>
  import { onMount, onDestroy } from 'svelte';

  let isPlaying = $state(false);
  let isCompleted = $state(false);
  let player = null;

  // Initialize YouTube IFrame Player
  function initPlayer() {
    if (!window.YT) return;
    
    player = new window.YT.Player('youtube-player', {
      height: '100%',
      width: '100%',
      videoId: 'q7J2T6MFA9g',
      playerVars: {
        autoplay: 1,
        rel: 0,
        modestbranding: 1,
        controls: 1,
        showinfo: 0,
        fs: 1
      },
      events: {
        'onStateChange': onPlayerStateChange
      }
    });
    window.player = player;
  }

  function onPlayerStateChange(event) {
    // YT.PlayerState.ENDED is 0
    if (event.data === 0) {
      isCompleted = true;
    }
  }

  function startVideo() {
    isPlaying = true;
    if (player && typeof player.playVideo === 'function') {
      player.playVideo();
      return;
    }

    if (window.YT && window.YT.Player) {
      initPlayer();
    } else {
      // Load YouTube IFrame API script dynamically
      if (!document.getElementById('youtube-iframe-api')) {
        const tag = document.createElement('script');
        tag.id = 'youtube-iframe-api';
        tag.src = 'https://www.youtube.com/iframe_api';
        const firstScriptTag = document.getElementsByTagName('script')[0];
        firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
        
        window.onYouTubeIframeAPIReady = () => {
          initPlayer();
        };
      } else {
        const checkReady = setInterval(() => {
          if (window.YT && window.YT.Player) {
            clearInterval(checkReady);
            initPlayer();
          }
        }, 100);
      }
    }
  }

  function resetVideo() {
    if (player && typeof player.stopVideo === 'function') {
      player.stopVideo();
    }
    isPlaying = false;
    isCompleted = false;
  }

  onDestroy(() => {
    if (player && typeof player.destroy === 'function') {
      player.destroy();
    }
  });
</script>

<svelte:head>
  <title>온라인 교육 | 마인드앤매뉴얼</title>
  <meta name="description" content="시간과 공간의 제약 없이 동영상을 통해 배우는 마인드앤매뉴얼 온라인 응급처치 교육 서비스입니다. 올바른 심폐소생술과 자동심장충격기 사용법을 학습하세요." />
</svelte:head>

<div class="page-container">
  <div class="container py-section">
    <!-- Breadcrumbs & Header -->
    <div class="header-section animate-slide-up">
      <div class="category-badge">ONLINE COURSE</div>
      <h1 class="page-title">온라인 응급처치 및 심폐소생술 교육</h1>
      <p class="page-desc">
        생명을 구하는 올바른 대처 방법, 영상을 통해 안전 교육의 기초를 체계적으로 다져보세요.<br />
        시작 버튼을 클릭하여 영상을 끝까지 시청하시면 이수 완료가 표시됩니다.
      </p>
    </div>

    <!-- Video Stage Box -->
    <div class="video-stage animate-zoom-in">
      <div class="video-ratio-wrapper">
        <!-- YouTube iframe target -->
        <div id="youtube-player" class="player-iframe" class:hidden={!isPlaying}></div>

        <!-- Start Overlay -->
        {#if !isPlaying && !isCompleted}
          <div class="video-overlay start-overlay">
            <div class="thumbnail-bg" style="background-image: url('https://img.youtube.com/vi/q7J2T6MFA9g/maxresdefault.jpg')"></div>
            <div class="overlay-blur-cover"></div>
            <div class="overlay-content">
              <span class="video-badge">기초 과정</span>
              <h3 class="video-title">올바른 심폐소생술과 제세동기 사용법</h3>
              <p class="video-subtitle">행정안전부·대한심폐소생협회 표준 가이드라인 교육</p>
              
              <button onclick={startVideo} class="play-btn-large">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                  <polygon points="5 3 19 12 5 21 5 3" fill="currentColor"/>
                </svg>
                <span>교육 시작하기</span>
              </button>
            </div>
          </div>
        {/if}

        <!-- Completed Overlay -->
        {#if isCompleted}
          <div class="video-overlay completed-overlay animate-fade-in">
            <div class="completed-content">
              <div class="success-icon-box">
                <svg class="checkmark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52">
                  <circle class="checkmark__circle" cx="26" cy="26" r="25" fill="none"/>
                  <path class="checkmark__check" fill="none" d="M14.1 27.2l7.1 7.2 16.7-16.8"/>
                </svg>
              </div>
              
              <h3 class="completed-title">교육 이수 완료</h3>
              <p class="completed-subtitle">
                '올바른 심폐소생술과 제세동기 사용법' 기초 과정을 끝까지 성실하게 이수하셨습니다.
              </p>
              
              <div class="badge-row">
                <div class="status-badge is-completed">
                  <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                    <polyline points="20 6 9 17 4 12"></polyline>
                  </svg>
                  <span>이수 상태: 완료</span>
                </div>
              </div>

              <div class="completed-actions">
                <a href="/Application" class="btn-action btn-primary">다른 실습 교육 신청하기</a>
                <button onclick={resetVideo} class="btn-action btn-outline">다시 시청하기</button>
              </div>
            </div>
          </div>
        {/if}
      </div>
    </div>

    <!-- Curriculum details below the video -->
    <div class="course-curriculum animate-slide-up">
      <h2 class="section-title">주요 학습 내용</h2>
      
      <div class="step-grid">
        <div class="step-card">
          <div class="step-num">01</div>
          <h3 class="step-title">반응 및 호흡 확인</h3>
          <p class="step-desc">
            환자의 어깨를 가볍게 두드리며 의식을 확인하고 눈으로 호흡 여부를 판별합니다.
          </p>
        </div>

        <div class="step-card">
          <div class="step-num">02</div>
          <h3 class="step-title">도움 요청 및 AED 호출</h3>
          <p class="step-desc">
            주변에 있는 특정 사람을 지목하여 119 신고를 부탁하고 자동심장충격기(AED)를 가져오도록 요청합니다.
          </p>
        </div>

        <div class="step-card">
          <div class="step-num">03</div>
          <h3 class="step-title">가슴압박 30회 실시</h3>
          <p class="step-desc">
            분당 100~120회 속도로, 약 5~6cm 깊이로 강하고 빠르게 환자의 가슴 중앙을 압박합니다.
          </p>
        </div>

        <div class="step-card">
          <div class="step-num">04</div>
          <h3 class="step-title">자동심장충격기 사용</h3>
          <p class="step-desc">
            기기가 도착하는 즉시 전원을 켜고 음성 안내에 따라 패드를 부착한 뒤 분석 및 전기충격을 가합니다.
          </p>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .page-container {
    padding-top: 130px;
    background-color: var(--bg-light);
    min-height: 100vh;
  }

  @media (max-width: 768px) {
    .page-container {
      padding-top: 90px;
    }
  }

  .header-section {
    text-align: center;
    max-width: 800px;
    margin: 0 auto 50px;
  }

  .category-badge {
    display: inline-block;
    padding: 6px 14px;
    background-color: rgba(255, 121, 0, 0.1);
    color: var(--primary);
    font-size: 12px;
    font-weight: 700;
    border-radius: 30px;
    margin-bottom: 15px;
    letter-spacing: 1.5px;
  }

  .page-title {
    font-size: 36px;
    font-weight: 800;
    color: var(--dark);
    margin-bottom: 18px;
    letter-spacing: -1px;
    line-height: 1.3;
  }

  .page-desc {
    font-size: 16px;
    color: var(--text-muted);
    line-height: 1.6;
    word-break: keep-all;
  }

  /* Video Stage Wrapper */
  .video-stage {
    max-width: 1000px;
    margin: 0 auto 60px;
    background-color: #000000;
    border-radius: 24px;
    overflow: hidden;
    box-shadow: 0 25px 60px rgba(0, 0, 0, 0.15);
    border: 4px solid var(--bg-white);
  }

  .video-ratio-wrapper {
    position: relative;
    width: 100%;
    padding-top: 56.25%; /* 16:9 Aspect Ratio */
  }

  .player-iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    border: 0;
  }

  .player-iframe.hidden {
    display: none;
  }

  /* Video Overlay CSS */
  .video-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 5;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffffff;
    text-align: center;
  }

  /* Start Overlay specific */
  .thumbnail-bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
  }

  .overlay-blur-cover {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(4px);
  }

  .overlay-content {
    position: relative;
    z-index: 2;
    padding: 24px;
    max-width: 600px;
  }

  .video-badge {
    display: inline-block;
    padding: 4px 10px;
    background-color: var(--primary);
    font-size: 11px;
    font-weight: 700;
    border-radius: 4px;
    margin-bottom: 12px;
  }

  .video-title {
    font-size: 28px;
    font-weight: 700;
    margin-bottom: 8px;
    text-shadow: 0 2px 4px rgba(0,0,0,0.5);
    word-break: keep-all;
  }

  .video-subtitle {
    font-size: 15px;
    color: rgba(255, 255, 255, 0.85);
    margin-bottom: 28px;
  }

  .play-btn-large {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    background-color: var(--primary);
    color: #ffffff;
    border: none;
    padding: 14px 28px;
    border-radius: 35px;
    font-size: 16px;
    font-weight: 700;
    cursor: pointer;
    box-shadow: 0 10px 25px rgba(255, 121, 0, 0.4);
    transition: all var(--transition-speed) ease;
  }

  .play-btn-large:hover {
    background-color: var(--primary-hover);
    transform: scale(1.05);
  }

  /* Completed Overlay specific */
  .completed-overlay {
    background-color: rgba(15, 15, 20, 0.95);
    backdrop-filter: blur(8px);
  }

  .completed-content {
    max-width: 550px;
    padding: 30px;
    z-index: 2;
  }

  .completed-title {
    font-size: 30px;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 12px;
    letter-spacing: -0.5px;
  }

  .completed-subtitle {
    font-size: 15px;
    color: rgba(255, 255, 255, 0.7);
    margin-bottom: 24px;
    line-height: 1.5;
    word-break: keep-all;
  }

  .badge-row {
    display: flex;
    justify-content: center;
    margin-bottom: 30px;
  }

  .status-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 8px 16px;
    border-radius: 30px;
    font-size: 13px;
    font-weight: 600;
  }

  .status-badge.is-completed {
    background-color: rgba(46, 196, 182, 0.15);
    color: #2ec4b6;
    border: 1px solid rgba(46, 196, 182, 0.3);
  }

  .completed-actions {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 16px;
  }

  .btn-action {
    display: inline-block;
    padding: 12px 24px;
    font-size: 14px;
    font-weight: 700;
    border-radius: 30px;
    cursor: pointer;
    transition: all var(--transition-speed) ease;
  }

  .btn-primary {
    background-color: var(--primary);
    color: #ffffff;
    border: none;
    box-shadow: 0 5px 15px rgba(255, 121, 0, 0.3);
  }

  .btn-primary:hover {
    background-color: var(--primary-hover);
    transform: translateY(-2px);
  }

  .btn-outline {
    background-color: transparent;
    color: #ffffff;
    border: 1px solid rgba(255, 255, 255, 0.3);
  }

  .btn-outline:hover {
    background-color: rgba(255, 255, 255, 0.1);
    border-color: #ffffff;
  }

  /* Success Icon Box & Checkmark Animation */
  .success-icon-box {
    margin-bottom: 25px;
    display: flex;
    justify-content: center;
  }

  .checkmark__circle {
    stroke-dasharray: 166;
    stroke-dashoffset: 166;
    stroke-width: 2;
    stroke-miterlimit: 10;
    stroke: #2ec4b6;
    fill: none;
    animation: stroke 0.6s cubic-bezier(0.65, 0, 0.45, 1) forwards;
  }

  .checkmark {
    width: 68px;
    height: 68px;
    border-radius: 50%;
    display: block;
    stroke-width: 3;
    stroke: #fff;
    stroke-miterlimit: 10;
    box-shadow: inset 0px 0px 0px #2ec4b6;
    animation: fill-animation .4s ease-in-out .4s forwards, scale-animation .3s ease-in-out .9s forwards;
  }

  .checkmark__check {
    transform-origin: 50% 50%;
    stroke-dasharray: 48;
    stroke-dashoffset: 48;
    animation: stroke 0.3s cubic-bezier(0.65, 0, 0.45, 1) 0.8s forwards;
  }

  @keyframes stroke {
    100% {
      stroke-dashoffset: 0;
    }
  }

  @keyframes fill-animation {
    100% {
      box-shadow: inset 0px 0px 0px 40px #2ec4b6;
    }
  }

  @keyframes scale-animation {
    0%, 100% {
      transform: none;
    }
    50% {
      transform: scale3d(1.1, 1.1, 1);
    }
  }

  /* Course Curriculum Grid */
  .course-curriculum {
    max-width: 1000px;
    margin: 0 auto;
    background-color: var(--bg-white);
    padding: 50px 40px;
    border-radius: 24px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
    border: 1px solid var(--border-color);
  }

  .section-title {
    font-size: 22px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 30px;
    text-align: center;
  }

  .step-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 24px;
  }

  .step-card {
    position: relative;
    padding: 24px;
    background-color: var(--bg-light);
    border-radius: 16px;
    transition: all var(--transition-speed) ease;
  }

  .step-card:hover {
    transform: translateY(-5px);
    background-color: #ffffff;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
  }

  .step-num {
    font-size: 32px;
    font-weight: 800;
    color: rgba(255, 121, 0, 0.2);
    margin-bottom: 12px;
    line-height: 1;
  }

  .step-title {
    font-size: 16px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 8px;
  }

  .step-desc {
    font-size: 13px;
    line-height: 1.5;
    color: var(--text-muted);
    word-break: keep-all;
  }

  @media (max-width: 992px) {
    .step-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 640px) {
    .page-title {
      font-size: 26px;
    }
    
    .video-stage {
      margin-bottom: 40px;
    }

    .video-title {
      font-size: 20px;
    }

    .completed-title {
      font-size: 22px;
    }

    .completed-actions {
      flex-direction: column;
      gap: 10px;
    }

    .btn-action {
      width: 100%;
    }

    .step-grid {
      grid-template-columns: 1fr;
    }

    .course-curriculum {
      padding: 30px 20px;
    }
  }
</style>
