<script>
  import { onMount } from 'svelte';
  import Header from '$lib/components/Header.svelte';
  import Footer from '$lib/components/Footer.svelte';
  import FloatingActions from '$lib/components/FloatingActions.svelte';
  import Certifications from '$lib/components/Certifications.svelte';

  let stickyMenu = $state(false);
  let activeSection = $state('imlab');

  const partners = [
    {
      id: 'imlab',
      name: '아이엠랩',
      title: '아이엠랩 X 마인드앤매뉴얼',
      desc: '아이엠랩은 심폐소생술 교육 분야의 창의적이고 혁신적인 스타트업으로 IoT기반의 교구 및 프로그램 개발을 통해 심폐소생술 교육을 보다 과학적이고 효율적으로 개선하고 있습니다. 아이엠랩과 마인드앤매뉴얼은 연구∙개발과 교육서비스 분야에서 다양한 프로젝트를 진행하고, 선진적인 심폐소생술 교육의 보급과 확대를 위하여 비즈니스 시너지를 내고 있습니다. 마인드앤매뉴얼 서울센터에서는 아이엠랩의 다양한 제품과 서비스들을 체험하실 수 있습니다.',
      imgs: [
        'https://cdn.imweb.me/thumbnail/20230830/e57fe1a3fbad1.png',
        'https://cdn.imweb.me/thumbnail/20230830/fc7ac6bc3aa4e.png'
      ]
    },
    {
      id: 'seoul-safety',
      name: '서울안전한마당',
      title: '서울안전한마당',
      desc: '서울안전한마당은 17년째 진행되고 있는 서울시 주요재난 대비 안전체험 프로그램으로 시민안전 강화와 시민들이 자발적으로 참여하고 즐길 수 있는 국내 최대 안전문화 행사입니다. 마인드앤매뉴얼은 ‘21년도부터 매년 생활안전 분야의 전문기관으로 참여하여 메타버스 심폐소생술 및 IoT를 이용한 어린이 안전교육 등 다양하고 재미있는 응급처치 교육 프로그램을 운영하고 있습니다.',
      imgs: [
        'https://cdn.imweb.me/thumbnail/20230830/530a538e06bd9.png',
        'https://cdn.imweb.me/thumbnail/20230830/8b703a9b09079.png'
      ]
    },
    {
      id: 'bynaeng',
      name: '바이냉',
      title: '바이냉 X 마인드앤매뉴얼',
      desc: '신은영 작가님은 인스타그램 등 SNS 상에서 Bynaeng이라는 닉네임으로 색연필 일러스트 작품 활동을 하고 계신 인플루언서 디자이너입니다. 작가님이 직접 손으로 그린 일러스트와 메세지를 통해 자칫 딱딱하게 느껴질 수 있는 심폐소생술 교육을 사람들에게 따뜻하고 공감할 수 있는 이야기로 전달할 수 있었습니다. 마인드앤매뉴얼에서 작가님이 만드신 예쁘고 다양한 엽서와 카드를 받아보세요.',
      imgs: [
        'https://cdn.imweb.me/thumbnail/20230830/785d2c3b0f2ce.png',
        'https://cdn.imweb.me/thumbnail/20230830/6171634bab9b2.png'
      ]
    },
    {
      id: 'cushion-ann',
      name: '쿠션앤',
      title: '쿠션앤 프로젝트',
      desc: '쿠션앤은 마인드앤매뉴얼에서 활동하신 최신영 작가님과 함께 한 우리 일상 속에 심폐소생술을 자연스럽게 접목시키기 위한 창의적인 프로젝트입니다. 귀여운 커버의 이 쿠션은 평상 시에는 홈이나 아웃도어 용품으로 사용할 수 있으며, 가슴압박을 하듯이 누르면 ‘삑삑삑’ 소리가 나도록 구현되어 있어 아이들과 함께 심폐소생술 실습교구로도 활용할 수 있습니다.',
      imgs: [
        'https://cdn.imweb.me/thumbnail/20230830/c211a219f26c5.png',
        'https://cdn.imweb.me/thumbnail/20230830/170ae66f0f815.png'
      ]
    },
    {
      id: 'heart-foundation',
      name: '한국심장재단',
      title: '한국심장재단',
      desc: '한국심장재단은 주로 경제적 형편이 어려워 치료받지 못하고 질병의 고통 속에 있는 심장병 및 기타 질환 환자들에게 진료비를 지원하는 단체입니다. 지원사업과 더불어 ‘심장병 예방 걷기대회’ 등 지역사회에 의미 있고 다양한 활동을 전개하고 있으며, 마인드앤매뉴얼은 ’19년부터 매년 재단 행사에 동참하여 심폐소생술 및 응급처치 교육 등 적극적인 지원을 하고 있습니다.',
      imgs: [
        'https://cdn.imweb.me/thumbnail/20230830/7d47ac0175987.png',
        'https://cdn.imweb.me/thumbnail/20230830/c3f57c67c2337.png'
      ]
    }
  ];

  onMount(() => {
    const handleScroll = () => {
      const scrollY = window.scrollY;
      stickyMenu = scrollY > 460; // Sticky past the hero descriptions and banner image

      // Active section detection
      for (const p of partners) {
        const el = document.getElementById(p.id);
        if (el) {
          const rect = el.getBoundingClientRect();
          if (rect.top <= 160 && rect.bottom >= 160) {
            activeSection = p.id;
            break;
          }
        }
      }
    };

    window.addEventListener('scroll', handleScroll);
    handleScroll();

    return () => window.removeEventListener('scroll', handleScroll);
  });

  function scrollToSection(id) {
    const el = document.getElementById(id);
    if (el) {
      const offset = 140; // sticky header (70px) + sticky subnav (60px)
      const bodyRect = document.body.getBoundingClientRect().top;
      const elementRect = el.getBoundingClientRect().top;
      const elementPosition = elementRect - bodyRect;
      const offsetPosition = elementPosition - offset;

      window.scrollTo({
        top: offsetPosition,
        behavior: 'smooth'
      });
    }
  }
