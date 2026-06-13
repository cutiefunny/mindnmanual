<script>
  import { onMount } from 'svelte';

  let scrolled = $state(false);
  let mobileMenuOpen = $state(false);
  let activeDropdown = $state(null);

  // Nav items list
  const menuItems = [
    { name: '브랜드 소개', href: '/brand' },
    {
      name: '교육 신청',
      href: '/Application',
      children: [
        { name: '스마트 심폐소생술 교육', href: '/smart' },
        { name: '대한심폐소생협회 일반인 심폐소생술 교육(단체)', href: '/cpr-Group' },
        { name: '대한심폐소생협회 일반인 심폐소생술 교육(개인)', href: '/cpr-person' },
        { name: '교직원 응급처치 교육', href: '/teaching-staff' },
        { name: '청소년 심폐소생술 교육', href: '/teenager' },
        { name: '어린이 안전교육', href: '/child' },
        { name: '온라인 교육', href: '/online' }
      ]
    },
    { name: '강사 지원', href: '/Instructor-apply' },
    { name: '파트너십 / 제휴', href: '/partnership' }
  ];

  onMount(() => {
    const handleScroll = () => {
      scrolled = window.scrollY > 20;
    };
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  });

  function toggleMobileMenu() {
    mobileMenuOpen = !mobileMenuOpen;
  }

  function handleDropdownClick(index) {
    if (activeDropdown === index) {
      activeDropdown = null;
    } else {
      activeDropdown = index;
    }
  }
</script>

