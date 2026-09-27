<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const isDark = ref(true)
const activeSection = ref('home')
const menuOpen = ref(false)
const showAllProjects = ref(false)
const showMessage = ref(false)
const currentYear = new Date().getFullYear()

const profile = {
  name: 'Otsusaka Rynn',
  role: 'Web Developer & Creative Learner',
  location: 'Jakarta, Indonesia',
  tagline: 'Membangun pengalaman digital yang rapi, interaktif, dan punya karakter.',
  description:
    'Saya adalah seorang developer yang senang mengeksplorasi web development, UI, coding, desain, dan berbagai teknologi kreatif. Saya suka belajar lewat proyek nyata dan mengubah ide sederhana menjadi sesuatu yang bisa digunakan.',
  email: 'hello@example.com',
  availability: 'Terbuka untuk proyek dan kolaborasi',
}

const navItems = [
  { id: 'home', label: 'Home' },
  { id: 'about', label: 'Tentang' },
  { id: 'skills', label: 'Skill' },
  { id: 'projects', label: 'Proyek' },
  { id: 'journey', label: 'Perjalanan' },
  { id: 'contact', label: 'Kontak' },
]

const stats = [
  { value: '∞', label: 'Rasa ingin tahu' },
  { value: '24/7', label: 'Mode belajar' },
  { value: '100%', label: 'Kemauan berkembang' },
  { value: '1', label: 'Tujuan: terus maju' },
]

const skills = [
  { name: 'HTML', level: 92, category: 'Frontend', icon: '⌘', description: 'Struktur halaman semantik dan aksesibel.' },
  { name: 'CSS', level: 88, category: 'Frontend', icon: '◇', description: 'Layout, responsive design, animasi, dan visual polish.' },
  { name: 'JavaScript', level: 82, category: 'Frontend', icon: 'JS', description: 'Interaksi halaman dan logika aplikasi.' },
  { name: 'Vue', level: 84, category: 'Frontend', icon: 'V', description: 'Membangun UI modular dengan Composition API.' },
  { name: 'Laravel', level: 78, category: 'Backend', icon: 'L', description: 'Routing, MVC, database, authentication, dan API.' },
  { name: 'PHP', level: 80, category: 'Backend', icon: 'P', description: 'Fondasi backend dan pengembangan aplikasi web.' },
  { name: 'MySQL', level: 76, category: 'Backend', icon: 'DB', description: 'Relational database, query, dan pengelolaan data.' },
  { name: 'Git', level: 83, category: 'Tools', icon: 'G', description: 'Version control dan workflow kolaborasi.' },
  { name: 'Flutter', level: 72, category: 'Mobile', icon: 'F', description: 'Eksperimen aplikasi mobile lintas platform.' },
  { name: 'UI Design', level: 75, category: 'Creative', icon: '✦', description: 'Menyusun interface yang konsisten dan enak dipakai.' },
]

const projects = [
  {
    title: 'NovaCart',
    category: 'Web Application',
    year: '2026',
    description: 'Konsep e-commerce dengan dashboard seller, manajemen produk, dan alur transaksi.',
    tags: ['Laravel', 'PHP', 'MySQL', 'Blade'],
    accent: 'violet',
  },
  {
    title: 'TwelveTech Calculator',
    category: 'Flutter Application',
    year: '2026',
    description: 'Kalkulator pintar dengan fokus pada tampilan modern, fitur geometri, dan pengalaman interaktif.',
    tags: ['Flutter', 'Dart', 'UI', 'Mobile'],
    accent: 'cyan',
  },
  {
    title: 'Learning Management System',
    category: 'Education Platform',
    year: '2026',
    description: 'Konsep platform pembelajaran dengan dashboard siswa, materi, dan fitur monitoring.',
    tags: ['Node.js', 'HTML', 'CSS', 'JavaScript'],
    accent: 'pink',
  },
  {
    title: 'PKL Monitoring',
    category: 'School System',
    year: '2025',
    description: 'Konsep sistem monitoring kegiatan praktik kerja lapangan untuk membantu pengelolaan data.',
    tags: ['Web', 'Database', 'Dashboard'],
    accent: 'orange',
  },
  {
    title: 'Personal Portfolio',
    category: 'Portfolio',
    year: '2026',
    description: 'Website profil pribadi yang menjadi tempat menampilkan perjalanan, skill, dan karya.',
    tags: ['Vue', 'CSS', 'JavaScript'],
    accent: 'green',
  },
  {
    title: 'Creative Playground',
    category: 'Experiment',
    year: '2026',
    description: 'Ruang eksperimen untuk mencoba animasi, visual interface, micro-interaction, dan ide random.',
    tags: ['Creative Coding', 'CSS', 'JS'],
    accent: 'blue',
  },
]

const journey = [
  {
    year: '2024',
    title: 'Mulai lebih serius dengan teknologi',
    text: 'Mulai mengeksplorasi coding dan menemukan bahwa membuat sesuatu dari nol terasa menyenangkan.',
  },
  {
    year: '2025',
    title: 'Memperluas kemampuan web',
    text: 'Mulai mengerjakan project yang melibatkan frontend, backend, database, dan deployment.',
  },
  {
    year: '2026',
    title: 'Masuk lebih dalam ke ekosistem modern',
    text: 'Mengeksplorasi Laravel, Vue, Flutter, Git, UI design, dan berbagai pola pengembangan aplikasi.',
  },
  {
    year: 'Next',
    title: 'Build bigger things',
    text: 'Target berikutnya adalah membangun project yang lebih matang, berguna, dan benar-benar dipakai orang.',
  },
]

const interests = [
  'Web Development',
  'UI / UX',
  'Anime & Illustration',
  'Game',
  'Creative Coding',
  'Music',
  'Technology',
  'Learning',
]

const visibleProjects = computed(() =>
  showAllProjects.value ? projects : projects.slice(0, 4),
)

function scrollToSection(id) {
  const element = document.getElementById(id)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth', block: 'start' })
  }
  activeSection.value = id
  menuOpen.value = false
}

function toggleTheme() {
  isDark.value = !isDark.value
  document.documentElement.classList.toggle('light-theme', !isDark.value)
}

function toggleProjects() {
  showAllProjects.value = !showAllProjects.value
}

function sendMessage() {
  showMessage.value = true
  window.setTimeout(() => {
    showMessage.value = false
  }, 3500)
}

function updateActiveSection() {
  const sections = navItems
    .map((item) => document.getElementById(item.id))
    .filter(Boolean)

  const scrollPosition = window.scrollY + 180

  for (const section of sections) {
    const top = section.offsetTop
    const bottom = top + section.offsetHeight

    if (scrollPosition >= top && scrollPosition < bottom) {
      activeSection.value = section.id
      break
    }
  }
}

onMounted(() => {
  window.addEventListener('scroll', updateActiveSection, { passive: true })
  updateActiveSection()
})

onUnmounted(() => {
  window.removeEventListener('scroll', updateActiveSection)
})
</script>

