<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { Head, router, Link } from '@inertiajs/vue3'
import { ref, computed } from 'vue'
import Swal from 'sweetalert2'
import { toast } from 'vue3-toastify'
import 'vue3-toastify/dist/index.css'
import { 
  Search, SlidersHorizontal, Calendar, MapPin, 
  Layers, Plus, MoreVertical, Eye, FileText, 
  Users2, CheckSquare, Edit3, Trash2, ArrowRight 
} from 'lucide-vue-next'

const props = defineProps({
  events: Array,
  auth: Object
})

// ================= STATE =================
const search = ref('')
const statusFilter = ref('all')

// ================= FILTER =================
const filteredEvents = computed(() => {
  return props.events.filter(e => {
    const matchSearch = e.title?.toLowerCase().includes(search.value.toLowerCase())
    const matchStatus = statusFilter.value === 'all' || e.status === statusFilter.value
    return matchSearch && matchStatus
  })
})

// ================= BADGE SYSTEM (Premium Notion Style) =================
const statusBadge = (status) => {
  switch (status) {
    case 'published':
      return 'bg-emerald-50 text-emerald-700 border-emerald-200/60'
    case 'draft':
      return 'bg-slate-100 text-slate-600 border-slate-200/60'
    case 'finished':
      return 'bg-blue-50 text-blue-700 border-blue-200/60'
    default:
      return 'bg-slate-100 text-slate-600 border-slate-200/60'
  }
}

const formatDateTime = (date) => {
  if (!date) return '-'
  return new Intl.DateTimeFormat('id-ID', {
    weekday: 'short',
    day: 'numeric',
    month: 'short',
    year: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  }).format(new Date(date))
}

// ================= ACTION SWEETALERT LAMA DI-PRESERVE TOTAL =================
const deleteEvent = (id) => {
  Swal.fire({
    title: 'Hapus event?',
    text: 'Event akan dipindahkan ke trash',
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#006633', // Diganti ke Primary SIMAKU
    cancelButtonColor: '#ef4444',
    confirmButtonText: 'Ya, hapus',
    cancelButtonText: 'Batal'
  }).then((result) => {
    if (result.isConfirmed) {
      router.delete(`/events/${id}`, {
        preserveScroll: true,
        onSuccess: () => {
          toast.success('Event berhasil dihapus', {
            position: 'top-right',
            autoClose: 3000
          })
        },
        onError: () => {
          toast.error('Gagal menghapus event', {
            position: 'top-right',
            autoClose: 3000
          })
        }
      })
    }
  })
}

const isSuperAdmin = computed(() => {
  return props.auth?.user?.role === 'adminsuper'
})
</script>

