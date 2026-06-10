<script>
  import Certifications from '$lib/components/Certifications.svelte';

  // Form states
  let selectedCourse = $state('');
  let groupName = $state('');
  let address = $state('');
  let hopeDate = $state('');
  let headcount = $state('');
  let contactName = $state('');
  let phone1 = $state('010');
  let phone2 = $state('');
  let phone3 = $state('');
  let email = $state('');
  let message = $state('');
  let agreePrivacy = $state(false);

  let showPrivacyModal = $state(false);
  let formSubmitted = $state(false);

  // Courses list matching live site dropdown
  const courses = [
    '스마트 심폐소생술 교육',
    '대한심폐소생협회 일반인 심폐소생술 교육 (단체)',
    '대한심폐소생협회 일반인 심폐소생술 교육 (개인)',
    '교직원 응급처치 교육',
    '청소년 심폐소생술 교육',
    '어린이 안전교육'
  ];

  function handleSubmit(event) {
    event.preventDefault();
    if (!agreePrivacy) {
      alert('개인정보 수집 및 이용에 동의해주셔야 신청이 가능합니다.');
      return;
    }

    const fullPhone = `${phone1}-${phone2}-${phone3}`;
    const payload = {
      course: selectedCourse,
      groupName,
      address,
      hopeDate,
      headcount,
      contactName,
      phone: fullPhone,
      email,
      message
    };

    console.log('Submitted Application Data:', payload);
    formSubmitted = true;
  }

  function togglePrivacyModal() {
    showPrivacyModal = !showPrivacyModal;
  }

  function resetForm() {
    selectedCourse = '';
    groupName = '';
    address = '';
    hopeDate = '';
    headcount = '';
    contactName = '';
    phone1 = '010';
    phone2 = '';
    phone3 = '';
    email = '';
    message = '';
    agreePrivacy = false;
    formSubmitted = false;
  }
</script>

<svelte:head>
  <title>교육 신청 문의 | 마인드앤매뉴얼</title>
  <meta name="description" content="마인드앤매뉴얼의 전문 교육 과정 신청 및 문의 페이지. 필요하신 교육 정보를 입력하시면 신속하게 상담 및 일정을 조율해 드립니다." />
</svelte:head>