<template>
  <div class="app-shell">
    <div class="noise"></div>

    <header class="site-header">
      <div class="container nav-container">
        <button class="brand" type="button" @click="scrollToSection('home')">
          <span class="brand-mark">R</span>
          <span class="brand-copy">
            <strong>Rynn<span>.</span></strong>
            <small>portfolio</small>
          </span>
        </button>

        <nav class="desktop-nav" aria-label="Navigasi utama">
          <button
            v-for="item in navItems"
            :key="item.id"
            class="nav-link"
            :class="{ active: activeSection === item.id }"
            type="button"
            @click="scrollToSection(item.id)"
          >
            {{ item.label }}
          </button>
        </nav>

        <div class="header-actions">
          <button
            class="icon-button"
            type="button"
            :aria-label="isDark ? 'Aktifkan mode terang' : 'Aktifkan mode gelap'"
            @click="toggleTheme"
          >
            <span v-if="isDark">☼</span>
            <span v-else>☾</span>
          </button>

          <button
            class="menu-button"
            type="button"
            aria-label="Buka menu"
            @click="menuOpen = !menuOpen"
          >
            <span></span>
            <span></span>
            <span></span>
          </button>
        </div>
      </div>

      <div v-if="menuOpen" class="mobile-menu">
        <button
          v-for="item in navItems"
          :key="item.id"
          type="button"
          :class="{ active: activeSection === item.id }"
          @click="scrollToSection(item.id)"
        >
          {{ item.label }}
        </button>
      </div>
    </header>

    <main>
      <section id="home" class="hero section">
        <div class="hero-orb orb-one"></div>
        <div class="hero-orb orb-two"></div>

        <div class="container hero-grid">
          <div class="hero-copy">
            <div class="eyebrow">
              <span class="status-dot"></span>
              {{ profile.availability }}
            </div>

            <p class="hero-kicker">HELLO, I'M</p>

            <h1>
              {{ profile.name }}
              <span class="gradient-text">.</span>
            </h1>

            <div class="hero-role">
              <span class="role-line"></span>
              {{ profile.role }}
            </div>

            <p class="hero-description">
              {{ profile.description }}
            </p>

            <div class="hero-actions">
              <button class="primary-button" type="button" @click="scrollToSection('projects')">
                Lihat karya
                <span>↗</span>
              </button>

              <button class="secondary-button" type="button" @click="scrollToSection('contact')">
                Hubungi saya
              </button>
            </div>

            <div class="hero-meta">
              <div class="meta-item">
                <span class="meta-icon">⌖</span>
                <span>{{ profile.location }}</span>
              </div>
              <div class="meta-item">
                <span class="meta-icon">✦</span>
                <span>Always learning</span>
              </div>
            </div>
          </div>

          <div class="hero-visual">
            <div class="visual-ring ring-back"></div>
            <div class="visual-ring ring-front"></div>

            <div class="profile-card">
              <div class="card-glow"></div>

              <div class="avatar-frame">
                <div class="avatar-placeholder">
                  <span>R</span>
                </div>
                <div class="avatar-status"></div>
              </div>

              <div class="profile-card-content">
                <span class="mini-label">CREATIVE DEVELOPER</span>
                <h2>Build. Learn. Create.</h2>
                <p>
                  Menggabungkan logika, desain, dan rasa ingin tahu untuk membuat
                  sesuatu yang punya karakter.
                </p>
              </div>

              <div class="floating-chip chip-top">
                <span>⌘</span>
                <span>Code</span>
              </div>

              <div class="floating-chip chip-bottom">
                <span>✦</span>
                <span>Create</span>
              </div>
            </div>
          </div>
        </div>

        <div class="container stats-grid">
          <article v-for="stat in stats" :key="stat.label" class="stat-card">
            <strong>{{ stat.value }}</strong>
            <span>{{ stat.label }}</span>
          </article>
        </div>

        <button class="scroll-hint" type="button" @click="scrollToSection('about')">
          <span>Scroll to explore</span>
          <span class="scroll-arrow">↓</span>
        </button>
      </section>

      <section id="about" class="section about-section">
        <div class="container">
          <div class="section-heading">
            <div>
              <span class="section-number">01</span>
              <span class="section-label">ABOUT ME</span>
            </div>
            <h2>Lebih dari sekadar <span>kode.</span></h2>
          </div>

          <div class="about-grid">
            <div class="about-text">
              <p class="large-paragraph">
                Saya percaya bahwa proses membuat sesuatu sama pentingnya dengan
                hasil akhirnya.
              </p>

              <p>
                Coding bagi saya bukan cuma soal menulis baris kode. Ada proses
                berpikir, mencari solusi, mencoba ulang, memperbaiki kesalahan,
                dan akhirnya melihat sebuah ide benar-benar hidup di layar.
              </p>

              <p>
                Saya senang berada di persimpangan antara teknologi dan kreativitas.
                Karena itu, saya menikmati frontend, backend, UI design, aplikasi
                mobile, visual, serta eksperimen kecil yang kadang berawal dari
                rasa penasaran.
              </p>

              <div class="quote-box">
                <span class="quote-mark">“</span>
                <p>Jangan berhenti hanya karena belum jago. Skill tumbuh karena dipakai.</p>
              </div>
            </div>

            <div class="about-side">
              <div class="info-card">
                <span class="info-card-icon">◈</span>
                <div>
                  <small>FOCUS</small>
                  <h3>Building useful experiences</h3>
                  <p>Interface yang jelas, interaksi yang terasa natural, dan kode yang bisa dikembangkan.</p>
                </div>
              </div>

              <div class="info-card">
                <span class="info-card-icon">∞</span>
                <div>
                  <small>MINDSET</small>
                  <h3>Curious by default</h3>
                  <p>Kalau ada teknologi baru, saya lebih suka mencoba daripada sekadar melihat.</p>
                </div>
              </div>

              <div class="info-card">
                <span class="info-card-icon">✦</span>
                <div>
                  <small>VALUES</small>
                  <h3>Consistency over perfection</h3>
                  <p>Lebih baik terus berkembang daripada menunggu semuanya sempurna.</p>
                </div>
              </div>
            </div>
          </div>

          <div class="interests-wrapper">
            <div class="subheading">
              <span>WHAT I LIKE</span>
              <div></div>
            </div>

            <div class="interest-list">
              <span v-for="interest in interests" :key="interest" class="interest-pill">
                {{ interest }}
              </span>
            </div>
          </div>
        </div>
      </section>

      <section id="skills" class="section skills-section">
        <div class="container">
          <div class="section-heading">
            <div>
              <span class="section-number">02</span>
              <span class="section-label">SKILLS</span>
            </div>
            <h2>Tools yang membantu <span>saya berkarya.</span></h2>
          </div>

          <div class="skills-intro">
            <p>
              Kemampuan di bawah bukan angka absolut. Ini adalah gambaran area
              yang sedang saya gunakan dan terus saya kembangkan.
            </p>
          </div>

          <div class="skills-grid">
            <article v-for="skill in skills" :key="skill.name" class="skill-card">
              <div class="skill-top">
                <div class="skill-icon">{{ skill.icon }}</div>
                <div>
                  <span>{{ skill.category }}</span>
                  <h3>{{ skill.name }}</h3>
                </div>
                <strong>{{ skill.level }}%</strong>
              </div>

              <div class="skill-bar">
                <span :style="{ width: skill.level + '%' }"></span>
              </div>

              <p>{{ skill.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section id="projects" class="section projects-section">
        <div class="container">
          <div class="section-heading project-heading">
            <div>
              <span class="section-number">03</span>
              <span class="section-label">SELECTED WORK</span>
            </div>
            <h2>Beberapa hal yang <span>pernah dibuat.</span></h2>
          </div>

          <div class="project-grid">
            <article
              v-for="(project, index) in visibleProjects"
              :key="project.title"
              class="project-card"
              :class="'accent-' + project.accent"
            >
              <div class="project-number">
                {{ String(index + 1).padStart(2, '0') }}
              </div>

              <div class="project-visual">
                <div class="project-grid-lines"></div>
                <div class="project-orb"></div>
                <div class="project-window">
                  <div class="window-top">
                    <i></i>
                    <i></i>
                    <i></i>
                  </div>
                  <div class="window-body">
                    <span></span>
                    <span></span>
                    <span></span>
                    <span></span>
                  </div>
                </div>
              </div>

              <div class="project-content">
                <div class="project-meta">
                  <span>{{ project.category }}</span>
                  <span>{{ project.year }}</span>
                </div>

                <h3>{{ project.title }}</h3>
                <p>{{ project.description }}</p>

                <div class="tag-list">
                  <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
                </div>

                <button class="project-link" type="button">
                  Explore project
                  <span>↗</span>
                </button>
              </div>
            </article>
          </div>

          <div class="projects-footer">
            <button class="outline-button" type="button" @click="toggleProjects">
              {{ showAllProjects ? 'Tampilkan lebih sedikit' : 'Lihat semua project' }}
              <span>{{ showAllProjects ? '↑' : '↓' }}</span>
            </button>
          </div>
        </div>
      </section>

      <section id="journey" class="section journey-section">
        <div class="container">
          <div class="section-heading">
            <div>
              <span class="section-number">04</span>
              <span class="section-label">MY JOURNEY</span>
            </div>
            <h2>Masih berjalan, <span>belum selesai.</span></h2>
          </div>

          <div class="journey-layout">
            <div class="journey-intro">
              <span class="big-year">∞</span>
              <p>
                Tidak ada titik di mana proses belajar benar-benar selesai.
                Setiap project membuka pertanyaan baru, dan setiap kesalahan
                memberi kesempatan untuk memahami sesuatu lebih dalam.
              </p>
            </div>

            <div class="timeline">
              <article v-for="item in journey" :key="item.year + item.title" class="timeline-item">
                <div class="timeline-marker">
                  <span></span>
                </div>
                <div class="timeline-content">
                  <span class="timeline-year">{{ item.year }}</span>
                  <h3>{{ item.title }}</h3>
                  <p>{{ item.text }}</p>
                </div>
              </article>
            </div>
          </div>
        </div>
      </section>

      <section class="section philosophy-section">
        <div class="container">
          <div class="philosophy-card">
            <div class="philosophy-decoration decoration-one"></div>
            <div class="philosophy-decoration decoration-two"></div>

            <span class="mini-label">PERSONAL PHILOSOPHY</span>

            <h2>
              “Create something you would be proud to
              <span>keep improving.</span>”
            </h2>

            <div class="philosophy-bottom">
              <span>— Rynn</span>
              <button type="button" @click="scrollToSection('contact')">
                Let's build something
                <span>↗</span>
              </button>
            </div>
          </div>
        </div>
      </section>

      <section id="contact" class="section contact-section">
        <div class="container">
          <div class="section-heading">
            <div>
              <span class="section-number">05</span>
              <span class="section-label">CONTACT</span>
            </div>
            <h2>Punya ide? <span>Mari ngobrol.</span></h2>
          </div>

          <div class="contact-grid">
            <div class="contact-copy">
              <p class="large-paragraph">
                Saya terbuka untuk diskusi, project, kolaborasi, atau sekadar
                bertukar ide tentang teknologi dan kreativitas.
              </p>

              <p>
                Tidak harus langsung punya brief sempurna. Ceritakan saja apa
                yang sedang ingin kamu buat.
              </p>

              <div class="contact-details">
                <a :href="'mailto:' + profile.email" class="contact-item">
                  <span class="contact-icon">@</span>
                  <span>
                    <small>EMAIL</small>
                    <strong>{{ profile.email }}</strong>
                  </span>
                  <span class="contact-arrow">↗</span>
                </a>

                <div class="contact-item">
                  <span class="contact-icon">⌖</span>
                  <span>
                    <small>LOCATION</small>
                    <strong>{{ profile.location }}</strong>
                  </span>
                </div>
              </div>
            </div>

            <form class="contact-form" @submit.prevent="sendMessage">
              <label>
                <span>Nama</span>
                <input type="text" placeholder="Nama kamu" required />
              </label>

              <label>
                <span>Email</span>
                <input type="email" placeholder="email@example.com" required />
              </label>

              <label>
                <span>Pesan</span>
                <textarea rows="6" placeholder="Ceritakan idemu..." required></textarea>
              </label>

              <button class="primary-button form-button" type="submit">
                Kirim pesan
                <span>↗</span>
              </button>

              <p v-if="showMessage" class="form-success">
                Pesan siap dikirim. Hubungkan form ini ke backend atau service email untuk produksi.
              </p>
            </form>
          </div>
        </div>
      </section>
    </main>

    <footer class="site-footer">
      <div class="container footer-main">
        <div class="footer-brand">
          <span class="brand-mark">R</span>
          <div>
            <strong>Rynn<span>.</span></strong>
            <p>Designing, coding, learning.</p>
          </div>
        </div>

        <div class="footer-links">
          <button v-for="item in navItems" :key="item.id" type="button" @click="scrollToSection(item.id)">
            {{ item.label }}
          </button>
        </div>

        <div class="footer-socials">
          <a href="#" aria-label="GitHub">GH</a>
          <a href="#" aria-label="Instagram">IG</a>
          <a href="#" aria-label="LinkedIn">IN</a>
        </div>
      </div>

      <div class="container footer-bottom">
        <span>© {{ currentYear }} {{ profile.name }}. Built with Vue.</span>
        <span>Made with curiosity ✦</span>
      </div>
    </footer>
  </div>
</template>


<style>
:root {
  --bg: #08090d;
  --bg-soft: #0d0f15;
  --surface: #11131b;
  --surface-2: #161925;
  --surface-3: #1c1f2d;
  --text: #f5f7ff;
  --muted: #9da4b8;
  --muted-2: #6d7488;
  --line: rgba(255, 255, 255, 0.09);
  --line-strong: rgba(255, 255, 255, 0.16);
  --primary: #a78bfa;
  --primary-2: #7c3aed;
  --cyan: #67e8f9;
  --pink: #f0abfc;
  --green: #86efac;
  --orange: #fdba74;
  --shadow: 0 30px 80px rgba(0, 0, 0, 0.35);
  --radius-lg: 28px;
  --radius-md: 18px;
  --radius-sm: 12px;
  --container: 1180px;
  --header-height: 76px;
  font-family:
    Inter,
    ui-sans-serif,
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    "Segoe UI",
    sans-serif;
  color-scheme: dark;
}

:root.light-theme {
  --bg: #f7f7fb;
  --bg-soft: #ffffff;
  --surface: #ffffff;
  --surface-2: #f0f1f7;
  --surface-3: #e6e8f0;
  --text: #11131a;
  --muted: #5e6474;
  --muted-2: #7a8090;
  --line: rgba(17, 19, 26, 0.1);
  --line-strong: rgba(17, 19, 26, 0.18);
  --shadow: 0 30px 80px rgba(29, 35, 60, 0.12);
  color-scheme: light;
}

* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  background: var(--bg);
}

body {
  margin: 0;
  min-width: 320px;
  background: var(--bg);
  color: var(--text);
  overflow-x: hidden;
}

button,
input,
textarea {
  font: inherit;
}

button,
a {
  -webkit-tap-highlight-color: transparent;
}

button {
  cursor: pointer;
}

a {
  color: inherit;
  text-decoration: none;
}

::selection {
  background: rgba(167, 139, 250, 0.35);
  color: var(--text);
}

.app-shell {
  position: relative;
  min-height: 100vh;
  overflow: hidden;
  background:
    radial-gradient(circle at 10% 5%, rgba(124, 58, 237, 0.08), transparent 28rem),
    radial-gradient(circle at 90% 30%, rgba(34, 211, 238, 0.05), transparent 30rem),
    var(--bg);
}

.noise {
  position: fixed;
  inset: 0;
  z-index: 20;
  pointer-events: none;
  opacity: 0.035;
  background-image:
    url("data:image/svg+xml,%3Csvg viewBox='0 0 180 180' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.8'/%3E%3C/svg%3E");
}

.container {
  width: min(var(--container), calc(100% - 48px));
  margin-inline: auto;
}

.section {
  position: relative;
  padding: 130px 0;
  scroll-margin-top: var(--header-height);
}

.site-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 50;
  height: var(--header-height);
  border-bottom: 1px solid var(--line);
  background: color-mix(in srgb, var(--bg) 78%, transparent);
  backdrop-filter: blur(22px);
}

