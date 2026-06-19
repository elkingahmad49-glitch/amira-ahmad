<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head, usePage, router } from '@inertiajs/vue3'
import { ref } from 'vue'

import SuperAdminSection from '@/Components/Dashboard/SuperAdminSection.vue'
import AdminSection from '@/Components/Dashboard/AdminSection.vue'
import UserSection from '@/Components/Dashboard/UserSection.vue'

const page = usePage()
const user = page.props.auth?.user ?? {}

const props = defineProps({
  role: String,
  events: Array,
  my_events: Array,
  stats: Object,
  greeting: Object,
})

const role = props.role ?? 'user'
const events = props.events ?? []
const myEvents = props.my_events ?? []
const stats = props.stats ?? {}

const goScan = () => {
  router.visit('/scan')
}

import { 
  ScanQrCode, Sparkles, CalendarDays, 
  Layers, ShieldAlert, CheckCircle2, ArrowRight
} from 'lucide-vue-next'
</script>

<template>
  <Head title="Dashboard Utama SIMAKU" />

  <AuthenticatedLayout>
    <template #header>
      <div class="flex items-center justify-between font-sans">
        <div>
          <h2 class="text-xl font-bold tracking-tight text-slate-900 md:text-2xl">
            Pusat Kendali Sistem
          </h2>
          <p class="text-xs text-slate-500 mt-0.5 hidden md:block">Manajemen portofolio kegiatan civitas akademika UIN.</p>
        </div>
      </div>
    </template>

    <div class="relative min-h-screen bg-[#F8FAFC]">
      <div class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-6 font-sans">
        
        <section class="relative overflow-hidden bg-white border border-slate-200/80 rounded-2xl p-6 md:p-8 shadow-xs flex flex-col md:flex-row items-start md:items-center justify-between gap-6">
          <div class="max-w-2xl">
            <div class="inline-flex items-center gap-1.5 text-xs font-semibold text-[#006633] bg-[#006633]/5 px-2.5 py-1 rounded-md mb-3 border border-[#006633]/10">
              <Sparkles class="w-3.5 h-3.5 text-[#D4AF37] fill-[#D4AF37]" /> SIMAKU Academic Hub
            </div>
            <h3 class="text-2xl md:text-3xl font-extrabold tracking-tight text-slate-900 leading-none">
              Assalamualaikum, {{ user.name }} 👋
            </h3>
            <p class="mt-2 text-sm text-slate-500 leading-relaxed">
              {{ greeting?.text ?? 'SIMAKU (Sistem Informasi Manajemen Kegiatan UIN) — Selamat beraktivitas di ekosistem digital kampus premium.' }}
            </p>
            <p v-if="greeting?.author" class="text-xs font-bold text-[#008751] mt-2">
              — {{ greeting.author }}
            </p>
          </div>
          
          <div class="bg-slate-50 border border-slate-100 px-4 py-3 rounded-xl flex items-center gap-3 text-xs font-medium text-slate-600 self-stretch md:self-auto justify-center">
            <span class="w-2 h-2 rounded-full bg-emerald-500 animate-ping"></span>
            SIAKAD Account Active
          </div>
        </section>

        <section v-if="role === 'user'" class="bg-gradient-to-r from-[#006633] to-[#008751] text-white rounded-2xl shadow-lg shadow-[#006633]/10 p-6 flex flex-col sm:flex-row items-center justify-between gap-4">
          <div class="text-center sm:text-left">
            <h3 class="text-lg md:text-xl font-bold tracking-tight">Presensi Kehadiran Terenkripsi</h3>
            <p class="text-xs opacity-80 mt-1">Lakukan check-in presensi seminar/kegiatan secara instan langsung lewat kamera gawai Anda.</p>
          </div>
          <button @click="goScan" class="w-full sm:w-auto bg-white text-[#006633] font-bold px-5 py-3 rounded-xl shadow-xs hover:bg-slate-50 active:scale-98 transition flex items-center justify-center gap-2 text-sm">
            <ScanQrCode class="w-4 h-4" />
            <span>Scan QR Presensi</span>
          </button>
        </section>

        <section class="grid grid-cols-2 md:grid-cols-3 gap-4 sm:gap-6">
          <div class="bg-white border border-slate-200/80 rounded-xl p-5 hover:border-slate-300 transition-colors">
            <div class="flex justify-between items-center text-slate-400">
              <span class="text-xs font-medium tracking-wide text-slate-500 uppercase">Event Diikuti</span>
              <CalendarDays class="w-4 h-4" />
            </div>
            <h2 class="text-2xl sm:text-3xl font-extrabold tracking-tight text-slate-900 mt-3">
              {{ stats.joined_events ?? 0 }}
            </h2>
          </div>

          <div class="bg-white border border-slate-200/80 rounded-xl p-5 hover:border-slate-300 transition-colors">
            <div class="flex justify-between items-center text-slate-400">
              <span class="text-xs font-medium tracking-wide text-slate-500 uppercase">Katalog Tersedia</span>
              <Layers class="w-4 h-4" />
            </div>
            <h2 class="text-2xl sm:text-3xl font-extrabold tracking-tight text-slate-900 mt-3">
              {{ events.length ?? 0 }}
            </h2>
          </div>

          <div class="bg-white border border-slate-200/80 rounded-xl p-5 hover:border-slate-300 transition-colors col-span-2 md:col-span-1">
            <div class="flex justify-between items-center text-slate-400">
              <span class="text-xs font-medium tracking-wide text-slate-500 uppercase">Akses Otentikasi</span>
              <CheckCircle2 class="w-4 h-4 text-emerald-600" />
            </div>
            <div class="flex items-center gap-1.5 mt-3">
              <h2 class="text-xl font-bold text-slate-800 tracking-tight capitalize">
                {{ role }}
              </h2>
              <span class="text-[10px] font-bold bg-emerald-50 text-[#006633] px-1.5 py-0.5 rounded border border-emerald-200/50 uppercase">Verified</span>
            </div>
          </div>
        </section>

        <section class="bg-white border border-slate-200/80 rounded-2xl p-6 shadow-xs">
          <SuperAdminSection v-if="role === 'superadmin'" :events="events" :stats="stats" />
          <AdminSection v-else-if="role === 'admin'" :events="events" :stats="stats" />
          <UserSection v-else :events="events" :my_events="myEvents" :stats="stats" />
        </section>

      </div>
    </div>
  </AuthenticatedLayout>
</template>