<div class="contact-page-container">
  <div class="container main-layout">
    
    <!-- Left Column: Guidelines -->
    <div class="guidelines-column animate-slide-up">
      <div class="brand-row">
        <h1 class="main-title">마인드앤매뉴얼<br/>교육 신청</h1>
        <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/3a2bfa8ae5888.png" alt="we learn miracles" class="brand-badge-img" />
      </div>

      <div class="guide-section">
        <h2 class="guide-title">I 신청 방법</h2>
        <ul class="guide-list">
          <li>신청양식에 따라 상세하게 정보를 기재하여 신청 접수</li>
          <li>신청 완료 시 마인드앤매뉴얼 담당자 연락을 통해 교육과정 상담 진행</li>
          <li>일정 등 신청 내용 변경 시 유선상담을 통해 변경사항 접수</li>
        </ul>
      </div>

      <div class="guide-section">
        <h2 class="guide-title">I 비용 납입</h2>
        <ul class="guide-list">
          <li>교육 수료 후 30일 이내 납부</li>
          <li>세금계산서 발행 및 카드 결제 가능</li>
        </ul>
      </div>
    </div>

    <!-- Right Column: Interactive Form -->
    <div class="form-column animate-slide-up">
      {#if formSubmitted}
        <div class="submission-success">
          <div class="success-icon-box">
            <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
              <polyline points="20 6 9 17 4 12"></polyline>
            </svg>
          </div>
          <h2>교육 신청 완료</h2>
          <p class="success-desc">
            교육 신청이 정상적으로 접수되었습니다.<br/>
            마인드앤매뉴얼 담당자가 확인 후 영업일 기준 1~2일 이내에 연락드리겠습니다.
          </p>
          <button class="reset-btn" onclick={resetForm}>추가 신청하기</button>
        </div>
      {:else}
        <form onsubmit={handleSubmit} class="application-form">
          
          <!-- 과정명 -->
          <div class="form-item">
            <label for="course" class="form-label">과정명 <span class="dot">*</span></label>
            <div class="select-wrapper">
              <select id="course" class="form-select" bind:value={selectedCourse} required>
                <option value="" disabled selected>(선택)</option>
                {#each courses as course}
                  <option value={course}>{course}</option>
                {/each}
              </select>
            </div>
          </div>

          <!-- 기관명 -->
          <div class="form-item">
            <label for="group-name" class="form-label">기관명 <span class="dot">*</span></label>
            <input type="text" id="group-name" class="form-input" placeholder="예: (주)마인드컴퍼니" bind:value={groupName} required />
          </div>

          <!-- 교육장주소 -->
          <div class="form-item">
            <label for="address" class="form-label">교육장주소 <span class="dot">*</span></label>
            <input type="text" id="address" class="form-input" placeholder="실습을 진행할 상세 주소를 입력해주세요." bind:value={address} required />
          </div>

          <!-- 교육희망일 -->
          <div class="form-item">
            <label for="hope-date" class="form-label">교육희망일 <span class="dot">*</span></label>
            <input type="date" id="hope-date" class="form-input" bind:value={hopeDate} required />
          </div>

          <!-- 교육인원 -->
          <div class="form-item">
            <label for="headcount" class="form-label">교육인원 <span class="dot">*</span></label>
            <input type="number" id="headcount" class="form-input" placeholder="예: 30" min="1" bind:value={headcount} required />
          </div>

          <!-- 담당자 성함 -->
          <div class="form-item">
            <label for="contact-name" class="form-label">담당자 성함 <span class="dot">*</span></label>
            <input type="text" id="contact-name" class="form-input" placeholder="담당자님의 성함을 입력해주세요." bind:value={contactName} required />
          </div>

          <!-- 담당자 연락처 -->
          <div class="form-item">
            <span class="form-label">담당자 연락처 <span class="dot">*</span></span>
            <div class="phone-input-grid">
              <select class="form-select phone-select" bind:value={phone1} required>
                <option value="010">010</option>
                <option value="02">02</option>
                <option value="031">031</option>
                <option value="032">032</option>
                <option value="041">041</option>
                <option value="042">042</option>
                <option value="051">051</option>
                <option value="070">070</option>
              </select>
              <span class="dash">-</span>
              <input type="text" class="form-input text-center" maxlength="4" placeholder="0000" bind:value={phone2} required pattern="[0-9]{3,4}" />
              <span class="dash">-</span>
              <input type="text" class="form-input text-center" maxlength="4" placeholder="0000" bind:value={phone3} required pattern="[0-9]{4}" />
            </div>
          </div>

          <!-- 담당자 이메일 -->
          <div class="form-item">
            <label for="email" class="form-label">담당자 이메일 <span class="dot">*</span></label>
            <input type="email" id="email" class="form-input" placeholder="예: example@domain.com" bind:value={email} required />
          </div>

          <!-- 특이 및 요청사항 -->
          <div class="form-item">
            <label for="message" class="form-label">특이 및 요청사항</label>
            <textarea id="message" class="form-textarea" rows="4" placeholder="교육 일정 조율, 필요 장비, 주차 관련 사항 등 문의 사항을 입력해주세요." bind:value={message}></textarea>
          </div>

          <!-- 개인정보수집동의 -->
          <div class="form-item privacy-consent-row">
            <label class="consent-checkbox-label">
              <input type="checkbox" bind:checked={agreePrivacy} required />
              <span class="custom-checkbox"></span>
              <span class="label-text">개인정보 수집 및 이용 동의 <span class="dot">*</span></span>
            </label>
            <!-- svelte-ignore a11y_invalid_attribute -->
            <a href="javascript:void(0)" class="view-policy-btn" onclick={togglePrivacyModal}>[보기]</a>
          </div>

          <!-- Submit Button -->
          <div class="form-submit-container">
            <button type="submit" class="submit-btn">
              <span>교육 신청하기</span>
            </button>
          </div>

        </form>
      {/if}
    </div>

  </div>
</div>

<!-- Privacy Policy Modal -->
{#if showPrivacyModal}
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <div class="modal-backdrop" onclick={togglePrivacyModal}></div>
  <div class="modal-wrapper animate-zoom-in" role="dialog" aria-modal="true" aria-labelledby="modal-title">
    <div class="modal-header">
      <h2 id="modal-title" class="modal-header-title">개인정보 수집 및 이용 동의</h2>
      <button class="modal-close-btn" onclick={togglePrivacyModal} aria-label="Close modal">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M18 6L6 18M6 6l12 12" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </div>
    
    <div class="modal-body">
      <div class="policy-content">
        <p class="policy-p">마인드앤매뉴얼은 교육 신청 및 안내를 위하여 아래와 같이 개인정보를 수집 및 이용하고 있습니다.</p>
        
        <table class="policy-table">
          <thead>
            <tr>
              <th>수집 목적</th>
              <th>수집 항목</th>
              <th>보유 및 이용 기간</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>교육과정 상담, 일정 조율 및 관련 업무 연계</td>
              <td>기관명, 교육장주소, 교육희망일, 교육인원, 담당자 성함, 연락처, 이메일 주소</td>
              <td class="font-bold">교육 완료 후 1년 또는 정보주체의 동의 철회 시까지</td>
            </tr>
          </tbody>
        </table>

        <p class="policy-p warning-text">* 귀하는 위 개인정보 수집 및 이용에 동의하지 않을 권리가 있으나, 동의하지 않을 경우 교육 신청이 제한될 수 있습니다.</p>
      </div>
    </div>
  </div>
{/if}

<Certifications />


<style>
  .contact-page-container {
    padding-top: 140px;
    padding-bottom: 80px;
    background-color: var(--bg-light);
  }

  .main-layout {
    display: grid;
    grid-template-columns: 1fr 1.3fr;
    gap: 80px;
    max-width: 1200px;
    margin: 0 auto;
    align-items: start;
  }

  /* Left Column: Guidelines */
  .guidelines-column {
    display: flex;
    flex-direction: column;
    gap: 40px;
  }

  .brand-row {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .main-title {
    font-size: 38px;
    font-weight: 700;
    line-height: 1.3;
    color: var(--dark);
    letter-spacing: -1.5px;
  }

  .brand-badge-img {
    width: 90px;
    height: 90px;
    object-fit: contain;
  }

  .guide-section {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .guide-title {
    font-size: 20px;
    font-weight: 700;
    color: var(--dark);
  }

  .guide-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding-left: 0;
  }

  .guide-list li {
    font-size: 15px;
    color: var(--text-muted);
    line-height: 1.6;
    position: relative;
    padding-left: 15px;
    word-break: keep-all;
  }

  .guide-list li::before {
    content: "•";
    color: var(--primary);
    position: absolute;
    left: 0;
    font-size: 18px;
    top: -2px;
  }

  /* Right Column: Form */
  .form-column {
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 24px;
    padding: 40px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
  }

  .application-form {
    display: flex;
    flex-direction: column;
    gap: 22px;
  }

  .form-item {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .form-label {
    font-size: 14px;
    font-weight: 600;
    color: var(--dark);
    display: flex;
    align-items: center;
    gap: 4px;
  }

  .dot {
    color: var(--primary);
    font-weight: 800;
  }

  .form-input, .form-textarea, .form-select {
    width: 100%;
    padding: 12px 16px;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    font-size: 14px;
    color: var(--text);
    background-color: var(--bg-white);
    transition: all 0.2s ease;
  }

  .form-input:focus, .form-textarea:focus, .form-select:focus {
    outline: none;
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(255, 121, 0, 0.1);
  }

  .form-textarea {
    resize: none;
  }

  /* Select wrapper for chevron arrow icon */
  .select-wrapper {
    position: relative;
    width: 100%;
  }

  .select-wrapper::after {
    content: '';
    position: absolute;
    right: 18px;
    top: 50%;
    transform: translateY(-50%) rotate(45deg);
    width: 8px;
    height: 8px;
    border-right: 2px solid var(--text-muted);
    border-bottom: 2px solid var(--text-muted);
    pointer-events: none;
  }

  .form-select {
    appearance: none;
    padding-right: 40px;
  }

  /* Phone Inputs Grid */
  .phone-input-grid {
    display: grid;
    grid-template-columns: 1fr auto 1.2fr auto 1.2fr;
    gap: 8px;
    align-items: center;
  }

  .dash {
    color: var(--text-muted);
    font-size: 14px;
  }

  .phone-select {
    padding-right: 30px;
  }

  /* Privacy Consent Checkbox */
  .privacy-consent-row {
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    margin-top: 10px;
  }

  .consent-checkbox-label {
    display: flex;
    align-items: center;
    gap: 10px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 500;
    color: var(--dark);
    position: relative;
    user-select: none;
  }

  .consent-checkbox-label input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  .custom-checkbox {
    width: 20px;
    height: 20px;
    background-color: var(--bg-white);
    border: 1px solid #dcdcdc;
    border-radius: 4px;
    display: inline-block;
    position: relative;
    transition: all 0.2s ease;
  }

  .consent-checkbox-label:hover input ~ .custom-checkbox {
    border-color: var(--primary);
  }

  .consent-checkbox-label input:checked ~ .custom-checkbox {
    background-color: var(--primary);
    border-color: var(--primary);
  }

  .custom-checkbox::after {
    content: "";
    position: absolute;
    display: none;
    left: 6px;
    top: 2px;
    width: 6px;
    height: 11px;
    border: solid white;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
  }

  .consent-checkbox-label input:checked ~ .custom-checkbox::after {
    display: block;
  }

  .view-policy-btn {
    font-size: 14px;
    color: var(--text-muted);
    font-weight: 500;
    transition: color 0.2s;
  }

  .view-policy-btn:hover {
    color: var(--primary);
  }

  /* Submit Container */
  .form-submit-container {
    margin-top: 15px;
  }

  .submit-btn {
    width: 100%;
    padding: 16px;
    border: none;
    border-radius: 12px;
    background-color: var(--primary);
    color: #ffffff;
    font-size: 16px;
    font-weight: 700;
    font-family: inherit;
    cursor: pointer;
    box-shadow: 0 4px 15px rgba(255, 121, 0, 0.2);
    transition: all 0.2s ease;
  }

  .submit-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-1px);
    box-shadow: 0 6px 20px rgba(255, 121, 0, 0.3);
  }

  /* Submission Success Box */
  .submission-success {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 60px 20px;
    text-align: center;
    gap: 20px;
  }

  .success-icon-box {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    background-color: #e8f5e9;
    color: #2e7d32;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 6px 20px rgba(46, 125, 50, 0.1);
  }

  .submission-success h2 {
    font-size: 24px;
    font-weight: 700;
    color: var(--dark);
  }

  .success-desc {
    font-size: 15px;
    line-height: 1.7;
    color: var(--text-muted);
    word-break: keep-all;
  }

  .reset-btn {
    background-color: var(--bg-light);
    color: var(--text-muted);
    border: 1px solid var(--border-color);
    padding: 12px 30px;
    border-radius: 30px;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
  }

  .reset-btn:hover {
    background-color: #ededed;
    color: var(--dark);
  }

  /* Privacy Modal styling */
  .modal-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(5px);
    z-index: 1001;
  }

  .modal-wrapper {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 90%;
    max-width: 600px;
    max-height: 85vh;
    background-color: var(--bg-white);
    border-radius: 20px;
    box-shadow: 0 20px 50px rgba(0, 0, 0, 0.15);
    z-index: 1002;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 24px;
    border-bottom: 1px solid var(--border-color);
  }

  .modal-header-title {
    font-size: 18px;
    font-weight: 700;
    color: var(--dark);
  }

  .modal-close-btn {
    background: none;
    border: none;
    color: var(--text-muted);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 4px;
    border-radius: 50%;
    transition: background-color 0.2s, color 0.2s;
  }

  .modal-close-btn:hover {
    background-color: var(--bg-light);
    color: var(--dark);
  }

  .modal-body {
    padding: 24px;
    overflow-y: auto;
  }

  .policy-content {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }

  .policy-p {
    font-size: 14px;
    line-height: 1.6;
    color: var(--text);
  }

  .warning-text {
    color: #ff3b30;
    font-size: 13px;
  }

  /* policy table */
  .policy-table {
    width: 100%;
    border-collapse: collapse;
    margin: 10px 0;
  }

  .policy-table th, .policy-table td {
    border: 1px solid #dcdcdc;
    padding: 12px;
    font-size: 13px;
    line-height: 1.6;
    text-align: left;
    word-break: keep-all;
  }

  .policy-table th {
    background-color: var(--bg-light);
    color: var(--dark);
    font-weight: 600;
  }

  .policy-table td {
    color: var(--text);
  }

  .font-bold {
    font-weight: 600;
    color: var(--dark);
  }

  /* Responsive Styling */
  @media (max-width: 992px) {
    .main-layout {
      grid-template-columns: 1fr;
      gap: 50px;
    }

    .guidelines-column {
      align-items: center;
      text-align: center;
    }

    .guide-list li {
      text-align: left;
    }

    .brand-row {
      flex-direction: column;
      text-align: center;
    }
  }

  @media (max-width: 768px) {
    .contact-page-container {
      padding-top: 100px;
      padding-bottom: 50px;
    }

    .main-title {
      font-size: 30px;
    }

    .brand-badge-img {
      width: 70px;
      height: 70px;
    }

    .form-column {
      padding: 24px;
    }

    .consent-checkbox-label {
      font-size: 13px;
    }

    .view-policy-btn {
      font-size: 13px;
    }

    .privacy-consent-row {
      flex-direction: column;
      align-items: flex-start;
      gap: 10px;
    }

    .view-policy-btn {
      align-self: flex-end;
    }

    .phone-input-grid {
      grid-template-columns: 1.2fr auto 1.5fr auto 1.5fr;
    }
  }
</style>
