<script setup>
import { Head, Link } from '@inertiajs/vue3'
import { Button } from '@/Components/ui/button'
import { Card, CardContent } from '@/Components/ui/card'
import { ref } from 'vue'
import { 
  Calendar, Users, Award, ShieldCheck, ArrowUpRight, 
  BookOpen, Compass, Trophy, GraduationCap, ChevronDown, 
  ArrowRight, Sparkles, MapPin, Milestone
} from 'lucide-vue-next'

// MEMPERTAHANKAN PROPS BAWAAN LARAVEL (Agar tidak error)
const props = defineProps({
  events: Array
})

// MEMPERTAHANKAN FUNGSI FORMAT BAWAAN
const formatDate = (date) => {
  if (!date) return '-'
  return new Date(date).toLocaleDateString('id-ID', {
    day: 'numeric',
    month: 'long',
    year: 'numeric'
  })
}

const getLocation = (event) => {
  if (event.venue) return event.venue.name
  if (event.custom_location) return event.custom_location
  return 'Lokasi belum ditentukan'
}

const activeFaq = ref(null)

const stats = [
  { label: 'Total Kegiatan Tahunan', value: '340+', icon: Calendar },
  { label: 'Mahasiswa Berpartisipasi', value: '18,500+', icon: Users },
  { label: 'Tingkat Kepuasan Layanan', value: '98.4%', icon: ShieldCheck },
  { label: 'Prestasi Nasional & Internasional', value: '42', icon: Award },
]

const categories = [
  { name: 'Seminar Akademik', desc: 'Diseminasi riset dan kajian keilmuan Islam kontemporer.', icon: BookOpen },
  { name: 'Workshop & Pelatihan', desc: 'Akselerasi hard skills dan sertifikasi kompetensi industri.', icon: Compass },
  { name: 'Lomba & Kompetisi', desc: 'Ajang pembuktian inovasi dan bakat mahasiswa tingkat nasional.', icon: Trophy },
  { name: 'PBAK & Pengenalan', desc: 'Orientasi inklusif pembentukan karakter mahasantri unggul.', icon: GraduationCap },
]

const faqs = [
  { q: 'Bagaimana cara mendaftar kegiatan di SIMAKU?', a: 'Mahasiswa cukup login menggunakan akun SIAKAD terintegrasi, pilih event aktif di e-katalog, dan klik Daftar secara instan.' },
  { q: 'Apakah sistem ini mengeluarkan e-sertifikat otomatis?', a: 'Ya, sistem akan melacak kehadiran Anda via QR-Scan terenkripsi dan langsung mengenerate e-sertifikat resmi di dashboard setelah event selesai.' },
  { q: 'Bagaimana panitia mengelola presensi peserta?', a: 'Panitia dibekali fitur Scanner bawaan di aplikasi mobile untuk validasi presensi secara real-time tanpa alat tambahan.' }
]
</script>

