<script>
  const certs = [
    {
      img: 'https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/72cc169227f59.png',
      title: '안전교육기관 지정',
      desc: '행정안전부 지정 (제2022-4호)'
    },
    {
      img: 'https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/37d5abcb1bfbd.png',
      title: '어린이안전교육 전문기관',
      desc: '행정안전부 지정 (제2022-114호)'
    },
    {
      img: 'https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/330b2b5261506.png',
      title: '일반인 심폐소생술 교육기관',
      desc: '대한심폐소생협회 지정 (제31호)'
    }
  ];
</script>

<section class="certifications-section animate-slide-up">
  <div class="container certs-container">
    <div class="certs-intro">
      <p class="intro-tag">Accredited Institution</p>
      <h2 class="intro-title">
        마인드앤매뉴얼은 행정안전부 및 대한심폐소생협회 인증 교육기관으로,
        건강하고 안전한 생활을 위한 기초의료기술을 올바르게 전달합니다.
      </h2>
    </div>
    
    <div class="certs-grid">
      {#each certs as cert}
        <div class="cert-card">
          <div class="cert-icon-wrapper">
            <img src={cert.img} alt={cert.title} />
          </div>
          <div class="cert-text">
            <h3 class="cert-title">{cert.title}</h3>
            <p class="cert-desc">{cert.desc}</p>
          </div>
        </div>
      {/each}
    </div>
  </div>
</section>

<style>
  .certifications-section {
    padding: 60px 0;
    background-color: var(--bg-white);
  }

  .certs-container {
    display: flex;
    flex-direction: column;
    gap: 40px;
  }

  .certs-intro {
    text-align: center;
    max-width: 900px;
    margin: 0 auto;
  }

  .intro-tag {
    font-size: 13px;
    font-weight: 600;
    color: var(--primary);
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 12px;
  }

  .intro-title {
    font-size: 20px;
    font-weight: 500;
    line-height: 1.6;
    color: var(--dark);
    word-break: keep-all;
  }

  .certs-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 24px;
    background-color: var(--bg-light);
    padding: 30px;
    border-radius: 20px;
    border: 1px solid rgba(0, 0, 0, 0.03);
  }

  .cert-card {
    background-color: var(--bg-white);
    padding: 24px;
    border-radius: 16px;
    display: flex;
    align-items: center;
    gap: 16px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.02);
    transition: transform var(--transition-speed), box-shadow var(--transition-speed);
  }

  .cert-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
  }

  .cert-icon-wrapper {
    flex-shrink: 0;
    width: 60px;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .cert-icon-wrapper img {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
  }

  .cert-text {
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  .cert-title {
    font-size: 16px;
    font-weight: 700;
    color: var(--dark);
  }

  .cert-desc {
    font-size: 13px;
    color: var(--text-muted);
  }

  /* Responsive layout */
  @media (max-width: 992px) {
    .certs-grid {
      grid-template-columns: 1fr;
      padding: 20px;
    }

    .intro-title {
      font-size: 16px;
    }
  }
</style>
