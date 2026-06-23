<script setup>
import { Head, useForm } from '@inertiajs/vue3'
import { watch, ref, computed } from 'vue'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import { toast } from 'vue3-toastify'
import { UserRound, Sparkles, Camera, CreditCard, Shirt, HelpCircle, CheckCircle2 } from 'lucide-vue-next'
import { Cropper, CircleStencil } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css'

const showSizeChart = ref(false)

const props = defineProps({
  user: Object,
  profile: Object,
  institusi: Object
})

const form = useForm({
  nama_lengkap: props.profile?.nama_lengkap || props.user?.name || '',
  jenis_kelamin: props.profile?.jenis_kelamin || '',
  alamat: props.profile?.alamat || '',
  no_telp: props.profile?.no_telp || '',
  no_wa: props.profile?.no_wa || '',
  jabatan: props.profile?.jabatan || '',
  alamat_kantor: props.profile?.alamat_kantor || '',
  ukuran_baju: props.profile?.ukuran_baju || '',
  is_smoking: props.profile?.is_smoking ?? false,
  institusi_id: props.profile?.institusi_id || '',

  nama_bank: props.profile?.nama_bank || '',
  no_rekening: props.profile?.no_rekening || '',

  avatar: null
})

watch(() => form.same_as_phone, (val) => {
  if (val) {
    form.no_wa = form.no_telp
  }
})

watch(() => form.no_telp, (val) => {
  if (form.same_as_phone) {
    form.no_wa = val
  }
})

const submit = () => {
  form.post(route('profile.store'), {
    onSuccess: () => {
      toast.success('Profil berhasil disimpan')
    },
    onError: (errors) => {
      toast.error('Periksa kembali formulir kamu')
      console.log(errors)
    }
  })
}

////////////////// AVATAR CROPPER LOGIC PRESERVED //////////////////////

const cropResult = ref(null)
const onCropChange = (result) => {
  cropResult.value = result
}

const showAvatarModal = ref(false)
const image = ref(null)
const croppedImage = ref(null)

const confirmCrop = () => {
  if (!cropResult.value) return

  cropResult.value.canvas.toBlob((blob) => {
    const file = new File([blob], 'avatar.jpg', { type: 'image/jpeg' })
    form.avatar = file

    croppedImage.value = URL.createObjectURL(file)

    showAvatarModal.value = false
    image.value = null
    cropResult.value = null
  }, 'image/jpeg')
}

const onFileChange = (e) => {
  const file = e.target.files[0]
  if (!file) return

  const reader = new FileReader()
  reader.onload = (event) => {
    image.value = event.target.result
    showAvatarModal.value = true
  }
  reader.readAsDataURL(file)
}

const nukeCropper = () => {
  showAvatarModal.value = false
  image.value = null
}

const avatarUrl = computed(() => {
  if (croppedImage.value) return croppedImage.value
  if (props.profile?.avatar_url) return props.profile.avatar_url
  if (props.profile?.avatar) return `/storage/${props.profile.avatar}`
  return null
})
</script>