</script>

<svelte:head>
  <title>파트너십 / 제휴 | 마인드앤매뉴얼</title>
  <meta name="description" content="마인드앤매뉴얼의 파트너스 및 비즈니스 비전, 주요 프로젝트와 협업 소식을 안내합니다." />
</svelte:head>

<Header />

<!-- Page Hero Banner -->
<section class="partner-hero-banner">
  <div class="container hero-container animate-fade-in">
    <h1 class="hero-title">파트너십 / 제휴</h1>
    <div class="hero-subtitle-divider"></div>
    <p class="hero-desc">
      마인드앤매뉴얼은 서비스 컨텐츠의 품질 향상 및 다양성을 확보하고,<br />
      응급처치교육의 확대를 위하여 다양한 분야의 전문가 및 기관들과 함께 하고 있습니다.<br />
      교육 분야의 연구∙개발, 행사 교육 서비스 및 안전 지원, 그 외 비즈니스 협업 등<br />
      마인드앤매뉴얼은 진정성 있고 창의적인 프로젝트들을 적극적으로 지원합니다.
    </p>

    <!-- Main Banner Image -->
    <div class="hero-image-box">
      <img src="https://cdn.imweb.me/thumbnail/20230830/fab73eefd670b.png" alt="파트너십 메인 그래픽" class="hero-img" />
    </div>
  </div>
</section>

<!-- Inquiry CTA (신청방법) -->
<section class="inquiry-cta-section py-section">
  <div class="container text-center animate-slide-up">
    <h2 class="inquiry-title">I 신청방법</h2>
    <p class="inquiry-desc">제안 내용 및 기획서 등을 메일로 송부해주시면 담당자 검토 후 연락드리겠습니다.</p>
    <div class="inquiry-buttons">
      <a href="mailto:mind_manual@naver.com" class="inquiry-btn mail-btn">
        <svg class="inquiry-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/>
        </svg>
        <span>이메일 문의하기</span>
      </a>
      <a href="tel:1660-3707" class="inquiry-btn tel-btn">
        <svg class="inquiry-icon" width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
          <path d="M6.62 10.79a15.15 15.15 0 006.59 6.59l2.2-2.2a1 1 0 011.11-.27 11.72 11.72 0 003.7.59 1 1 0 011 1V20a1 1 0 01-1 1A17 17 0 013 4a1 1 0 011-1h3.5a1 1 0 011 1 11.72 11.72 0 00.59 3.7 1 1 0 01-.27 1.1l-2.2 2.2z"/>
        </svg>
        <span>전화 문의하기</span>
      </a>
    </div>
  </div>
</section>