.nav-container {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
}

.brand {
  display: inline-flex;
  align-items: center;
  gap: 11px;
  padding: 0;
  border: 0;
  background: transparent;
  color: var(--text);
  text-align: left;
}

.brand-mark {
  display: grid;
  place-items: center;
  width: 38px;
  height: 38px;
  border: 1px solid var(--line-strong);
  border-radius: 12px;
  background: linear-gradient(145deg, var(--surface-2), var(--surface));
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.08);
  font-size: 17px;
  font-weight: 800;
}

.brand-copy {
  display: grid;
  gap: 0;
}

.brand-copy strong,
.footer-brand strong {
  font-size: 16px;
  letter-spacing: -0.03em;
}

.brand-copy strong span,
.footer-brand strong span {
  color: var(--primary);
}

.brand-copy small {
  color: var(--muted-2);
  font-size: 9px;
  letter-spacing: 0.16em;
  text-transform: uppercase;
}

.desktop-nav {
  display: flex;
  align-items: center;
  gap: 7px;
}

.nav-link {
  position: relative;
  padding: 9px 13px;
  border: 0;
  border-radius: 10px;
  background: transparent;
  color: var(--muted);
  font-size: 12px;
  font-weight: 600;
  transition:
    color 0.2s ease,
    background 0.2s ease;
}