<template>
  <Head title="Lengkapi Profil Akun" />

  <AuthenticatedLayout>
    <div class="max-w-3xl mx-auto py-8 px-4 sm:px-6 lg:px-8 font-sans bg-[#F8FAFC] min-h-screen">

      <!-- TOP HEADER BANNER (Premium SaaS Design) -->
      <div class="relative overflow-hidden bg-white border border-slate-200 rounded-2xl p-6 mb-6 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 shadow-sm">
        <div class="flex items-center gap-4">
          <div class="w-12 h-12 rounded-xl bg-[#006633] flex items-center justify-center shadow-md shadow-[#006633]/10 flex-shrink-0">
            <UserRound class="w-5 h-5 text-white" />
          </div>
          <div>
            <div class="flex items-center gap-1.5 text-xs font-semibold text-[#006633] mb-1">
              <Sparkles class="w-3.5 h-3.5 text-[#D4AF37] fill-[#D4AF37]" /> Data Otentikasi Pengguna
            </div>
            <h1 class="text-xl font-extrabold tracking-tight text-slate-900">Lengkapi Profil Akun Anda</h1>
            <p class="text-xs text-slate-500 mt-0.5">Informasi profil resmi untuk integrasi registrasi, sertifikat, dan agenda SIMAKU.</p>
          </div>
        </div>
        <div class="bg-slate-50 px-3 py-1.5 rounded-xl border border-slate-100 flex items-center gap-2 text-xs font-medium text-slate-600 self-stretch sm:self-auto justify-center">
          <span class="w-2 h-2 rounded-full bg-emerald-500 animate-ping"></span> Profile Verification
        </div>
      </div>

      <!-- MAIN CONFIGURATION CARD CONTAINER -->
      <div class="bg-white border border-slate-200/80 rounded-2xl p-6 sm:p-8 shadow-xs space-y-6">

        <!-- AVATAR CAPTURE SECTOR -->
        <div class="flex items-center gap-5 bg-slate-50/50 border border-slate-100 p-4 rounded-xl">
          <div class="relative">
            <div class="w-20 h-20 rounded-full bg-white overflow-hidden border-2 border-[#006633] flex items-center justify-center shadow-xs">
              <img v-if="avatarUrl" :src="avatarUrl" class="w-full h-full object-cover" />
              <div v-else class="w-full h-full flex items-center justify-center bg-emerald-50 text-[#006633] font-bold text-xl uppercase">
                {{ form.nama_lengkap ? form.nama_lengkap.charAt(0) : 'U' }}
              </div>
            </div>
            <!-- UPLOAD FLOATING BUTTON -->
            <label class="absolute bottom-0 right-0 bg-[#006633] hover:bg-[#008751] text-white p-2 rounded-full cursor-pointer shadow-sm transition-colors border border-white">
              <Camera class="w-3.5 h-3.5" />
              <input type="file" class="hidden" @change="onFileChange" />
            </label>
          </div>
          <div>
            <h3 class="font-bold text-slate-900 text-sm">Foto Profil Formal</h3>
            <p class="text-xs text-slate-500 mt-0.5">Format file .jpg atau .png. Klik ikon kamera untuk mengunggah dan memotong foto profil Anda.</p>
          </div>
        </div>

        <!-- FORM DATA CORE FIELDS -->
        <div class="space-y-4">
          <!-- Nama Lengkap -->
          <div>
            <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Nama Lengkap Sesuai Identitas</label>
            <input v-model="form.nama_lengkap" class="input-premium" placeholder="Masukkan nama lengkap beserta gelar" />
            <p v-if="form.errors.nama_lengkap" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.nama_lengkap }}</p>
          </div>

          <div class="grid sm:grid-cols-2 gap-4">
            <!-- Jenis Kelamin -->
            <div>
              <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Jenis Kelamin</label>
              <select v-model="form.jenis_kelamin" class="select-premium">
                <option value="">Pilih Jenis Kelamin</option>
                <option value="L">Laki-laki</option>
                <option value="P">Perempuan</option>
              </select>
              <p v-if="form.errors.jenis_kelamin" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.jenis_kelamin }}</p>
            </div>

            <!-- Institusi Kampus/Organisasi -->
            <div>
              <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Afiliasi Institusi Kampus</label>
              <select v-model="form.institusi_id" class="select-premium">
                <option value="">Pilih Asal Institusi</option>
                <template v-for="(items, zona) in institusi" :key="zona">
                  <optgroup :label="zona" class="font-bold text-slate-400 bg-white">
                    <option v-for="item in items" :key="item.id" :value="item.id">
                      {{ item.nama_institusi }}
                    </option>
                  </optgroup>
                </template>
              </select>
              <p v-if="form.errors.institusi_id" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.institusi_id }}</p>
            </div>
          </div>

          <!-- Alamat Tinggal -->
          <div>
            <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Alamat Tempat Tinggal Saat Ini</label>
            <textarea v-model="form.alamat" rows="2" class="input-premium py-2" placeholder="Tuliskan alamat lengkap domisili aktif"></textarea>
            <p v-if="form.errors.alamat" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.alamat }}</p>
          </div>

          <!-- Telepon & WhatsApp Block -->
          <div class="bg-slate-50/40 border border-slate-100 p-4 rounded-xl space-y-4">
            <div class="grid sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Nomor Telepon Seluler</label>
                <input v-model="form.no_telp" class="input-premium" placeholder="Contoh: 081234567890" />
                <p v-if="form.errors.no_telp" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.no_telp }}</p>
              </div>

              <div>
                <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Nomor WhatsApp Aktif</label>
                <input v-model="form.no_wa" :disabled="form.same_as_phone" class="input-premium" placeholder="Contoh: 081234567890" />
                <p v-if="form.errors.no_wa" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.no_wa }}</p>
              </div>
            </div>

            <!-- Sinkronisasi Checkbox WA -->
            <label class="inline-flex items-center gap-2 text-xs font-semibold text-slate-600 select-none cursor-pointer">
              <input type="checkbox" v-model="form.same_as_phone" class="rounded border-slate-300 text-[#006633] focus:ring-[#006633] w-4 h-4">
              Nomor WhatsApp sama dengan Nomor Telepon Seluler
            </label>
          </div>

          <!-- Jabatan & Alamat Kantor -->
          <div class="grid sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Jabatan / Profesi</label>
              <input v-model="form.jabatan" class="input-premium" placeholder="Contoh: Mahasiswa, Dosen, Peneliti" />
              <p v-if="form.errors.jabatan" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.jabatan }}</p>
            </div>

            <div>
              <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Alamat Kantor / Fakultas</label>
              <input v-model="form.alamat_kantor" class="input-premium" placeholder="Fakultas / Unit Kerja" />
              <p v-if="form.errors.alamat_kantor" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.alamat_kantor }}</p>
            </div>
          </div>

          <!-- FINANCIAL SECTOR BLOCK -->
          <div class="border-t border-slate-100 pt-5 mt-2 space-y-4">
            <div class="flex items-center gap-2 mb-1">
              <CreditCard class="w-4 h-4 text-slate-400" />
              <span class="text-xs font-bold text-slate-800 tracking-wide uppercase">Informasi Akun Rekening Bank</span>
              <span class="text-[10px] bg-slate-100 text-slate-500 font-bold px-1.5 py-0.5 rounded border">Keperluan Administrasi</span>
            </div>

            <div class="grid sm:grid-cols-2 gap-4">
              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Nama Lembaga Perbankan</label>
                <select v-model="form.nama_bank" class="select-premium">
                  <option value="">Pilih Bank Penerima</option>
                  <option>BCA</option>
                  <option>BRI</option>
                  <option>BNI</option>
                  <option>Mandiri</option>
                  <option>BSI</option>
                  <option>CIMB Niaga</option>
                  <option>BTN</option>
                </select>
                <p v-if="form.errors.nama_bank" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.nama_bank }}</p>
              </div>

              <div>
                <label class="block text-xs font-medium text-slate-600 mb-1.5">Nomor Rekening Akurat</label>
                <input v-model="form.no_rekening" class="input-premium" placeholder="Masukkan angka tanpa spasi/tanda baca" />
                <p v-if="form.errors.no_rekening" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.no_rekening }}</p>
              </div>
            </div>
          </div>

          <!-- MERCHANDISE SECTOR BLOCK -->
          <div class="border-t border-slate-100 pt-5 grid sm:grid-cols-2 gap-4">
            <!-- Ukuran Baju -->
            <div>
              <div class="flex items-center justify-between mb-1.5">
                <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase">Dimensi Ukuran Baju</label>
                <button type="button" @click="showSizeChart = true" class="inline-flex items-center gap-1 text-[11px] font-bold text-[#006633] hover:underline bg-transparent border-none cursor-pointer">
                  <HelpCircle class="w-3 h-3" /> Lihat Panduan Ukuran
                </button>
              </div>
              <select v-model="form.ukuran_baju" class="select-premium">
                <option value="">Pilih Ukuran</option>
                <option>M</option>
                <option>L</option>
                <option>XL</option>
                <option>XXL</option>
                <option>XXXL</option>
              </select>
              <p v-if="form.errors.ukuran_baju" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.ukuran_baju }}</p>
            </div>

            <!-- Smoking Preference -->
            <div>
              <label class="block text-xs font-bold text-slate-700 tracking-wide uppercase mb-1.5">Preferensi Ruangan Akomodasi</label>
              <select v-model="form.is_smoking" class="select-premium">
                <option :value="0">Kamar Bebas Asap Rokok (Non-Smoking)</option>
                <option :value="1">Kamar Diperbolehkan Merokok (Smoking Room)</option>
              </select>
              <p v-if="form.errors.is_smoking" class="text-red-500 text-xs mt-1 font-semibold">{{ form.errors.is_smoking }}</p>
            </div>
          </div>
        </div>

        <!-- CONSOLE OPERATION ACTION EXECUTE SUBMIT -->
        <div class="pt-4">
          <button @click="submit" class="w-full inline-flex items-center justify-center px-4 py-3 bg-[#006633] hover:bg-[#008751] text-white font-bold text-sm rounded-xl shadow-md shadow-[#006633]/10 border-none cursor-pointer active:scale-98 transition-all" :disabled="form.processing">
            Simpan Konfigurasi Profil Resmi
          </button>
        </div>

      </div>
    </div>
  </AuthenticatedLayout>

  <!-- SIZE CHART MODERN LIGHTBOX MODAL -->
  <div v-if="showSizeChart" class="fixed inset-0 bg-black/60 backdrop-blur-xs flex items-center justify-center z-50 p-4" @click.self="showSizeChart = false">
    <div class="bg-white rounded-2xl shadow-xl max-w-lg w-full p-5 relative border border-slate-200">
      <button class="absolute top-4 right-4 text-slate-400 hover:text-red-500 text-sm font-bold bg-transparent border-none cursor-pointer" @click="showSizeChart = false">
        ✕ Tutup
      </button>
      <h2 class="text-base font-bold text-slate-900 flex items-center gap-2 mb-4">
        <Shirt class="w-4 h-4 text-[#006633]" /> Standar Spesifikasi Ukuran Baju
      </h2>
      <img src="/images/size-chart.jpg" alt="Size Chart" class="w-full rounded-xl border border-slate-100 object-contain max-h-[60vh]" />
    </div>
  </div>

  <!-- AVATAR PHOTO ADVANCED CROPPER LIGHTBOX MODAL -->
  <div v-if="showAvatarModal" class="fixed inset-0 bg-black/70 backdrop-blur-xs flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-2xl w-full max-w-md shadow-xl overflow-hidden border border-slate-200">
      <div class="p-4 border-b border-slate-100 flex items-center justify-between">
        <h2 class="text-sm font-bold text-slate-800">Sesuaikan Komposisi Foto Profil</h2>
        <button class="text-slate-400 hover:text-red-500 text-sm font-bold bg-transparent border-none cursor-pointer" @click="nukeCropper">✕</button>
      </div>

      <div class="p-4 bg-slate-900/5">
        <Cropper v-if="image" class="h-72 rounded-xl overflow-hidden bg-slate-950" :src="image" :stencil-component="CircleStencil" @change="onCropChange" />
      </div>

      <div class="p-4 border-t border-slate-100 bg-white flex flex-col gap-2">
        <button class="w-full bg-[#006633] hover:bg-[#008751] text-white py-2.5 rounded-xl text-sm font-bold border-none cursor-pointer transition-all" @click="confirmCrop">
          Potong & Terapkan Foto
        </button>
        <button class="w-full bg-slate-50 hover:bg-slate-100 text-slate-600 py-2.5 rounded-xl text-sm font-semibold border-none cursor-pointer transition-all" @click="nukeCropper">
          Batalkan
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.input-premium {
  width: 100%;
  padding: 11px 14px;
  background-color: #F8FAFC;
  border-radius: 12px;
  border: 1px solid #E2E8F0;
  outline: none;
  font-size: 0.875rem;
  transition: all 0.2s ease;
  color: #0F172A;
}
.input-premium:focus {
  border-color: #006633;
  background-color: #FFFFFF;
  box-shadow: 0 0 0 3px rgba(0, 102, 51, 0.08);
}
.select-premium {
  width: 100%;
  padding: 11px 14px;
  background-color: #F8FAFC;
  border-radius: 12px;
  border: 1px solid #E2E8F0;
  outline: none;
  font-size: 0.875rem;
  transition: all 0.2s ease;
  color: #0F172A;
  appearance: none;
  background-image: url("data:image/svg+xml;charset=UTF-8,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%2364748B' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpolyline points='6 9 12 15 18 9'%3E%3C/polyline%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 14px center;
  background-size: 16px;
}
.select-premium:focus {
  border-color: #006633;
  background-color: #FFFFFF;
  box-shadow: 0 0 0 3px rgba(0, 102, 51, 0.08);
}
</style>