<header class="header {scrolled ? 'scrolled' : ''}">
  <div class="container header-container">
    <!-- Mobile Hamburger Menu Button -->
    <button 
      class="mobile-toggle-btn" 
      onclick={toggleMobileMenu} 
      aria-label="Toggle Navigation Menu"
    >
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M3 12h18M3 6h18M3 18h18" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>

    <!-- Logo -->
    <a href="/" class="logo">
      <img src="/logo.png" alt="마인드앤매뉴얼" />
    </a>

    <!-- Desktop Navigation Menu -->
    <nav class="desktop-nav">
      <ul class="nav-list">
        {#each menuItems as item, idx}
          <li class="nav-item {item.children ? 'has-dropdown' : ''}">
            {#if item.children}
              <a 
                href={item.href}
                class="nav-link dropdown-toggle" 
              >
                {item.name}
                <svg class="chevron" width="10" height="6" viewBox="0 0 10 6" fill="none">
                  <path d="M1 1l4 4 4-4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </a>
              <ul class="dropdown-menu">
                {#each item.children as child}
                  <li>
                    <a href={child.href} class="dropdown-link">{child.name}</a>
                  </li>
                {/each}
              </ul>
            {:else}
              <a href={item.href} class="nav-link">{item.name}</a>
            {/if}
          </li>
        {/each}
      </ul>
    </nav>

    <!-- Contact & Consultation Button -->
    <div class="contact-info">
      <span class="consultation-label">전국 교육상담</span>
      <a href="tel:1660-3707" class="phone-link">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" class="phone-icon">
          <path d="M6.62 10.79a15.15 15.15 0 006.59 6.59l2.2-2.2a1 1 0 011.11-.27 11.72 11.72 0 003.7.59 1 1 0 011 1V20a1 1 0 01-1 1A17 17 0 013 4a1 1 0 011-1h3.5a1 1 0 011 1 11.72 11.72 0 00.59 3.7 1 1 0 01-.27 1.1l-2.2 2.2z"/>
        </svg>
        <span>1660-3707</span>
      </a>
    </div>
  </div>
</header>

<!-- Mobile Navigation Drawer -->
<div class="mobile-drawer {mobileMenuOpen ? 'open' : ''}">
  <div class="drawer-header">
    <a href="/" class="logo">
      <img src="/logo.png" alt="마인드앤매뉴얼" />
    </a>
    <button class="close-btn" onclick={toggleMobileMenu} aria-label="Close menu">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M18 6L6 18M6 6l12 12" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
  </div>
  <nav class="mobile-nav-menu">
    <ul class="mobile-nav-list">
      {#each menuItems as item, idx}
        <li class="mobile-nav-item">
          {#if item.children}
            <button 
              class="mobile-nav-link dropdown-btn {activeDropdown === idx ? 'active' : ''}" 
              onclick={() => handleDropdownClick(idx)}
            >
              {item.name}
              <svg class="chevron" width="12" height="8" viewBox="0 0 10 6" fill="none">
                <path d="M1 1l4 4 4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </button>
            <ul class="mobile-dropdown-menu" style="display: {activeDropdown === idx ? 'block' : 'none'}">
              {#each item.children as child}
                <li>
                  <a href={child.href} class="mobile-dropdown-link" onclick={toggleMobileMenu}>{child.name}</a>
                </li>
              {/each}
            </ul>
          {:else}
            <a href={item.href} class="mobile-nav-link" onclick={toggleMobileMenu}>{item.name}</a>
          {/if}
        </li>
      {/each}
    </ul>
  </nav>
  <div class="mobile-drawer-footer">
    <p class="tel-label">전국 교육상담</p>
    <a href="tel:1660-3707" class="tel-number">1660-3707</a>
  </div>
</div>
<!-- Backdrop for mobile drawer -->
{#if mobileMenuOpen}
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <div class="mobile-backdrop" onclick={toggleMobileMenu}></div>
{/if}

<style>
  .header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 1000;
    background-color: transparent;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    transition: all 0.3s ease;
  }

  .header.scrolled {
    background-color: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
    border-bottom: 1px solid var(--border-color);
  }

  .header-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 90px;
    transition: height 0.3s ease;
  }

  .header.scrolled .header-container {
    height: 70px;
  }

  .logo img {
    height: 26px;
    width: auto;
    transition: height 0.3s ease;
  }

  .header.scrolled .logo img {
    height: 20px;
  }

  .desktop-nav {
    display: block;
  }

  .nav-list {
    display: flex;
    gap: 32px;
  }

  .nav-item {
    position: relative;
  }

  .nav-link {
    font-size: 16px;
    font-weight: 500;
    color: var(--dark);
    padding: 10px 0;
    cursor: pointer;
    background: none;
    border: none;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .nav-link:hover, .nav-item:hover > .nav-link {
    color: var(--primary);
  }

  /* Dropdown Menu styling */
  .chevron {
    transition: transform var(--transition-speed);
  }

  .nav-item:hover .chevron {
    transform: rotate(180deg);
  }

  .dropdown-menu {
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translateX(-50%) translateY(10px);
    background-color: var(--bg-white);
    min-width: 260px;
    border-radius: 12px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
    opacity: 0;
    visibility: hidden;
    transition: all 0.3s ease;
    padding: 10px 0;
  }

  .nav-item:hover .dropdown-menu {
    opacity: 1;
    visibility: visible;
    transform: translateX(-50%) translateY(0);
  }

  .dropdown-link {
    display: block;
    padding: 12px 20px;
    font-size: 14px;
    color: #555555;
    font-weight: 400;
    white-space: normal;
  }

  .dropdown-link:hover {
    background-color: var(--bg-light);
    color: var(--primary);
  }

  /* Contact consultation area */
  .contact-info {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .consultation-label {
    font-size: 14px;
    color: var(--text-muted);
    font-weight: 500;
  }

  .phone-link {
    display: flex;
    align-items: center;
    gap: 6px;
    background-color: var(--primary);
    color: var(--bg-white);
    padding: 10px 18px;
    border-radius: 30px;
    font-size: 14px;
    font-weight: 600;
    box-shadow: 0 4px 15px rgba(255, 121, 0, 0.2);
  }

  .phone-link:hover {
    background-color: var(--primary-hover);
    transform: translateY(-2px);
  }

  .phone-icon {
    width: 16px;
    height: 16px;
  }

  /* Mobile layout styling */
  .mobile-toggle-btn {
    display: none;
    background: none;
    border: none;
    color: var(--dark);
    cursor: pointer;
  }

  .mobile-drawer {
    position: fixed;
    top: 0;
    left: -320px;
    width: 300px;
    height: 100%;
    background-color: var(--bg-white);
    z-index: 1002;
    box-shadow: 10px 0 30px rgba(0, 0, 0, 0.1);
    transition: left 0.3s ease;
    display: flex;
    flex-direction: column;
    padding: 24px;
  }

  .mobile-drawer.open {
    left: 0;
  }

  .drawer-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 30px;
  }

  .drawer-header .logo img {
    height: 36px;
  }

  .close-btn {
    background: none;
    border: none;
    color: var(--dark);
    cursor: pointer;
  }

  .mobile-nav-menu {
    flex-grow: 1;
    overflow-y: auto;
  }

  .mobile-nav-list {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .mobile-nav-link {
    width: 100%;
    text-align: left;
    background: none;
    border: none;
    font-size: 16px;
    font-weight: 500;
    color: var(--dark);
    padding: 10px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
  }

  .mobile-nav-link.active .chevron {
    transform: rotate(180deg);
  }

  .mobile-dropdown-menu {
    padding-left: 15px;
    border-left: 2px solid var(--border-color);
    margin-top: 8px;
    display: none;
  }

  .mobile-dropdown-link {
    display: block;
    padding: 8px 0;
    font-size: 14px;
    color: var(--text-muted);
  }

  .mobile-drawer-footer {
    border-top: 1px solid var(--border-color);
    padding-top: 20px;
  }

  .tel-label {
    font-size: 12px;
    color: var(--text-muted);
    margin-bottom: 4px;
  }

  .tel-number {
    font-size: 20px;
    font-weight: 700;
    color: var(--primary);
  }

  .mobile-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(4px);
    z-index: 1001;
  }

  /* Responsive styling */
  @media (max-width: 992px) {
    .desktop-nav, .consultation-label {
      display: none;
    }

    .mobile-toggle-btn {
      display: block;
    }

    .header-container {
      height: 70px;
    }

    .logo img {
      height: 20px;
    }
  }
</style>