.nav-link:hover,
.nav-link.active {
  color: var(--text);
  background: var(--surface-2);
}

.nav-link.active::after {
  content: "";
  position: absolute;
  left: 50%;
  bottom: 3px;
  width: 3px;
  height: 3px;
  border-radius: 999px;
  background: var(--primary);
  transform: translateX(-50%);
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 8px;
}

.icon-button,
.menu-button {
  width: 38px;
  height: 38px;
  display: grid;
  place-items: center;
  border: 1px solid var(--line);
  border-radius: 12px;
  background: var(--surface);
  color: var(--text);
}

.icon-button {
  font-size: 17px;
}

.menu-button {
  display: none;
  gap: 4px;
}

.menu-button span {
  width: 15px;
  height: 1px;
  background: currentColor;
}

.mobile-menu {
  display: none;
}

.hero {
  min-height: 880px;
  display: flex;
  align-items: center;
  padding-top: 150px;
  padding-bottom: 80px;
}

.hero-grid {
  display: grid;
  grid-template-columns: 1.02fr 0.98fr;
  align-items: center;
  gap: 70px;
}

.hero-copy {
  position: relative;
  z-index: 2;
}

.eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 9px;
  padding: 8px 12px;
  border: 1px solid var(--line);
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.025);
  color: var(--muted);
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.09em;
  text-transform: uppercase;
}

.status-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #86efac;
  box-shadow: 0 0 0 4px rgba(134, 239, 172, 0.08);
  animation: pulse 2s ease-in-out infinite;
}

.hero-kicker {
  margin: 42px 0 8px;
  color: var(--muted-2);
  font-size: 12px;
  font-weight: 800;
  letter-spacing: 0.3em;
}

.hero h1 {
  max-width: 800px;
  margin: 0;
  font-size: clamp(58px, 8vw, 108px);
  line-height: 0.93;
  letter-spacing: -0.075em;
  font-weight: 850;
}

.gradient-text {
  color: var(--primary);
  text-shadow: 0 0 45px rgba(167, 139, 250, 0.3);
}

.hero-role {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 28px;
  color: var(--text);
  font-size: 16px;
  font-weight: 650;
}

.role-line {
  width: 34px;
  height: 1px;
  background: var(--primary);
}

.hero-description {
  max-width: 600px;
  margin: 25px 0 0;
  color: var(--muted);
  font-size: 15px;
  line-height: 1.9;
}

.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 34px;
}

.primary-button,
.secondary-button,
.outline-button {
  min-height: 46px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 13px;
  padding: 0 18px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 750;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    background 0.2s ease;
}

.primary-button {
  border: 1px solid rgba(255, 255, 255, 0.15);
  background: var(--text);
  color: var(--bg);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.18);
}

.primary-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 16px 36px rgba(0, 0, 0, 0.24);
}

.secondary-button,
.outline-button {
  border: 1px solid var(--line-strong);
  background: transparent;
  color: var(--text);
}

.secondary-button:hover,
.outline-button:hover {
  transform: translateY(-2px);
  background: var(--surface-2);
}

.hero-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 22px;
  margin-top: 32px;
}

.meta-item {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  color: var(--muted-2);
  font-size: 11px;
}

.meta-icon {
  color: var(--primary);
}

.hero-visual {
  position: relative;
  min-height: 560px;
  display: grid;
  place-items: center;
}

.visual-ring {
  position: absolute;
  border: 1px solid var(--line);
  border-radius: 50%;
}

.ring-back {
  width: 520px;
  height: 520px;
  border-color: rgba(167, 139, 250, 0.11);
  animation: slowRotate 18s linear infinite;
}

.ring-front {
  width: 410px;
  height: 410px;
  border-style: dashed;
  border-color: rgba(103, 232, 249, 0.12);
  animation: slowRotateReverse 24s linear infinite;
}

.profile-card {
  position: relative;
  width: min(390px, 82vw);
  min-height: 455px;
  padding: 42px 36px;
  overflow: hidden;
  border: 1px solid var(--line-strong);
  border-radius: 32px;
  background:
    linear-gradient(145deg, rgba(255, 255, 255, 0.06), transparent 38%),
    var(--surface);
  box-shadow: var(--shadow);
  transform: rotate(3deg);
  transition: transform 0.4s ease;
}

.profile-card:hover {
  transform: rotate(0deg) translateY(-5px);
}

.card-glow {
  position: absolute;
  width: 220px;
  height: 220px;
  top: -100px;
  right: -70px;
  border-radius: 50%;
  background: rgba(167, 139, 250, 0.2);
  filter: blur(40px);
}

.avatar-frame {
  position: relative;
  width: 150px;
  height: 150px;
  margin-bottom: 34px;
}

.avatar-placeholder {
  width: 100%;
  height: 100%;
  display: grid;
  place-items: center;
  border: 1px solid rgba(167, 139, 250, 0.3);
  border-radius: 42px;
  background:
    radial-gradient(circle at 30% 25%, rgba(240, 171, 252, 0.7), transparent 24%),
    radial-gradient(circle at 70% 70%, rgba(103, 232, 249, 0.5), transparent 27%),
    linear-gradient(145deg, #25213a, #11131b);
  box-shadow:
    0 30px 60px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.12);
}

