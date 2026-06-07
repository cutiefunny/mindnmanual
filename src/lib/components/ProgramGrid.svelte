<script>
  const programs = [
    {
      img: "https://cdn.imweb.me/thumbnail/20230814/74ca9bbdd0d58.png",
      badge: "기업 / 단체 대상",
      title: "스마트 심폐소생술 교육",
      desc: "마인드앤매뉴얼의 IoT교구재를 이용한 시그니처 프로그램으로 주요 연령별 심폐소생술을 선진적인 실습방식으로 배울 수 있습니다.",
      links: [{ label: "교육 신청", href: "/smart" }],
    },
    {
      img: "https://cdn.imweb.me/thumbnail/20230814/0b35b5161efc9.png",
      badge: "개인 / 단체 대상",
      title: "KACPR 일반인 심폐소생술 교육",
      desc: "대한심폐소생협회 표준프로그램을 이용한 실습 중심의 심폐소생술 교육과정으로 기초/심화과정으로 구성되어 있습니다.",
      links: [
        { label: "개인 교육 신청", href: "/cpr-person" },
        { label: "단체 교육 신청", href: "/cpr-Group" },
      ],
    },
    {
      img: "https://cdn.imweb.me/thumbnail/20230814/88dd96a328060.png",
      badge: "교직원 / 청소년 대상",
      title: "교직원 · 청소년 응급처치 교육",
      desc: "초·중·고 교직원/청소년 대상의 법정의무 교육과정으로 교내에서 발생할 수 있는 사고에 대한 주요 응급처치술을 배울 수 있습니다.",
      links: [
        { label: "교직원 교육 신청", href: "/teaching-staff" },
        { label: "청소년 교육 신청", href: "/teenager" },
      ],
    },
    {
      img: "https://cdn.imweb.me/thumbnail/20230814/fe49bdb591b15.png",
      badge: "어린이 이용시설 종사자 대상",
      title: "어린이 안전교육",
      desc: "어린이이용시설 종사자 대상의 법정의무 교육과정으로 현장에서 발생할 수 있는 사고에 대한 주요 응급처치술을 배울 수 있습니다.",
      links: [{ label: "교육 신청", href: "/child" }],
    },
  ];
</script>

<section id="programs" class="programs-section py-section">
  <div class="container">
    <div class="section-header animate-slide-up">
      <span class="sub-title">Training Programs</span>
      <h2 class="title">프로그램 소개</h2>
      <p class="desc-text">
        안전을 지키는 확실한 방법, 마인드앤매뉴얼의 전문 교육 커리큘럼입니다.
      </p>
    </div>

    <div class="programs-grid">
      {#each programs as prog, idx}
        <div
          class="program-card animate-slide-up"
          style="animation-delay: {idx * 0.15}s"
        >
          <div
            class="card-bg"
            style="background-image: linear-gradient(to bottom, rgba(0,0,0,0.2) 0%, rgba(0,0,0,0.85) 90%), url({prog.img})"
          ></div>

          <div class="card-content">
            <span class="card-badge">{prog.badge}</span>
            <h3 class="card-title">{prog.title}</h3>
            <p class="card-desc">{prog.desc}</p>

            <div class="card-actions">
              {#each prog.links as link}
                <a href={link.href} class="btn-card">
                  <svg
                    width="16"
                    height="16"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    class="arrow-icon"
                  >
                    <circle cx="12" cy="12" r="10" />
                    <path d="M12 8l4 4-4 4M8 12h8" />
                  </svg>
                  <span>{link.label}</span>
                </a>
              {/each}
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>
</section>

<style>
  .programs-section {
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

  .programs-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 24px;
  }

  .program-card {
    position: relative;
    border-radius: 20px;
    overflow: hidden;
    height: 480px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    transition:
      transform 0.4s cubic-bezier(0.16, 1, 0.3, 1),
      box-shadow 0.4s ease;
  }

  .program-card:hover {
    transform: translateY(-8px);
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
  }

  .card-bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
    z-index: 1;
  }

  .program-card:hover .card-bg {
    transform: scale(1.08);
  }

  .card-content {
    position: relative;
    z-index: 2;
    padding: 30px 24px;
    color: #ffffff;
    display: flex;
    flex-direction: column;
    height: 100%;
    justify-content: flex-end;
  }

  .card-badge {
    align-self: flex-start;
    background-color: var(--primary);
    color: #ffffff;
    font-size: 11px;
    font-weight: 600;
    padding: 6px 14px;
    border-radius: 30px;
    margin-bottom: 15px;
    text-transform: uppercase;
  }

  .card-title {
    font-size: 22px;
    font-weight: 700;
    line-height: 1.3;
    margin-bottom: 12px;
    word-break: keep-all;
  }

  .card-desc {
    font-size: 14px;
    line-height: 1.5;
    color: rgba(255, 255, 255, 0.8);
    margin-bottom: 24px;
    display: -webkit-box;
    -webkit-line-clamp: 4;
    -webkit-box-orient: vertical;
    overflow: hidden;
    word-break: keep-all;
  }

  .card-actions {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .btn-card {
    display: flex;
    align-items: center;
    gap: 8px;
    color: #ffffff;
    font-size: 13px;
    font-weight: 600;
    padding: 8px 0;
    transition: color 0.2s ease;
  }

  .btn-card:hover {
    color: var(--primary);
  }

  .arrow-icon {
    transition: transform var(--transition-speed);
  }

  .btn-card:hover .arrow-icon {
    transform: translateX(4px);
  }

  /* Responsive sizes */
  @media (max-width: 1200px) {
    .programs-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 640px) {
    .programs-grid {
      grid-template-columns: 1fr;
    }

    .program-card {
      height: 420px;
    }

    .title {
      font-size: 28px;
    }
  }
</style>