<template>
  <Head title="Events Management - SIMAKU" />

  <AuthenticatedLayout>
    <template #header>
      <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 font-sans w-full">
        <div>
          <h2 class="text-2xl font-extrabold tracking-tight text-slate-900 md:text-3xl">Manajemen Kegiatan</h2>
          <p class="text-xs text-slate-500 mt-1">Kelola publikasi, penyusunan rundown, dan daftar kehadiran presensi acara UIN.</p>
        </div>

        <a href="/events/create" class="inline-flex items-center justify-center px-4 py-2.5 text-sm font-bold text-white bg-[#006633] rounded-xl hover:bg-[#008751] active:scale-98 transition-all shadow-md shadow-[#006633]/10 gap-2 self-stretch sm:self-auto">
          <Plus class="w-4 h-4" /> Buat Kegiatan Baru
        </a>
      </div>
    </template>

    <div class="relative min-h-screen bg-[#F8FAFC]">
      <div class="relative z-10 py-8 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 space-y-6 font-sans">

        <div class="bg-white border border-slate-200/80 rounded-2xl p-4 flex flex-col lg:flex-row gap-4 lg:items-center lg:justify-between shadow-xs">
          <div class="relative w-full lg:max-w-md">
            <Search class="absolute left-3.5 top-1/2 -translate-y-1/2 w-4 h-4 text-slate-400" />
            <input v-model="search" placeholder="Cari kegiatan akademis..." class="w-full pl-10 pr-4 py-2 text-sm bg-slate-50/50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-[#006633]/20 focus:border-[#006633] transition-all" />
          </div>

          <div class="flex items-center gap-1 overflow-x-auto pb-1 lg:pb-0 scrollbar-none">
            <button @click="statusFilter = 'all'" class="px-4 py-2 rounded-xl text-xs font-bold transition-all whitespace-nowrap" :class="statusFilter === 'all' ? 'bg-[#006633] text-white shadow-xs' : 'bg-slate-50 text-slate-600 hover:bg-slate-100 border border-slate-200/30'">All Events</button>
            <button @click="statusFilter = 'draft'" class="px-4 py-2 rounded-xl text-xs font-bold transition-all whitespace-nowrap" :class="statusFilter === 'draft' ? 'bg-[#006633] text-white shadow-xs' : 'bg-slate-50 text-slate-600 hover:bg-slate-100 border border-slate-200/30'">Draft</button>
            <button @click="statusFilter = 'published'" class="px-4 py-2 rounded-xl text-xs font-bold transition-all whitespace-nowrap" :class="statusFilter === 'published' ? 'bg-[#006633] text-white shadow-xs' : 'bg-slate-50 text-slate-600 hover:bg-slate-100 border border-slate-200/30'">Published</button>
            <button @click="statusFilter = 'finished'" class="px-4 py-2 rounded-xl text-xs font-bold transition-all whitespace-nowrap" :class="statusFilter === 'finished' ? 'bg-[#006633] text-white shadow-xs' : 'bg-slate-50 text-slate-600 hover:bg-slate-100 border border-slate-200/30'">Finished</button>
          </div>
        </div>

        <div v-if="filteredEvents.length" class="grid md:grid-cols-2 gap-6">
          <div v-for="event in filteredEvents" :key="event.hashid" class="group bg-white border border-slate-200/80 rounded-2xl shadow-xs hover:shadow-md hover:border-slate-300/80 transition-all duration-300 flex flex-col justify-between overflow-hidden">
            
            <div class="p-6 border-b border-slate-100 space-y-2">
              <div class="flex justify-between items-start gap-4">
                <h3 class="font-bold text-slate-900 tracking-tight text-base group-hover:text-[#006633] transition-colors leading-snug">
                  {{ event.title }}
                </h3>
                <span class="text-[10px] font-bold px-2.5 py-1 rounded-md border tracking-wide uppercase whitespace-nowrap shadow-xs" :class="statusBadge(event.status)">
                  {{ event.status }}
                </span>
              </div>
              <div class="text-[11px] font-medium text-slate-400 flex items-center gap-1">
                <Layers class="w-3 h-3" /> Token ID: <span class="font-mono bg-slate-50 px-1 py-0.5 rounded border border-slate-200/40 text-slate-500">{{ event.hashid }}</span>
              </div>
            </div>

            <div class="p-6 space-y-3 text-xs text-slate-600 font-medium flex-1">
              <div class="flex items-center gap-2.5 bg-slate-50/60 p-2.5 border border-slate-100 rounded-xl">
                <div class="text-base">📍</div>
                <span class="truncate text-slate-700 font-semibold">{{ event.venue?.name ?? 'Custom Location / Ruangan Luar' }}</span>
              </div>
              <div class="flex items-center gap-2.5 bg-slate-50/60 p-2.5 border border-slate-100 rounded-xl">
                <div class="text-base">📅</div>
                <span class="text-slate-700 font-semibold">{{ formatDateTime(event.start_date) }}</span>
              </div>
            </div>

            <div class="p-5 border-t border-slate-100 bg-slate-50/40 space-y-4">
              <div class="grid grid-cols-3 gap-2">
                <Link :href="`/events/${event.hashid}`" class="inline-flex items-center justify-center px-3 py-2 bg-white border border-slate-200 text-slate-700 hover:bg-[#006633] hover:text-white hover:border-[#006633] rounded-xl text-xs font-bold transition-all shadow-xs gap-1">
                  <Eye class="w-3.5 h-3.5" /> Detail
                </Link>

                <Link :href="`/events/${event.hashid}/sessions`" class="inline-flex items-center justify-center px-3 py-2 bg-white border border-slate-200 text-slate-700 hover:bg-[#006633] hover:text-white hover:border-[#006633] rounded-xl text-xs font-bold transition-all shadow-xs gap-1">
                  <FileText class="w-3.5 h-3.5" /> Rundown
                </Link>
                
                <Link :href="`/events/${event.hashid}/attendance`" class="inline-flex items-center justify-center px-3 py-2 bg-white border border-slate-200 text-slate-700 hover:bg-[#006633] hover:text-white hover:border-[#006633] rounded-xl text-xs font-bold transition-all shadow-xs gap-1">
                  <CheckSquare class="w-3.5 h-3.5" /> Absen
                </Link>
              </div>

              <div class="flex justify-between items-center text-xs pt-1">
                <div class="flex items-center gap-4 font-bold">
                  <Link :href="route('participant.index', event.hashid)" class="text-[#008751] hover:text-[#006633] flex items-center gap-1.5 transition-colors">
                    <Users2 class="w-3.5 h-3.5" /> Peserta
                  </Link>

                  <Link v-if="isSuperAdmin" :href="`/events/${event.hashid}/admins`" class="text-slate-600 hover:text-slate-900 flex items-center gap-1.5 transition-colors">
                    ⚙️ Hak Admin
                  </Link>
                </div>

                <div class="flex items-center gap-3 font-bold">
                  <Link :href="`/events/${event.hashid}/edit`" class="text-amber-600 hover:text-amber-700 flex items-center gap-1 transition-colors">
                    <Edit3 class="w-3.5 h-3.5" /> Edit
                  </Link>

                  <button @click="deleteEvent(event.hashid)" class="text-red-500 hover:text-red-700 flex items-center gap-0.5 border-none bg-transparent cursor-pointer font-bold transition-colors">
                    <Trash2 class="w-3.5 h-3.5" /> Hapus
                  </button>
                </div>
              </div>

            </div>
          </div>
        </div>

        <div v-else class="text-center py-20 bg-white border border-dashed border-slate-200 rounded-2xl max-w-md mx-auto shadow-xs">
          <div class="w-12 h-12 bg-slate-50 border border-slate-100 rounded-xl flex items-center justify-center text-slate-400 mx-auto mb-4">
            <SlidersHorizontal class="w-5 h-5" />
          </div>
          <h3 class="text-sm font-bold text-slate-900 tracking-tight">Tidak Ada Kegiatan</h3>
          <p class="text-xs text-slate-500 mt-1 max-w-xs mx-auto leading-relaxed">Kriteria pencarian atau status kegiatan yang Anda pilih tidak ditemukan.</p>
        </div>

      </div>
    </div>
  </AuthenticatedLayout>
</template>