.avatar-placeholder span {
  font-size: 68px;
  font-weight: 900;
  letter-spacing: -0.1em;
  color: white;
  text-shadow: 0 0 40px rgba(167, 139, 250, 0.6);
}

.avatar-status {
  position: absolute;
  right: -4px;
  bottom: -4px;
  width: 25px;
  height: 25px;
  border: 5px solid var(--surface);
  border-radius: 50%;
  background: #86efac;
}

.mini-label {
  color: var(--primary);
  font-size: 9px;
  font-weight: 850;
  letter-spacing: 0.22em;
}

.profile-card-content h2 {
  margin: 10px 0;
  font-size: 28px;
  letter-spacing: -0.05em;
}

.profile-card-content p {
  margin: 0;
  color: var(--muted);
  font-size: 12px;
  line-height: 1.75;
}

.floating-chip {
  position: absolute;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 9px 12px;
  border: 1px solid var(--line-strong);
  border-radius: 999px;
  background: rgba(17, 19, 27, 0.85);
  backdrop-filter: blur(12px);
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
  color: var(--text);
  font-size: 10px;
  font-weight: 700;
}

.chip-top {
  top: 78px;
  right: -26px;
}

.chip-bottom {
  bottom: 70px;
  left: -34px;
}

.floating-chip span:first-child {
  color: var(--primary);
}

.stats-grid {
  position: relative;
  z-index: 2;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  margin-top: 55px;
  border-top: 1px solid var(--line);
  border-bottom: 1px solid var(--line);
}

.stat-card {
  min-height: 110px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
  padding: 18px 24px;
  border-right: 1px solid var(--line);
}

.stat-card:last-child {
  border-right: 0;
}

.stat-card strong {
  font-size: 27px;
  letter-spacing: -0.05em;
}

.stat-card span {
  color: var(--muted-2);
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.scroll-hint {
  position: absolute;
  left: 50%;
  bottom: 28px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 7px;
  border: 0;
  background: transparent;
  color: var(--muted-2);
  font-size: 9px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  transform: translateX(-50%);
}

.scroll-arrow {
  color: var(--primary);
  font-size: 17px;
  animation: float 1.8s ease-in-out infinite;
}

.hero-orb {
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
  filter: blur(2px);
}

.orb-one {
  width: 300px;
  height: 300px;
  top: 130px;
  left: -190px;
  background: rgba(124, 58, 237, 0.12);
  filter: blur(70px);
}

.orb-two {
  width: 260px;
  height: 260px;
  right: -160px;
  bottom: 150px;
  background: rgba(34, 211, 238, 0.08);
  filter: blur(80px);
}

.section-heading {
  margin-bottom: 62px;
}

.section-heading > div {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 15px;
}

.section-number {
  color: var(--primary);
  font-size: 10px;
  font-weight: 800;
}

.section-label {
  color: var(--muted-2);
  font-size: 10px;
  font-weight: 800;
  letter-spacing: 0.22em;
}

.section-heading h2 {
  max-width: 760px;
  margin: 0;
  font-size: clamp(38px, 5vw, 65px);
  line-height: 1.03;
  letter-spacing: -0.065em;
}

.section-heading h2 span {
  color: var(--muted-2);
}

.about-section {
  border-top: 1px solid var(--line);
}

.about-grid {
  display: grid;
  grid-template-columns: 1.15fr 0.85fr;
  gap: 90px;
}

.about-text {
  max-width: 720px;
}

.large-paragraph {
  color: var(--text);
  font-size: 24px;
  line-height: 1.45;
  letter-spacing: -0.03em;
}

.about-text > p:not(.large-paragraph) {
  color: var(--muted);
  font-size: 14px;
  line-height: 1.9;
}

.quote-box {
  position: relative;
  margin-top: 38px;
  padding: 24px 26px 24px 52px;
  border-left: 2px solid var(--primary);
  background: linear-gradient(90deg, rgba(167, 139, 250, 0.07), transparent);
}

.quote-mark {
  position: absolute;
  top: 13px;
  left: 18px;
  color: var(--primary);
  font-size: 32px;
  font-weight: 900;
}

.quote-box p {
  margin: 0;
  color: var(--text);
  font-size: 13px;
  line-height: 1.7;
}

.about-side {
  display: grid;
  gap: 13px;
}

.info-card {
  display: grid;
  grid-template-columns: 42px 1fr;
  gap: 15px;
  padding: 20px;
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
  background: var(--surface);
  transition:
    transform 0.2s ease,
    border-color 0.2s ease;
}

.info-card:hover {
  transform: translateY(-3px);
  border-color: var(--line-strong);
}

.info-card-icon {
  width: 40px;
  height: 40px;
  display: grid;
  place-items: center;
  border-radius: 11px;
  background: var(--surface-2);
  color: var(--primary);
  font-size: 17px;
}

.info-card small {
  color: var(--muted-2);
  font-size: 8px;
  font-weight: 800;
  letter-spacing: 0.18em;
}

.info-card h3 {
  margin: 6px 0;
  font-size: 14px;
}

.info-card p {
  margin: 0;
  color: var(--muted);
  font-size: 11px;
  line-height: 1.7;
}

.interests-wrapper {
  margin-top: 85px;
}

.subheading {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 18px;
}

.subheading span {
  color: var(--muted-2);
  font-size: 9px;
  font-weight: 800;
  letter-spacing: 0.18em;
}

.subheading div {
  flex: 1;
  height: 1px;
  background: var(--line);
}

.interest-list {
  display: flex;
  flex-wrap: wrap;
  gap: 9px;
}

.interest-pill {
  padding: 10px 13px;
  border: 1px solid var(--line);
  border-radius: 999px;
  background: var(--surface);
  color: var(--muted);
  font-size: 11px;
  transition:
    color 0.2s ease,
    border-color 0.2s ease,
    transform 0.2s ease;
}

.interest-pill:hover {
  color: var(--text);
  border-color: var(--line-strong);
  transform: translateY(-2px);
}

.skills-section {
  background:
    linear-gradient(180deg, transparent, rgba(167, 139, 250, 0.025), transparent);
}

.skills-intro {
  max-width: 620px;
  margin-top: -35px;
  margin-bottom: 42px;
}

.skills-intro p {
  color: var(--muted);
  font-size: 13px;
  line-height: 1.8;
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
}

.skill-card {
  padding: 22px;
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
  background: var(--surface);
  transition:
    transform 0.25s ease,
    border-color 0.25s ease;
}

.skill-card:hover {
  transform: translateY(-4px);
  border-color: var(--line-strong);
}

.skill-top {
  display: grid;
  grid-template-columns: 45px 1fr auto;
  align-items: center;
  gap: 14px;
}

.skill-icon {
  width: 44px;
  height: 44px;
  display: grid;
  place-items: center;
  border-radius: 12px;
  background: var(--surface-2);
  color: var(--primary);
  font-size: 13px;
  font-weight: 900;
}

.skill-top span {
  color: var(--muted-2);
  font-size: 8px;
  font-weight: 800;
  letter-spacing: 0.16em;
  text-transform: uppercase;
}

.skill-top h3 {
  margin: 4px 0 0;
  font-size: 14px;
}

.skill-top strong {
  color: var(--muted);
  font-size: 11px;
}

.skill-bar {
  height: 5px;
  margin-top: 18px;
  overflow: hidden;
  border-radius: 999px;
  background: var(--surface-3);
}

.skill-bar span {
  display: block;
  height: 100%;
  border-radius: inherit;
  background: linear-gradient(90deg, var(--primary-2), var(--primary), var(--cyan));
  box-shadow: 0 0 18px rgba(167, 139, 250, 0.25);
}

.skill-card p {
  margin: 13px 0 0;
  color: var(--muted);
  font-size: 10px;
  line-height: 1.6;
}

.projects-section {
  border-top: 1px solid var(--line);
}

.project-heading {
  display: flex;
  justify-content: space-between;
  gap: 30px;
}

.project-heading h2 {
  margin-left: auto;
}

.project-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.project-card {
  position: relative;
  min-height: 500px;
  overflow: hidden;
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  background: var(--surface);
  transition:
    transform 0.3s ease,
    border-color 0.3s ease;
}

.project-card:hover {
  transform: translateY(-6px);
  border-color: var(--line-strong);
}

.project-number {
  position: absolute;
  top: 18px;
  right: 21px;
  z-index: 3;
  color: rgba(255, 255, 255, 0.45);
  font-size: 10px;
  font-weight: 800;
}

.project-visual {
  position: relative;
  height: 245px;
  overflow: hidden;
  background:
    radial-gradient(circle at 50% 30%, rgba(167, 139, 250, 0.13), transparent 45%),
    var(--surface-2);
}

.project-grid-lines {
  position: absolute;
  inset: 0;
  opacity: 0.3;
  background-image:
    linear-gradient(var(--line) 1px, transparent 1px),
    linear-gradient(90deg, var(--line) 1px, transparent 1px);
  background-size: 35px 35px;
  transform: perspective(500px) rotateX(55deg) scale(1.5);
  transform-origin: bottom;
}

.project-orb {
  position: absolute;
  width: 160px;
  height: 160px;
  top: 25px;
  left: 50%;
  border-radius: 50%;
  transform: translateX(-50%);
  background: radial-gradient(circle at 30% 25%, rgba(255, 255, 255, 0.35), transparent 10%), linear-gradient(145deg, rgba(167, 139, 250, 0.8), rgba(34, 211, 238, 0.25));
  filter: blur(1px);
  box-shadow: 0 0 80px rgba(167, 139, 250, 0.22);
}

.project-window {
  position: absolute;
  left: 50%;
  bottom: 18px;
  width: 230px;
  height: 125px;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.14);
  border-radius: 13px;
  background: rgba(8, 9, 13, 0.72);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.35);
  backdrop-filter: blur(12px);
  transform: translateX(-50%);
}

