<script>
  import { onMount } from 'svelte';

  const navs = [
    { name: '프로그램 소개', href: '#programs' },
    { name: '브랜드 소개', href: '#brand' },
    { name: '주요활동', href: '#activities' },
    { name: '트레이닝 센터', href: '#training-center' }
  ];

  let sticky = $state(false);
  let activeSection = $state('');

  onMount(() => {
    const handleScroll = () => {
      // Sticky detection: make sticky when scrolled past the hero header
      const threshold = window.innerHeight - 100;
      sticky = window.scrollY > threshold;

      // Active section detection
      for (const nav of navs) {
        const el = document.querySelector(nav.href);
        if (el) {
          const rect = el.getBoundingClientRect();
          if (rect.top <= 120 && rect.bottom >= 120) {
            activeSection = nav.href;
            break;
          }
        }
      }
    };

    window.addEventListener('scroll', handleScroll);
    handleScroll(); // initial trigger

    return () => window.removeEventListener('scroll', handleScroll);
  });
</script>

<div class="quick-nav-wrapper {sticky ? 'sticky' : ''}">
  <div class="container nav-container">
    <div class="nav-pills">
      {#each navs as nav}
        <a 
          href={nav.href} 
          class="nav-pill {activeSection === nav.href ? 'active' : ''}"
        >
          {nav.name}
        </a>
      {/each}
    </div>
  </div>
</div>

<style>
  .quick-nav-wrapper {
    background-color: var(--bg-light);
    border-top: 1px solid var(--border-color);
    border-bottom: 1px solid var(--border-color);
    padding: 16px 0;
    transition: all 0.3s ease;
    z-index: 900;
  }

  .quick-nav-wrapper.sticky {
    position: fixed;
    top: 70px; /* Right beneath the header */
    left: 0;
    width: 100%;
    background-color: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.03);
    border-top: none;
  }

  /* Make sure header doesn't overlap on scroll on smaller devices */
  @media (max-width: 992px) {
    .quick-nav-wrapper.sticky {
      top: 70px;
    }
  }

  .nav-container {
    display: flex;
    justify-content: center;
  }

  .nav-pills {
    display: flex;
    gap: 12px;
    overflow-x: auto;
    max-width: 100%;
    scrollbar-width: none; /* Firefox */
  }

  .nav-pills::-webkit-scrollbar {
    display: none; /* Safari & Chrome */
  }

  .nav-pill {
    padding: 10px 24px;
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 30px;
    font-size: 14px;
    font-weight: 500;
    color: #555555;
    white-space: nowrap;
    transition: all var(--transition-speed);
  }

  .nav-pill:hover, .nav-pill.active {
    background-color: var(--primary);
    color: var(--bg-white);
    border-color: var(--primary);
    box-shadow: 0 4px 10px rgba(255, 121, 0, 0.2);
  }
</style>
