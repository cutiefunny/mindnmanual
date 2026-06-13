<script>

  const generalCourses = [
    {
      img: 'https://cdn.imweb.me/thumbnail/20230830/a2b65b7351d57.png',
      title: '스마트 심폐소생술 교육',
      desc: '마인드앤매뉴얼의 IoT교구재를 이용한 시그니처 프로그램으로 실시간 피드백을 통해 정확한 가슴압박과 실습 방식을 교육합니다.',
      href: '/smart'
    },
    {
      img: 'https://cdn.imweb.me/thumbnail/20230830/8517d3d650ef8.png',
      title: '대한심폐소생협회 일반인 심폐소생술 교육 (단체)',
      desc: '단체 및 기업 맞춤형 대한심폐소생협회(KACPR) 표준 가이드라인 프로그램으로 실습 중심의 수료증 발급 과정입니다.',
      href: '/cpr-Group'
    },
    {
      img: 'https://cdn.imweb.me/thumbnail/20230830/af21f4e0c9f20.png',
      title: '대한심폐소생협회 일반인 심폐소생술 교육 (개인)',
      desc: '개인 신청자를 대상으로 일정에 맞춰 정기적으로 개설되는 대한심폐소생협회(KACPR) 공인 교육 과정입니다.',
      href: '/cpr-person'
    },
    {
      img: 'https://img.youtube.com/vi/q7J2T6MFA9g/maxresdefault.jpg',
      title: '온라인 심폐소생술 및 응급처치 교육',
      desc: '시간과 공간의 제약 없이 언제 어디서나 동영상을 통해 쉽고 체계적으로 배울 수 있는 온라인 기초 교육 프로그램입니다.',
      href: '/online'
    }
  ];

  const mandatoryCourses = [
    {
      img: 'https://cdn.imweb.me/thumbnail/20230830/da1d15a287c7c.png',
      title: '교직원 응급처치 교육',
      desc: '초·중·고 교직원 대상 법정 의무 연수로 교내 안전사고 대비 및 응급상황 대처 능력을 기르는 실습 교육입니다.',
      href: '/teaching-staff'
    },
    {
      img: 'https://cdn.imweb.me/thumbnail/20230830/175796f218b10.png',
      title: '청소년 심폐소생술 교육',
      desc: '초·중·고 학생들의 연령과 눈높이에 맞추어 친숙하고 흥미롭게 심폐소생술 원리와 실습을 진행하는 교육입니다.',
      href: '/teenager'
    },
    {
      img: 'https://cdn.imweb.me/thumbnail/20230908/a78d003b0b99b.png',
      title: '어린이 안전교육',
      desc: '어린이이용시설 종사자 대상 행정안전부 고시 지정 법정 의무 교육과정으로 심폐소생술 및 응급처치 실습을 제공합니다.',
      href: '/child'
    }
  ];
</script>

<svelte:head>
  <title>교육 신청 | 마인드앤매뉴얼</title>
  <meta name="description" content="마인드앤매뉴얼의 전문 교육 신청 페이지. 스마트 심폐소생술 교육, 일반인 심폐소생술 교육, 교직원·청소년 응급처치 교육, 어린이 안전교육 등 다양한 교육을 신청하세요." />
</svelte:head>



<div class="page-container">
  <!-- Section 1: 일반 교육과정 -->
  <section class="courses-section">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">
          <span class="orange-prefix">I</span> 일반 교육과정
        </h2>
      </div>

      <div class="courses-grid animate-slide-up">
        {#each generalCourses as course}
          <div class="course-card">
            <div class="course-img-box">
              <img src={course.img} alt={course.title} />
            </div>
            <div class="course-info">
              <h3 class="course-title">{course.title}</h3>
              <p class="course-desc">{course.desc}</p>
              <a href={course.href} class="course-apply-btn">
                <span>자세히 보기 및 신청</span>
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <line x1="5" y1="12" x2="19" y2="12"></line>
                  <polyline points="12 5 19 12 12 19"></polyline>
                </svg>
              </a>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- Section 2: 법정의무 교육과정 -->
  <section class="courses-section">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">
          <span class="orange-prefix">I</span> 법정의무 교육과정
        </h2>
      </div>

      <div class="courses-grid animate-slide-up">
        {#each mandatoryCourses as course}
          <div class="course-card">
            <div class="course-img-box">
              <img src={course.img} alt={course.title} />
            </div>
            <div class="course-info">
              <h3 class="course-title">{course.title}</h3>
              <p class="course-desc">{course.desc}</p>
              <a href={course.href} class="course-apply-btn">
                <span>자세히 보기 및 신청</span>
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <line x1="5" y1="12" x2="19" y2="12"></line>
                  <polyline points="12 5 19 12 12 19"></polyline>
                </svg>
              </a>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>
</div>



<style>
  .page-container {
    padding-top: 130px; /* Offset for fixed header */
    background-color: var(--bg-white);
  }

  .courses-section {
    padding: 40px 0 60px;
  }

  /* Section Headers */
  .section-header {
    margin-bottom: 30px;
    border-bottom: 1px solid var(--border-color);
    padding-bottom: 15px;
  }

  .section-title {
    font-size: 20px;
    font-weight: 700;
    color: var(--dark);
    letter-spacing: -0.5px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .orange-prefix {
    color: var(--primary);
    font-weight: 800;
  }

  /* Courses Grid */
  .courses-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 30px;
    max-width: 1200px;
    margin: 0 auto;
  }

  .course-card {
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 20px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    box-shadow: 0 5px 20px rgba(0, 0, 0, 0.02);
    transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1), box-shadow 0.4s ease;
  }

  .course-card:hover {
    transform: translateY(-8px);
    box-shadow: 0 15px 35px rgba(0, 0, 0, 0.08);
  }

  .course-img-box {
    width: 100%;
    aspect-ratio: 16 / 9;
    overflow: hidden;
    position: relative;
  }

  .course-img-box img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
  }

  .course-card:hover .course-img-box img {
    transform: scale(1.05);
  }

  .course-info {
    padding: 30px;
    display: flex;
    flex-direction: column;
    flex-grow: 1;
  }

  .course-title {
    font-size: 18px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 12px;
    line-height: 1.4;
    word-break: keep-all;
  }

  .course-desc {
    font-size: 14px;
    line-height: 1.6;
    color: var(--text-muted);
    margin-bottom: 24px;
    flex-grow: 1;
    word-break: keep-all;
  }

  .course-apply-btn {
    display: inline-flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    border-top: 1px solid var(--border-color);
    padding-top: 20px;
    font-size: 14px;
    font-weight: 600;
    color: var(--dark);
    transition: color 0.3s ease;
  }

  .course-card:hover .course-apply-btn {
    color: var(--primary);
  }

  .course-apply-btn svg {
    transition: transform 0.3s ease;
  }

  .course-card:hover .course-apply-btn svg {
    transform: translateX(6px);
  }

  /* Responsive Styling */
  @media (max-width: 992px) {
    .courses-grid {
      grid-template-columns: repeat(2, 1fr);
      gap: 24px;
    }
  }

  @media (max-width: 768px) {
    .page-container {
      padding-top: 100px;
    }

    .courses-section {
      padding: 20px 0 40px;
    }

    .section-title {
      font-size: 18px;
    }

    .courses-grid {
      grid-template-columns: 1fr;
      max-width: 450px;
    }


  }
</style>