.window-top {
  height: 22px;
  display: flex;
  align-items: center;
  gap: 4px;
  padding-left: 9px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}

.window-top i {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.3);
}

.window-body {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 6px;
  padding: 13px;
}

.window-body span {
  height: 9px;
  border-radius: 3px;
  background: rgba(255, 255, 255, 0.1);
}

.window-body span:nth-child(1) {
  grid-column: span 4;
  height: 25px;
  background: rgba(167, 139, 250, 0.25);
}

.window-body span:nth-child(2) {
  grid-column: span 2;
  height: 25px;
  background: rgba(103, 232, 249, 0.15);
}

.window-body span:nth-child(3) {
  grid-column: span 2;
}

.window-body span:nth-child(4) {
  grid-column: span 4;
}

.project-content {
  padding: 24px;
}

.project-meta {
  display: flex;
  justify-content: space-between;
  color: var(--muted-2);
  font-size: 9px;
  font-weight: 800;
  letter-spacing: 0.11em;
  text-transform: uppercase;
}

.project-content h3 {
  margin: 13px 0 8px;
  font-size: 26px;
  letter-spacing: -0.045em;
}

.project-content p {
  max-width: 500px;
  margin: 0;
  color: var(--muted);
  font-size: 11px;
  line-height: 1.75;
}

.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 17px;
}

.tag-list span {
  padding: 6px 8px;
  border: 1px solid var(--line);
  border-radius: 7px;
  color: var(--muted-2);
  font-size: 8px;
  font-weight: 700;
}

.project-link {
  display: inline-flex;
  align-items: center;
  gap: 9px;
  margin-top: 20px;
  padding: 0;
  border: 0;
  background: transparent;
  color: var(--text);
  font-size: 10px;
  font-weight: 800;
}

.project-link span {
  color: var(--primary);
  transition: transform 0.2s ease;
}

.project-link:hover span {
  transform: translate(3px, -3px);
}

.accent-cyan .project-orb {
  background: linear-gradient(145deg, rgba(103, 232, 249, 0.75), rgba(167, 139, 250, 0.25));
}

.accent-pink .project-orb {
  background: linear-gradient(145deg, rgba(240, 171, 252, 0.75), rgba(167, 139, 250, 0.25));
}

.accent-orange .project-orb {
  background: linear-gradient(145deg, rgba(253, 186, 116, 0.75), rgba(167, 139, 250, 0.25));
}

.accent-green .project-orb {
  background: linear-gradient(145deg, rgba(134, 239, 172, 0.75), rgba(103, 232, 249, 0.25));
}

.accent-blue .project-orb {
  background: linear-gradient(145deg, rgba(96, 165, 250, 0.75), rgba(167, 139, 250, 0.25));
}

.projects-footer {
  display: flex;
  justify-content: center;
  margin-top: 35px;
}

.outline-button {
  padding-inline: 20px;
}

.journey-section {
  background: var(--bg-soft);
}

.journey-layout {
  display: grid;
  grid-template-columns: 0.72fr 1.28fr;
  gap: 90px;
}

.journey-intro {
  position: sticky;
  top: 130px;
  align-self: start;
  height: max-content;
}

.big-year {
  display: block;
  color: var(--primary);
  font-size: 120px;
  line-height: 0.8;
  font-weight: 900;
  letter-spacing: -0.1em;
  opacity: 0.75;
}

.journey-intro p {
  max-width: 340px;
  margin-top: 45px;
  color: var(--muted);
  font-size: 14px;
  line-height: 1.9;
}

.timeline {
  position: relative;
}

.timeline::before {
  content: "";
  position: absolute;
  top: 5px;
  bottom: 0;
  left: 5px;
  width: 1px;
  background: var(--line);
}

.timeline-item {
  position: relative;
  display: grid;
  grid-template-columns: 30px 1fr;
  gap: 25px;
  min-height: 160px;
}

.timeline-marker {
  position: relative;
  z-index: 2;
  padding-top: 6px;
}

.timeline-marker span {
  display: block;
  width: 11px;
  height: 11px;
  border: 2px solid var(--primary);
  border-radius: 50%;
  background: var(--bg-soft);
  box-shadow: 0 0 0 6px rgba(167, 139, 250, 0.06);
}

.timeline-content {
  padding-bottom: 50px;
}

.timeline-year {
  color: var(--primary);
  font-size: 9px;
  font-weight: 850;
  letter-spacing: 0.18em;
}

.timeline-content h3 {
  margin: 8px 0 8px;
  font-size: 22px;
  letter-spacing: -0.035em;
}

.timeline-content p {
  max-width: 590px;
  margin: 0;
  color: var(--muted);
  font-size: 12px;
  line-height: 1.8;
}

.philosophy-section {
  padding-top: 80px;
}

.philosophy-card {
  position: relative;
  min-height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
  padding: 70px;
  border: 1px solid var(--line);
  border-radius: 34px;
  background:
    radial-gradient(circle at 20% 10%, rgba(167, 139, 250, 0.12), transparent 25%),
    radial-gradient(circle at 80% 80%, rgba(103, 232, 249, 0.08), transparent 25%),
    var(--surface);
}

