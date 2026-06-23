<script setup>
import { Button } from '@/Components/ui/button'
import { Head, usePage } from '@inertiajs/vue3'
import LoadingOverlay from '@/Components/LoadingOverlay.vue'
import { GraduationCap, CalendarCheck2, Users, QrCode, ShieldCheck, ArrowRight, Sparkles } from 'lucide-vue-next'

const page = usePage()

const redirectGoogle = () => {
  document.dispatchEvent(new Event('loading:start'))
  setTimeout(() => {
    window.location.href = route('auth.google')
  }, 100)
}
</script>

<template>
  <LoadingOverlay />
  <Head title="Masuk Akun - SIMAKU" />

  <div class="min-h-screen grid md:grid-cols-2 font-sans selection:bg-[#006633]/10 selection:text-[#006633]">

    <div class="relative hidden md:flex flex-col justify-between overflow-hidden p-12 bg-[#001f0f]">

      <div class="absolute inset-0 bg-gradient-simaku"></div>
      <div class="absolute inset-0 bg-black/5"></div>

      <div class="absolute inset-0 opacity-[0.04]"
        style="background-image: radial-gradient(circle, #fff 1.5px, transparent 1.5px); background-size: 24px 24px;">
      </div>

      <div class="absolute -top-40 -left-20 w-96 h-96 bg-[#008751]/20 rounded-full blur-3xl pointer-events-none"></div>
      <div class="absolute bottom-0 right-0 w-80 h-80 bg-[#006633]/30 rounded-full blur-3xl pointer-events-none"></div>

      <div class="relative z-10 flex items-center gap-3">
        <div class="w-9 h-9 rounded-xl bg-white/10 backdrop-blur-md border border-white/20 flex items-center justify-center">
          <span class="text-white font-bold text-base tracking-tighter">S</span>
        </div>
        <div>
          <p class="text-white font-bold text-sm tracking-tight leading-none">SIMAKU PLATFORM</p>
          <p class="text-white/50 text-[10px] tracking-wider font-medium mt-0.5 uppercase">UIN Academic Operations</p>
        </div>
      </div>

      <div class="relative z-10">
        <div class="inline-flex items-center gap-2 bg-white/5 border border-white/10 rounded-xl px-3 py-1.5 mb-6 backdrop-blur-xs">
          <Sparkles class="w-3.5 h-3.5 text-[#D4AF37] fill-[#D4AF37]" />
          <span class="text-white/90 text-xs font-semibold tracking-wide">Ekosistem Kegiatan Digital Terintegrasi</span>
        </div>

        <h1 class="text-4xl font-extrabold text-white tracking-tight leading-tight">
          Akselerasi Potensi<br />
          Akademik & Bakat<br />
          <span class="bg-gradient-to-r from-emerald-400 to-teal-300 bg-clip-text text-transparent">Civitas UIN Resmi</span>
        </h1>

        <p class="mt-4 text-white/70 text-sm leading-relaxed max-w-sm font-normal">
          Satu gerbang operasi premium untuk memvalidasi portofolio ilmiah, perlombaan, sosial, dan keagamaan kampus secara real-time.
        </p>

        <div class="mt-8 space-y-3.5">
          <div class="flex items-center gap-3 text-white/80 hover:text-white transition-colors duration-200">
            <div class="w-8 h-8 rounded-xl bg-white/5 border border-white/10 flex items-center justify-center flex-shrink-0 shadow-xs">
              <CalendarCheck2 class="w-4 h-4 text-emerald-400" />
            </div>
            <span class="text-sm font-medium">E-Katalog Agenda Kegiatan Terstruktur</span>
          </div>
          <div class="flex items-center gap-3 text-white/80 hover:text-white transition-colors duration-200">
            <div class="w-8 h-8 rounded-xl bg-white/5 border border-white/10 flex items-center justify-center flex-shrink-0 shadow-xs">
              <Users class="w-4 h-4 text-emerald-400" />
            </div>
            <span class="text-sm font-medium">Registrasi & Validasi Kuota Peserta</span>
          </div>
          <div class="flex items-center gap-3 text-white/80 hover:text-white transition-colors duration-200">
            <div class="w-8 h-8 rounded-xl bg-white/5 border border-white/10 flex items-center justify-center flex-shrink-0 shadow-xs">
              <QrCode class="w-4 h-4 text-emerald-400" />
            </div>
            <span class="text-sm font-medium">Presensi QR-Code Terenkripsi Ganda</span>
          </div>
        </div>
      </div>

      <div class="relative z-10">
        <div class="h-px bg-white/10 mb-4"></div>
        <p class="text-white/40 text-xs font-medium">
          Premium Academic Operations · Universitas Islam Negeri · © 2026
        </p>
      </div>

    </div>

    <div class="relative flex items-center justify-center bg-[#F8FAFC] px-6 py-12">

      <div class="absolute inset-0 bg-[radial-gradient(ellipse_at_top_right,rgba(0,102,51,0.04),transparent_50%)] pointer-events-none"></div>
      <div class="absolute inset-0 bg-[radial-gradient(ellipse_at_bottom_left,rgba(0,135,81,0.03),transparent_50%)] pointer-events-none"></div>

      <div class="relative w-full max-w-sm">

        <div class="flex flex-col items-center mb-8 text-center">
          <div class="w-14 h-14 rounded-2xl bg-[#006633] flex items-center justify-center shadow-lg shadow-[#006633]/20 mb-4 transition-transform hover:scale-102">
            <span class="text-white font-bold text-2xl tracking-tighter">S</span>
          </div>
          <h2 class="text-2xl font-extrabold tracking-tight text-slate-900">Selamat Datang</h2>
          <p class="text-sm text-slate-500 mt-1.5 leading-relaxed max-w-xs">
            Masuk ke panel pengendali <span class="font-bold text-[#006633]">SIMAKU</span> menggunakan otentikasi Google resmi Anda.
          </p>
        </div>

        <div
          v-if="page.props.errors?.email || page.props.flash?.error"
          class="mb-6 rounded-xl bg-red-50 p-3.5 text-xs text-red-700 border border-red-200/60 flex items-start gap-2.5 shadow-xs font-semibold"
        >
          <span class="text-base leading-none">⚠️</span>
          <span>{{ page.props.errors?.email || page.props.flash?.error }}</span>
        </div>

        <div class="rounded-2xl border border-slate-200/80 bg-white p-5 mb-5 shadow-xs">
          <div class="flex items-center gap-2.5 mb-5 border-b border-slate-100 pb-4">
            <div class="w-4 h-4 rounded-full bg-emerald-500/10 flex items-center justify-center flex-shrink-0 border border-emerald-500/20">
              <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
            </div>
            <p class="text-[11px] text-slate-400 font-bold tracking-wide uppercase">
              Single Sign-On (SSO) Gate
            </p>
          </div>

          <button
            type="button"
            class="w-full flex items-center justify-center gap-3 py-3.5 rounded-xl bg-[#006633] hover:bg-[#008751] active:scale-98 text-white font-bold text-sm transition-all shadow-md shadow-[#006633]/10 border-none cursor-pointer group"
            @click="redirectGoogle"
          >
            <svg class="h-4 w-4 fill-current transition-transform group-hover:rotate-12 duration-300" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/>
              <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/>
              <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.06H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.94l2.85-2.22.81-.63z"/>
              <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.06l3.66 2.84c.87-2.6 3.3-4.52 6.16-4.52z"/>
            </svg>
            <span>Masuk dengan Akun Google</span>
            <ArrowRight class="w-4 h-4 opacity-70 group-hover:translate-x-0.5 transition-transform" />
          </button>
        </div>

        <div class="rounded-xl bg-slate-50 border border-slate-200/60 px-4 py-3 flex items-start gap-2.5">
          <ShieldCheck class="w-4 h-4 text-[#006633] flex-shrink-0 mt-0.5" />
          <p class="text-[11px] text-slate-500 leading-relaxed font-medium">
            Gunakan alamat email Google institusi resmi UIN yang telah disinkronisasikan secara aktif dengan database SIAKAD utama.
          </p>
        </div>

        <p class="text-center text-[10px] font-bold text-slate-400 tracking-wider uppercase mt-12">
          SIMAKU UIN PLATFORM · © 2026
        </p>

      </div>
    </div>

  </div>
</template>

<style scoped>
@keyframes gradientMove {
  0%   { background-position: 0% 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.bg-gradient-simaku {
  background: linear-gradient(
    -45deg,
    #001a0d,
    #00331a,
    #006633,
    #004d26,
    #008751,
    #002613
  );
  background-size: 400% 400%;
  animation: gradientMove 16s ease infinite;
}
</style>