<template>
  <Head title="SIMAKU - Sistem Informasi Manajemen Kegiatan UIN" />
  
  <div class="min-h-screen bg-[#F8FAFC] text-slate-900 font-sans antialiased selection:bg-[#006633]/10 selection:text-[#006633]">
    
    <nav class="sticky top-0 z-50 bg-[#F8FAFC]/80 backdrop-blur-md border-b border-slate-200/60">
      <div class="max-w-6xl mx-auto flex justify-between items-center px-6 h-16">
        <div class="flex items-center gap-3">
          <div class="w-9 h-9 rounded-xl bg-[#006633] flex items-center justify-center shadow-lg shadow-[#006633]/20">
            <span class="text-white font-bold text-lg tracking-tighter">S</span>
          </div>
          <div class="flex flex-col">
            <span class="font-bold text-slate-900 tracking-tight text-base leading-none">SIMAKU</span>
            <span class="text-[10px] text-slate-500 font-medium tracking-wider mt-0.5">UIN PLATFORM</span>
          </div>
        </div>

        <div class="flex items-center gap-4">
          <Link href="/login">
            <Button variant="ghost" class="text-sm font-medium text-slate-600 hover:text-[#006633]">Masuk</Button>
          </Link>
          <Link href="/login">
            <Button class="bg-[#006633] hover:bg-[#008751] text-white rounded-xl px-4 shadow-sm shadow-[#006633]/10">
              Get Started
            </Button>
          </Link>
        </div>
      </div>
    </nav>

    <header class="relative overflow-hidden pt-16 pb-24 lg:pt-24 lg:pb-32">
      <div class="max-w-6xl mx-auto px-6 relative z-10">
        <div class="text-center max-w-3xl mx-auto">
          <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-[#006633]/5 text-[#006633] text-xs font-semibold mb-6 border border-[#006633]/10 tracking-wide uppercase">
            <Sparkles class="w-3.5 h-3.5 text-[#D4AF37] fill-[#D4AF37]" />
            Transformasi Ekosistem Kegiatan Digital UIN
          </div>
          <h1 class="text-4xl sm:text-6xl font-extrabold tracking-tight text-slate-900 leading-tight">
            Akselerasi Potensi <br/>
            <span class="bg-gradient-to-r from-[#006633] to-[#008751] bg-clip-text text-transparent">Akademik & Bakat</span> Anda
          </h1>
          <p class="mt-6 text-lg text-slate-600 font-normal max-w-2xl mx-auto leading-relaxed">
            Satu platform premium terintegrasi untuk mengelola, mengikuti, dan memvalidasi seluruh portofolio kegiatan ilmiah, sosial, dan keagamaan di UIN.
          </p>
          <div class="mt-10 flex flex-col sm:flex-row items-center justify-center gap-4">
            <a href="/events" class="w-full sm:w-auto">
              <Button class="w-full sm:w-auto px-6 py-6 text-base font-medium text-white bg-[#006633] hover:bg-[#008751] rounded-xl shadow-lg shadow-[#006633]/10 transition-all group">
                Jelajahi Event 
                <ArrowRight class="w-4 h-4 ml-2 group-hover:translate-x-1 transition-transform" />
              </Button>
            </a>
            <a href="/events/create" class="w-full sm:w-auto">
              <Button variant="outline" class="w-full sm:w-auto px-6 py-6 text-base font-medium text-slate-600 bg-white border border-slate-200 rounded-xl hover:bg-slate-50 transition-colors">
                Buat Event
              </Button>
            </a>
          </div>
        </div>
      </div>
      <div class="absolute inset-0 z-0 bg-[radial-gradient(ellipse_at_top_right,_var(--tw-gradient-stops))] from-[#008751]/5 via-transparent to-transparent"></div>
    </header>

    <section class="border-y border-slate-200/60 bg-white py-12">
      <div class="max-w-6xl mx-auto px-6">
        <div class="grid grid-cols-2 lg:grid-cols-4 gap-8">
          <div v-for="(stat, i) in stats" :key="i" class="flex flex-col items-center text-center lg:text-left lg:items-start">
            <span class="text-3xl sm:text-4xl font-bold tracking-tight text-slate-900">{{ stat.value }}</span>
            <span class="text-xs font-medium text-slate-500 mt-2 tracking-wide">{{ stat.label }}</span>
          </div>
        </div>
      </div>
    </section>

    <section class="py-24 max-w-6xl mx-auto px-6">
      <div class="text-center max-w-2xl mx-auto mb-16">
        <h2 class="text-3xl font-bold tracking-tight text-slate-900 sm:text-4xl">Kategori Kegiatan Terstruktur</h2>
        <p class="mt-4 text-slate-600">Ekosistem SIMAKU mencakup segala bentuk akselerasi kapasitas tri dharma perguruan tinggi.</p>
      </div>
      
      <div class="grid md:grid-cols-2 gap-6">
        <div v-for="(cat, i) in categories" :key="i" class="group relative p-8 bg-white border border-slate-200 rounded-2xl transition-all duration-300 hover:shadow-xl hover:shadow-slate-100 hover:border-[#006633]/20 flex gap-5 items-start">
          <div class="p-3 bg-[#006633]/5 text-[#006633] rounded-xl group-hover:bg-[#006633] group-hover:text-white transition-colors duration-300">
            <component :is="cat.icon" class="w-6 h-6" />
          </div>
          <div>
            <h3 class="text-lg font-semibold text-slate-900 tracking-tight flex items-center gap-1.5">
              {{ cat.name }}
              <ArrowUpRight class="w-4 h-4 text-slate-400 opacity-0 group-hover:opacity-100 group-hover:translate-x-0.5 group-hover:-translate-y-0.5 transition-all" />
            </h3>
            <p class="mt-2 text-sm text-slate-500 leading-relaxed">{{ cat.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <section class="max-w-6xl mx-auto px-6 py-16">
      <div class="flex items-end justify-between mb-8">
        <div>
          <h3 class="text-2xl font-bold text-gray-900 tracking-tight">Event Akademis Terbaru</h3>
          <p class="text-xs text-slate-500 mt-1">Agenda terverifikasi yang sedang membuka pendaftaran kuota.</p>
        </div>
        <Link href="/events" class="text-[#006633] text-sm font-semibold hover:underline flex items-center gap-1">
          Lihat semua katalog <ArrowRight class="w-4 h-4" />
        </Link>
      </div>

      <div v-if="!events || events.length === 0" class="text-center text-slate-500 py-16 border border-dashed border-slate-200 rounded-2xl bg-white">
        Belum ada event tersedia saat ini.
      </div>

      <div v-else class="grid md:grid-cols-3 gap-6">
        <Card v-for="event in events" :key="event.id" class="group overflow-hidden rounded-2xl border border-slate-200/80 bg-white hover:shadow-xl hover:border-slate-300 transition-all duration-300 flex flex-col justify-between">
          <div>
            <div class="h-44 relative overflow-hidden bg-gradient-to-br from-[#006633]/10 to-[#008751]/20">
              <img v-if="event.banner" :src="`/storage/${event.banner}`" class="absolute inset-0 w-full h-full object-cover group-hover:scale-105 transition duration-500" />
              <div class="absolute inset-0 bg-gradient-to-t from-black/40 via-transparent to-transparent"></div>
              <span class="absolute top-3 left-4 px-2.5 py-1 rounded-md bg-white/90 text-[10px] font-bold text-[#006633] tracking-wide uppercase shadow-sm">
                UIN EVENT
              </span>
            </div>

            <div class="p-5 space-y-3">
              <h4 class="text-base font-bold text-slate-900 tracking-tight group-hover:text-[#006633] transition-colors line-clamp-2">
                {{ event.title }}
              </h4>
              <div class="space-y-1.5 text-xs text-slate-500 font-medium">
                <div class="flex items-center gap-2">
                  <Calendar class="w-3.5 h-3.5 text-slate-400" /> <span>{{ formatDate(event.start_date) }}</span>
                </div>
                <div class="flex items-center gap-2">
                  <MapPin class="w-3.5 h-3.5 text-slate-400" /> <span class="truncate">{{ getLocation(event) }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="p-5 pt-0">
            <Link :href="`/events/${event.id}`" class="w-full">
              <Button class="w-full rounded-xl bg-slate-50 hover:bg-[#006633] text-slate-700 hover:text-white border border-slate-200/80 hover:border-[#006633] font-semibold text-xs py-2.5 transition-all duration-200">
                Detail Event
              </Button>
            </Link>
          </div>
        </Card>
      </div>
    </section>

    <section class="py-24 max-w-4xl mx-auto px-6">
      <h2 class="text-2xl font-bold text-center tracking-tight text-slate-900 mb-12">Pertanyaan Umum (FAQ)</h2>
      <div class="space-y-4">
        <div v-for="(faq, i) in faqs" :key="i" class="bg-white border border-slate-200/80 rounded-xl overflow-hidden transition-all duration-200">
          <button @click="activeFaq = activeFaq === i ? null : i" class="w-full p-5 text-left flex items-center justify-between font-semibold text-slate-900 hover:bg-slate-50/50">
            <span>{{ faq.q }}</span>
            <ChevronDown class="w-4 h-4 text-slate-500 transition-transform duration-300" :class="{ 'rotate-180': activeFaq === i }" />
          </button>
          <div v-if="activeFaq === i" class="p-5 pt-0 text-sm text-slate-600 leading-relaxed border-t border-slate-100 bg-slate-50/30">
            {{ faq.a }}
          </div>
        </div>
      </div>
    </section>

    <footer class="border-t border-slate-200 bg-white py-8 text-center text-xs text-slate-500">
      <div class="max-w-6xl mx-auto px-6 flex flex-col sm:flex-row items-center justify-between gap-4">
        <div class="flex items-center gap-2">
          <div class="w-6 h-6 rounded bg-[#006633] flex items-center justify-center text-white text-xs font-bold">S</div>
          <span class="font-bold text-slate-800 tracking-tight">SIMAKU UIN</span>
        </div>
        <p>© SIMAKU (Sistem Informasi Manajemen Kegiatan UIN) - 2026. Premium SaaS Standard.</p>
      </div>
    </footer>

  </div>
</template>