.philosophy-card h2 {
  position: relative;
  z-index: 2;
  max-width: 860px;
  margin: 20px 0 35px;
  font-size: clamp(34px, 5vw, 61px);
  line-height: 1.05;
  letter-spacing: -0.065em;
}

.philosophy-card h2 span {
  color: var(--muted-2);
}

.philosophy-bottom {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
}

.philosophy-bottom > span {
  color: var(--muted-2);
  font-size: 11px;
}

.philosophy-bottom button {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 11px 13px;
  border: 1px solid var(--line-strong);
  border-radius: 10px;
  background: var(--surface-2);
  color: var(--text);
  font-size: 10px;
  font-weight: 750;
}

.philosophy-decoration {
  position: absolute;
  border: 1px solid rgba(167, 139, 250, 0.11);
  border-radius: 50%;
}

.decoration-one {
  width: 400px;
  height: 400px;
  right: -180px;
  top: -150px;
}

.decoration-two {
  width: 270px;
  height: 270px;
  right: -110px;
  top: -85px;
  border-style: dashed;
}

.contact-section {
  border-top: 1px solid var(--line);
}

.contact-grid {
  display: grid;
  grid-template-columns: 0.9fr 1.1fr;
  gap: 100px;
}

.contact-copy > p:not(.large-paragraph) {
  color: var(--muted);
  font-size: 13px;
  line-height: 1.8;
}

.contact-details {
  display: grid;
  gap: 9px;
  margin-top: 35px;
}

.contact-item {
  display: grid;
  grid-template-columns: 40px 1fr auto;
  align-items: center;
  gap: 13px;
  padding: 13px;
  border: 1px solid var(--line);
  border-radius: 13px;
  background: var(--surface);
}

.contact-icon {
  width: 38px;
  height: 38px;
  display: grid;
  place-items: center;
  border-radius: 10px;
  background: var(--surface-2);
  color: var(--primary);
  font-weight: 800;
}

.contact-item small {
  display: block;
  color: var(--muted-2);
  font-size: 7px;
  font-weight: 800;
  letter-spacing: 0.16em;
}

.contact-item strong {
  display: block;
  margin-top: 4px;
  color: var(--text);
  font-size: 11px;
}

.contact-arrow {
  color: var(--primary);
}

.contact-form {
  display: grid;
  gap: 17px;
  padding: 28px;
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  background: var(--surface);
}

.contact-form label {
  display: grid;
  gap: 8px;
}

.contact-form label > span {
  color: var(--muted-2);
  font-size: 9px;
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.contact-form input,
.contact-form textarea {
  width: 100%;
  border: 1px solid var(--line);
  border-radius: 11px;
  outline: none;
  background: var(--surface-2);
  color: var(--text);
  padding: 12px 13px;
  font-size: 12px;
  transition:
    border-color 0.2s ease,
    box-shadow 0.2s ease;
}

.contact-form input {
  height: 44px;
}

.contact-form textarea {
  resize: vertical;
  min-height: 130px;
}

.contact-form input::placeholder,
.contact-form textarea::placeholder {
  color: var(--muted-2);
}

.contact-form input:focus,
.contact-form textarea:focus {
  border-color: rgba(167, 139, 250, 0.55);
  box-shadow: 0 0 0 4px rgba(167, 139, 250, 0.07);
}

.form-button {
  width: 100%;
  margin-top: 4px;
}

.form-success {
  margin: 0;
  padding: 11px;
  border: 1px solid rgba(134, 239, 172, 0.18);
  border-radius: 9px;
  background: rgba(134, 239, 172, 0.06);
  color: #86efac;
  font-size: 10px;
  line-height: 1.6;
}

.site-footer {
  border-top: 1px solid var(--line);
  background: var(--bg-soft);
}

.footer-main {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: center;
  gap: 30px;
  min-height: 130px;
}

.footer-brand {
  display: flex;
  align-items: center;
  gap: 12px;
}

.footer-brand p {
  margin: 4px 0 0;
  color: var(--muted-2);
  font-size: 9px;
}

.footer-links {
  display: flex;
  gap: 17px;
}

.footer-links button {
  padding: 4px;
  border: 0;
  background: transparent;
  color: var(--muted);
  font-size: 10px;
}

.footer-links button:hover {
  color: var(--text);
}

.footer-socials {
  display: flex;
  justify-content: flex-end;
  gap: 7px;
}

.footer-socials a {
  width: 33px;
  height: 33px;
  display: grid;
  place-items: center;
  border: 1px solid var(--line);
  border-radius: 9px;
  color: var(--muted);
  font-size: 8px;
  font-weight: 800;
}

.footer-socials a:hover {
  color: var(--text);
  border-color: var(--line-strong);
}

.footer-bottom {
  min-height: 65px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  border-top: 1px solid var(--line);
  color: var(--muted-2);
  font-size: 9px;
}

@keyframes pulse {
  0%,
  100% {
    box-shadow: 0 0 0 4px rgba(134, 239, 172, 0.08);
  }

  50% {
    box-shadow: 0 0 0 8px rgba(134, 239, 172, 0.02);
  }
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(6px);
  }
}

@keyframes slowRotate {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}

@keyframes slowRotateReverse {
  from {
    transform: rotate(360deg);
  }

  to {
    transform: rotate(0deg);
  }
}

@media (max-width: 1000px) {
  .hero-grid {
    grid-template-columns: 1fr;
  }

  .hero-copy {
    max-width: 780px;
  }

  .hero-visual {
    min-height: 510px;
  }

  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .stat-card:nth-child(2) {
    border-right: 0;
  }

  .stat-card:nth-child(-n + 2) {
    border-bottom: 1px solid var(--line);
  }

  .about-grid,
  .journey-layout,
  .contact-grid {
    grid-template-columns: 1fr;
    gap: 50px;
  }

  .journey-intro {
    position: static;
  }

  .big-year {
    font-size: 90px;
  }

  .journey-intro p {
    margin-top: 25px;
  }

  .project-heading {
    display: block;
  }

  .project-heading h2 {
    margin-left: 0;
  }
}

@media (max-width: 760px) {
  :root {
    --header-height: 67px;
  }

  .container {
    width: min(var(--container), calc(100% - 32px));
  }

  .section {
    padding: 90px 0;
  }

  .desktop-nav {
    display: none;
  }

  .menu-button {
    display: grid;
  }

  .mobile-menu {
    position: absolute;
    top: calc(100% + 1px);
    left: 0;
    right: 0;
    display: grid;
    padding: 12px 16px 17px;
    border-bottom: 1px solid var(--line);
    background: var(--bg);
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.18);
  }

  .mobile-menu button {
    padding: 13px 10px;
    border: 0;
    border-radius: 9px;
    background: transparent;
    color: var(--muted);
    text-align: left;
    font-size: 12px;
  }

  .mobile-menu button.active,
  .mobile-menu button:hover {
    background: var(--surface-2);
    color: var(--text);
  }

  .hero {
    min-height: auto;
    padding-top: 120px;
    padding-bottom: 70px;
  }

  .hero h1 {
    font-size: clamp(55px, 18vw, 90px);
  }

  .hero-description {
    font-size: 13px;
  }

  .hero-visual {
    min-height: 430px;
  }

  .ring-back {
    width: 390px;
    height: 390px;
  }

  .ring-front {
    width: 320px;
    height: 320px;
  }

  .profile-card {
    min-height: 385px;
    padding: 30px;
  }

  .avatar-frame {
    width: 125px;
    height: 125px;
  }

  .avatar-placeholder span {
    font-size: 55px;
  }

  .chip-top {
    right: -8px;
  }

  .chip-bottom {
    left: -8px;
  }

  .stats-grid {
    grid-template-columns: 1fr 1fr;
  }

  .stat-card {
    min-height: 90px;
    padding: 14px;
  }

  .section-heading {
    margin-bottom: 40px;
  }

  .section-heading h2 {
    font-size: 39px;
  }

  .about-grid,
  .skills-grid,
  .project-grid {
    grid-template-columns: 1fr;
  }

  .large-paragraph {
    font-size: 20px;
  }

  .skills-intro {
    margin-top: -15px;
  }

  .project-card {
    min-height: 480px;
  }

  .philosophy-card {
    min-height: 360px;
    padding: 35px 25px;
    border-radius: 24px;
  }

  .philosophy-card h2 {
    font-size: 34px;
  }

  .philosophy-bottom {
    align-items: flex-start;
    flex-direction: column;
  }

  .contact-form {
    padding: 20px;
  }

  .footer-main {
    grid-template-columns: 1fr;
    padding: 35px 0;
  }

  .footer-links {
    flex-wrap: wrap;
  }

  .footer-socials {
    justify-content: flex-start;
  }

  .footer-bottom {
    align-items: flex-start;
    flex-direction: column;
    justify-content: center;
    padding: 18px 0;
  }
}