<!-- Sticky Sub-Navigation -->
<div class="sub-nav-wrapper {stickyMenu ? 'sticky' : ''}">
  <div class="container sub-nav-container">
    <div class="sub-nav-pills">
      {#each partners as p}
        <button 
          class="sub-nav-pill {activeSection === p.id ? 'active' : ''}"
          onclick={() => scrollToSection(p.id)}
        >
          {p.name}
        </button>
      {/each}
    </div>
  </div>
</div>

<!-- Partners Sections -->
<div class="partners-content-list bg-white">
  {#each partners as p, idx}
    <section id={p.id} class="partner-item-section py-section">
      <div class="container section-container animate-slide-up">
        <!-- Partner Section Title -->
        <div class="partner-title-row">
          <h2 class="partner-section-title">
            <span class="orange-bar">I</span>
            <span>{p.title}</span>
          </h2>
        </div>

        <!-- Two-column grid layout for images -->
        <div class="partner-gallery-grid">
          {#each p.imgs as img}
            <div class="partner-gallery-item">
              <img src={img} alt={p.name} class="partner-gallery-img" />
            </div>
          {/each}
        </div>

        <!-- Description Paragraph -->
        <div class="partner-desc-box">
          <p class="partner-paragraph">{p.desc}</p>
        </div>
      </div>
    </section>

    {#if idx < partners.length - 1}
      <div class="container"><div class="section-divider"></div></div>
    {/if}
  {/each}
</div>

<!-- Bottom CTA Banner -->
<section class="bottom-cta-section py-section">
  <div class="container text-center animate-slide-up">
    <h2 class="bottom-cta-title">마인드앤매뉴얼과 함께하세요.</h2>
    <p class="bottom-cta-desc">진정성 있고 창의적인 협업 비즈니스 아이디어를 환영합니다.</p>
    <div class="inquiry-buttons">
      <a href="mailto:mind_manual@naver.com" class="inquiry-btn mail-btn white-btn">
        <svg class="inquiry-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/>
        </svg>
        <span>이메일 문의하기</span>
      </a>
      <a href="tel:1660-3707" class="inquiry-btn tel-btn white-btn">
        <svg class="inquiry-icon" width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
          <path d="M6.62 10.79a15.15 15.15 0 006.59 6.59l2.2-2.2a1 1 0 011.11-.27 11.72 11.72 0 003.7.59 1 1 0 011 1V20a1 1 0 01-1 1A17 17 0 013 4a1 1 0 011-1h3.5a1 1 0 011 1 11.72 11.72 0 00.59 3.7 1 1 0 01-.27 1.1l-2.2 2.2z"/>
        </svg>
        <span>전화 문의하기</span>
      </a>
    </div>
  </div>
</section>

<Certifications />
<Footer />
<FloatingActions />

<style>
  /* Base layouts / anchor settings */
  section {
    scroll-margin-top: 135px;
  }

  .partner-hero-banner {
    background: linear-gradient(135deg, #fefdfb 0%, #f6f3ed 100%);
    padding: 140px 0 60px;
  }

  .hero-container {
    text-align: center;
    max-width: 900px;
    margin: 0 auto;
  }

  .hero-title {
    font-size: 44px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 15px;
    letter-spacing: -1px;
  }

  .hero-subtitle-divider {
    width: 50px;
    height: 3px;
    background-color: var(--primary);
    margin: 0 auto 20px;
  }

  .hero-desc {
    font-size: 16px;
    line-height: 1.8;
    color: var(--text-muted);
    font-weight: 400;
    margin-bottom: 40px;
    word-break: keep-all;
  }

  .hero-image-box {
    width: 100%;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
    border: 1px solid var(--border-color);
  }

  .hero-img {
    width: 100%;
    height: auto;
    display: block;
    object-fit: cover;
  }

  /* Inquiry CTA Area */
  .inquiry-cta-section {
    background-color: var(--bg-light);
    border-top: 1px solid var(--border-color);
    border-bottom: 1px solid var(--border-color);
    padding: 50px 0;
  }

  .inquiry-title {
    font-size: 24px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 8px;
  }

  .inquiry-desc {
    font-size: 15px;
    color: var(--text-muted);
    margin-bottom: 24px;
  }

  .inquiry-buttons {
    display: flex;
    justify-content: center;
    gap: 16px;
  }

  .inquiry-btn {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 12px 28px;
    border-radius: 30px;
    font-size: 15px;
    font-weight: 600;
    transition: all var(--transition-speed);
  }

  .mail-btn {
    background-color: var(--primary);
    color: var(--bg-white);
    box-shadow: 0 4px 15px rgba(255, 121, 0, 0.2);
  }

  .mail-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(255, 121, 0, 0.3);
  }

  .tel-btn {
    background-color: var(--dark);
    color: var(--bg-white);
    box-shadow: 0 4px 15px rgba(33, 33, 33, 0.2);
  }

  .tel-btn:hover {
    background-color: #000;
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(33, 33, 33, 0.3);
  }

  /* Sticky subnav */
  .sub-nav-wrapper {
    background-color: var(--bg-white);
    border-bottom: 1px solid var(--border-color);
    padding: 15px 0;
    z-index: 900;
    width: 100%;
    transition: all 0.3s ease;
  }

  .sub-nav-wrapper.sticky {
    position: fixed;
    top: 90px;
    left: 0;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04);
    background-color: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
  }

  :global(.header.scrolled) ~ .sub-nav-wrapper.sticky {
    top: 70px;
  }

  .sub-nav-container {
    display: flex;
    justify-content: center;
  }

  .sub-nav-pills {
    display: flex;
    gap: 12px;
    overflow-x: auto;
    max-width: 100%;
    scrollbar-width: none;
  }

  .sub-nav-pills::-webkit-scrollbar {
    display: none;
  }

  .sub-nav-pill {
    padding: 10px 24px;
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 30px;
    font-size: 14px;
    font-weight: 500;
    color: #555555;
    cursor: pointer;
    white-space: nowrap;
    transition: all var(--transition-speed);
  }

  .sub-nav-pill:hover, .sub-nav-pill.active {
    background-color: var(--primary);
    color: var(--bg-white);
    border-color: var(--primary);
    box-shadow: 0 4px 10px rgba(255, 121, 0, 0.15);
  }

  /* Partner list items */
  .partner-item-section {
    background-color: var(--bg-white);
  }

  .partner-title-row {
    margin-bottom: 35px;
  }

  .partner-section-title {
    font-size: 26px;
    font-weight: 700;
    color: var(--dark);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .partner-section-title .orange-bar {
    color: var(--primary);
    font-weight: 900;
  }

  .partner-gallery-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
    margin-bottom: 30px;
  }

  .partner-gallery-item {
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.03);
    border: 1px solid var(--border-color);
  }

  .partner-gallery-img {
    width: 100%;
    height: 320px;
    object-fit: cover;
    display: block;
    transition: transform var(--transition-speed);
  }

  .partner-gallery-item:hover .partner-gallery-img {
    transform: scale(1.03);
  }

  .partner-desc-box {
    max-width: 1000px;
  }

  .partner-paragraph {
    font-size: 16px;
    line-height: 1.8;
    color: var(--text);
    word-break: keep-all;
    font-weight: 400;
  }

  .section-divider {
    width: 100%;
    height: 1px;
    background-color: var(--border-color);
  }

  /* Bottom CTA Banner */
  .bottom-cta-section {
    background-color: var(--primary);
    color: var(--bg-white);
  }

  .bottom-cta-title {
    font-size: 28px;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 10px;
  }

  .bottom-cta-desc {
    font-size: 16px;
    color: rgba(255, 255, 255, 0.85);
    margin-bottom: 30px;
  }

  .white-btn {
    background-color: var(--bg-white);
    color: var(--primary);
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  }

  .white-btn:hover {
    background-color: #fff3e6;
    color: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
  }

  .tel-btn.white-btn {
    background-color: var(--dark);
    color: var(--bg-white);
  }

  .tel-btn.white-btn:hover {
    background-color: #000;
    color: var(--bg-white);
  }

  /* Responsive styling */
  @media (max-width: 992px) {
    .partner-gallery-grid {
      grid-template-columns: 1fr;
    }

    .partner-gallery-img {
      height: 240px;
    }

    .inquiry-buttons {
      flex-direction: column;
      align-items: center;
      gap: 12px;
    }

    .inquiry-btn {
      width: 100%;
      max-width: 280px;
      justify-content: center;
    }
  }

  @media (max-width: 768px) {
    .hero-title {
      font-size: 32px;
    }

    .hero-desc {
      font-size: 14px;
    }

    .partner-hero-banner {
      padding: 120px 0 50px;
    }

    .partner-section-title {
      font-size: 20px;
    }

    .partner-paragraph {
      font-size: 14px;
      line-height: 1.7;
    }

    .bottom-cta-title {
      font-size: 22px;
    }
  }
</style>
