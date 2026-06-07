<script>
  const socialActions = [
    {
      name: 'KakaoTalk',
      href: 'http://pf.kakao.com/_hyxfuG',
      color: '#fee500',
      icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="currentColor"><path d="M12 3c-4.97 0-9 3.185-9 7.115 0 2.557 1.707 4.8 4.27 6.054-.188.702-.68 2.531-.777 2.947-.118.5-.422 1.67.202 1.25.437-.296 2.512-1.748 3.518-2.446C10.74 18.068 11.36 18.1 12 18.1c4.97 0 9-3.186 9-7.115C21 6.185 16.97 3 12 3z"/></svg>`,
      textColor: '#3c1e1e'
    },
    {
      name: 'Instagram',
      href: 'https://www.instagram.com/mind_manual/',
      color: '#e1306c',
      icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5" ry="5"/><path d="M16 11.37A4 4 0 1112.63 8 4 4 0 0116 11.37zM17.5 6.5h.01"/></svg>`,
      textColor: '#ffffff'
    },
    {
      name: 'Naver Blog',
      href: 'https://blog.naver.com/mind_manual',
      color: '#2db400',
      icon: `<span style="font-family: sans-serif; font-weight: 800; font-size: 16px;">B</span>`,
      textColor: '#ffffff'
    },
    {
      name: 'Naver Band',
      href: 'https://band.us/band/79055830',
      color: '#22b14c',
      icon: `<span style="font-family: sans-serif; font-weight: 800; font-size: 15px;">BAND</span>`,
      textColor: '#ffffff'
    }
  ];

  let showScrollTop = $state(false);

  function scrollToTop() {
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  // Scroll visibility handle
  import { onMount } from 'svelte';
  onMount(() => {
    const checkScroll = () => {
      showScrollTop = window.scrollY > 300;
    };
    window.addEventListener('scroll', checkScroll);
    return () => window.removeEventListener('scroll', checkScroll);
  });
</script>

<div class="floating-container">
  <!-- Social Media shortcuts -->
  <div class="social-pills-list">
    {#each socialActions as action}
      <a 
        href={action.href} 
        target="_blank" 
        rel="noopener noreferrer" 
        class="floating-pill" 
        style="--bg-color: {action.color}; --text-color: {action.textColor}"
        aria-label="Visit {action.name}"
      >
        {@html action.icon}
        <span class="tooltip">{action.name}</span>
      </a>
    {/each}
  </div>

  <!-- Scroll To Top button -->
  {#if showScrollTop}
    <button 
      class="scroll-top-btn animate-fade-in" 
      onclick={scrollToTop} 
      aria-label="Scroll to top"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M18 15l-6-6-6 6" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
  {/if}
</div>

<style>
  .floating-container {
    position: fixed;
    right: 24px;
    bottom: 40px;
    display: flex;
    flex-direction: column;
    gap: 12px;
    z-index: 999;
  }

  .social-pills-list {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .floating-pill {
    position: relative;
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background-color: var(--bg-color);
    color: var(--text-color);
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  }

  .floating-pill:hover {
    transform: scale(1.1) translateY(-2px);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
  }

  /* Tooltip overlay on hover */
  .tooltip {
    position: absolute;
    right: 60px;
    background-color: var(--dark);
    color: #ffffff;
    padding: 6px 12px;
    border-radius: 6px;
    font-size: 12px;
    font-weight: 500;
    white-space: nowrap;
    opacity: 0;
    visibility: hidden;
    transition: all 0.2s ease;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }

  .tooltip::after {
    content: '';
    position: absolute;
    top: 50%;
    right: -4px;
    transform: translateY(-50%) rotate(45deg);
    width: 8px;
    height: 8px;
    background-color: var(--dark);
  }

  .floating-pill:hover .tooltip {
    opacity: 1;
    visibility: visible;
    right: 56px;
  }

  /* Scroll To Top button */
  .scroll-top-btn {
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background-color: var(--bg-white);
    color: var(--dark);
    border: 1px solid var(--border-color);
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
    transition: all 0.3s ease;
  }

  .scroll-top-btn:hover {
    background-color: var(--primary);
    color: #ffffff;
    border-color: var(--primary);
    transform: translateY(-2px);
  }

  /* Responsive styling */
  @media (max-width: 768px) {
    .floating-container {
      right: 16px;
      bottom: 24px;
    }

    .floating-pill, .scroll-top-btn {
      width: 42px;
      height: 42px;
    }

    .tooltip {
      display: none; /* Hide tooltips on mobile */
    }
  }
</style>