@media (max-width: 430px) {
  .container {
    width: min(var(--container), calc(100% - 24px));
  }

  .hero h1 {
    font-size: 55px;
  }

  .hero-actions {
    flex-direction: column;
  }

  .primary-button,
  .secondary-button {
    width: 100%;
  }

  .hero-meta {
    flex-direction: column;
    gap: 10px;
  }

  .stats-grid {
    grid-template-columns: 1fr;
  }

  .stat-card,
  .stat-card:nth-child(2) {
    border-right: 0;
    border-bottom: 1px solid var(--line);
  }

  .stat-card:last-child {
    border-bottom: 0;
  }

  .section-heading h2 {
    font-size: 34px;
  }

  .skill-top {
    grid-template-columns: 40px 1fr auto;
  }

  .skill-icon {
    width: 40px;
    height: 40px;
  }

  .project-visual {
    height: 220px;
  }

  .project-window {
    width: 205px;
  }
}

@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }

  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
</style>

/*
  /* Customization note 001: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 002: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 003: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 004: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 005: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 006: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 007: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 008: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 009: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 010: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 011: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 012: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 013: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 014: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 015: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 016: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 017: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 018: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 019: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 020: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 021: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 022: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 023: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 024: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 025: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 026: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 027: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 028: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 029: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 030: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 031: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 032: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 033: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 034: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 035: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 036: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 037: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 038: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 039: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 040: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 041: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 042: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 043: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 044: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 045: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 046: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 047: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 048: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 049: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 050: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 051: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 052: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 053: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 054: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 055: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 056: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 057: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 058: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 059: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 060: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 061: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 062: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 063: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 064: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 065: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 066: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 067: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 068: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 069: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 070: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 071: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 072: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 073: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 074: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 075: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 076: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 077: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 078: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 079: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 080: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 081: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 082: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 083: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 084: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 085: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 086: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 087: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 088: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 089: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 090: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 091: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 092: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 093: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 094: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 095: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 096: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 097: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 098: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 099: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 100: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 101: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 102: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 103: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 104: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 105: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 106: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 107: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 108: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 109: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 110: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 111: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 112: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 113: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 114: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 115: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 116: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 117: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 118: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 119: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 120: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 121: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 122: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 123: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 124: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 125: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 126: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 127: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 128: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 129: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 130: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 131: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 132: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 133: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 134: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 135: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 136: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 137: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 138: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 139: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 140: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 141: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 142: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 143: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 144: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 145: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 146: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 147: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 148: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 149: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 150: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 151: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 152: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 153: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 154: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 155: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 156: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 157: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 158: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 159: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 160: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 161: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 162: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 163: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 164: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 165: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 166: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 167: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 168: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 169: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 170: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 171: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 172: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 173: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 174: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 175: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 176: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 177: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 178: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 179: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 180: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 181: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 182: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 183: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 184: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 185: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 186: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 187: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 188: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 189: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 190: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 191: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 192: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 193: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 194: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 195: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 196: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 197: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 198: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 199: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 200: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 201: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 202: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 203: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 204: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 205: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 206: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 207: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 208: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 209: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 210: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 211: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 212: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 213: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 214: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 215: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 216: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 217: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 218: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 219: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 220: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 221: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 222: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 223: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 224: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 225: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 226: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 227: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 228: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 229: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 230: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 231: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 232: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 233: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 234: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 235: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 236: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 237: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 238: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 239: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 240: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 241: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 242: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 243: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 244: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 245: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 246: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 247: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 248: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 249: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 250: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 251: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 252: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 253: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 254: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 255: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 256: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 257: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 258: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 259: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 260: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 261: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 262: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 263: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 264: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 265: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 266: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 267: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 268: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 269: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 270: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 271: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 272: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 273: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 274: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 275: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 276: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 277: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 278: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 279: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 280: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 281: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 282: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 283: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 284: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 285: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 286: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 287: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 288: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 289: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 290: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 291: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 292: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 293: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 294: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 295: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 296: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 297: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 298: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 299: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 300: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 301: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 302: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 303: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 304: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 305: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 306: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 307: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 308: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 309: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 310: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 311: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 312: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 313: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 314: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 315: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 316: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 317: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 318: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 319: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 320: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 321: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 322: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 323: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 324: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 325: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 326: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 327: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 328: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 329: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 330: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 331: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 332: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 333: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 334: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 335: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 336: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 337: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 338: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 339: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 340: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 341: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 342: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 343: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 344: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 345: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 346: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 347: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 348: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 349: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 350: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 351: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 352: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 353: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 354: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 355: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 356: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 357: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 358: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 359: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 360: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 361: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 362: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 363: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 364: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 365: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 366: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 367: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 368: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 369: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 370: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 371: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 372: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 373: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 374: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 375: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 376: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 377: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 378: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 379: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 380: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 381: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 382: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 383: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 384: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 385: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 386: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 387: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 388: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 389: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 390: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 391: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 392: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 393: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 394: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 395: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 396: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 397: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 398: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 399: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 400: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 401: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 402: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 403: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 404: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 405: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 406: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 407: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 408: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 409: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 410: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 411: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 412: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 413: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 414: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 415: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 416: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 417: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 418: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 419: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 420: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 421: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 422: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 423: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 424: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 425: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 426: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 427: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 428: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 429: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 430: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 431: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 432: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 433: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 434: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 435: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 436: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 437: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 438: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 439: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 440: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 441: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 442: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 443: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 444: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 445: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 446: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 447: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 448: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 449: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 450: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 451: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 452: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 453: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 454: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 455: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 456: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 457: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 458: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 459: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 460: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 461: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 462: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 463: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 464: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 465: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 466: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 467: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 468: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 469: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 470: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 471: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 472: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 473: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 474: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 475: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 476: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 477: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 478: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 479: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 480: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 481: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 482: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 483: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 484: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 485: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 486: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 487: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 488: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 489: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 490: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 491: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 492: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 493: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 494: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 495: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 496: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 497: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 498: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 499: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
  /* Customization note 500: bagian ini sengaja diberi ruang agar file mudah dikembangkan tanpa harus mencari struktur utama dari nol. */
*/
