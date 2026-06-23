<script setup>
import { Button } from '@/Components/ui/button';
import { Card, CardContent } from '@/Components/ui/card';
import { Input } from '@/Components/ui/input';
import { Label } from '@/Components/ui/label';
import InputError from '@/Components/InputError.vue';
import GuestLayout from '@/Layouts/GuestLayout.vue';
import { Head, useForm, usePage } from '@inertiajs/vue3';
import { ShieldCheck, Mail, Lock, Sparkles, ArrowRight } from 'lucide-vue-next';

const page = usePage()

const form = useForm({
    email: '',
    password: '',
});

const submit = () => {
    form.post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};

const redirectGoogle = () => {
  window.location.href = route('auth.google')
}
</script>

<template>
  <GuestLayout>
    <Head title="Masuk Otentikasi - SIMAKU" />

    <div class="min-h-screen bg-[#F8FAFC] flex flex-col justify-center py-12 sm:px-6 lg:px-8 font-sans selection:bg-[#006633]/10 selection:text-[#006633]">
      
      <!-- Ambient decorative glow behind the card -->
      <div class="absolute inset-0 bg-[radial-gradient(ellipse_at_top,rgba(0,102,51,0.03),transparent_50%)] pointer-events-none"></div>

      <div class="sm:mx-auto sm:w-full sm:max-w-md text-center px-4 relative z-10">
        <!-- SIMAKU Mini Hexagon Logo Badge -->
        <div class="mx-auto w-12 h-12 rounded-2xl bg-[#006633] flex items-center justify-center shadow-lg shadow-[#006633]/20 mb-5">
          <span class="text-white font-bold text-xl tracking-tighter">S</span>
        </div>
        
        <h2 class="text-2xl font-extrabold tracking-tight text-slate-900 sm:text-3xl">
          Selamat Datang Kembali
        </h2>
        <p class="mt-2 text-sm text-slate-500">
          Silakan masuk untuk mengakses pusat kendali manajemen aktivitas terpadu.
        </p>
      </div>

      <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md px-4 relative z-10">
        
        <!-- ERROR FLASH CONSOLE ALERT -->
        <div
          v-if="page.props.flash?.error"
          class="mb-5 rounded-xl bg-red-50 p-3.5 text-xs text-red-700 border border-red-200/60 flex items-start gap-2 shadow-xs font-semibold"
        >
          <span class="text-base leading-none">⚠️</span>
          <span>{{ page.props.flash.error }}</span>
        </div>

        <Card class="bg-white border border-slate-200/80 rounded-2xl shadow-xs overflow-hidden">
          <CardContent class="p-6 sm:p-10">

            <form @submit.prevent="submit" class="flex flex-col gap-5">

              <!-- USERNAME / EMAIL FIELD -->
              <div class="grid gap-2">
                <Label for="email" class="text-xs font-bold text-slate-700 tracking-wide uppercase">Identitas Username / Email</Label>
                <div class="relative mt-0.5">
                  <Mail class="absolute left-3.5 top-1/2 -translate-y-1/2 w-4 h-4 text-slate-400" />
                  <Input
                    id="email"
                    type="text"
                    v-model="form.email"
                    required
                    autofocus
                    class="block w-full pl-10 pr-4 py-2.5 bg-slate-50/50 border border-slate-200 rounded-xl focus-visible:ring-2 focus-visible:ring-[#006633]/20 focus-visible:border-[#006633] text-sm transition-all"
                    placeholder="Masukkan username atau email resmi"
                  />
                </div>
                <InputError :message="form.errors.email" class="text-xs text-red-500 mt-1" />
              </div>

              <!-- PASSWORD FIELD -->
              <div class="grid gap-2">
                <Label for="password" class="text-xs font-bold text-slate-700 tracking-wide uppercase">Kata Sandi Rahasia</Label>
                <div class="relative mt-0.5">
                  <Lock class="absolute left-3.5 top-1/2 -translate-y-1/2 w-4 h-4 text-slate-400" />
                  <Input
                    id="password"
                    type="password"
                    v-model="form.password"
                    required
                    class="block w-full pl-10 pr-4 py-2.5 bg-slate-50/50 border border-slate-200 rounded-xl focus-visible:ring-2 focus-visible:ring-[#006633]/20 focus-visible:border-[#006633] text-sm transition-all"
                    placeholder="••••••••"
                  />
                </div>
                <InputError :message="form.errors.password" class="text-xs text-red-500 mt-1" />
              </div>

              <!-- NATIVE PREMIUM LOGIN SUBMIT BUTTON -->
              <Button
                type="submit"
                class="w-full inline-flex items-center justify-center px-4 py-6 bg-[#006633] hover:bg-[#008751] text-white font-bold text-sm rounded-xl shadow-md shadow-[#006633]/10 border-none cursor-pointer group transition-all mt-2"
                :disabled="form.processing"
              >
                <span>Masuk Aplikasi</span>
                <ArrowRight class="w-4 h-4 ml-1 opacity-80 group-hover:translate-x-0.5 transition-transform" />
              </Button>

              <!-- LIGHT MATRIX SEPARATOR -->
              <div class="relative flex py-2 items-center">
                <div class="flex-grow border-t border-slate-100"></div>
                <span class="flex-shrink mx-4 text-slate-400 text-[10px] font-bold tracking-wider uppercase">Atau Metode Lain</span>
                <div class="flex-grow border-t border-slate-100"></div>
              </div>

              <!-- GOOGLE GATEWAY SINGLE SIGN ON LINK -->
              <Button
                type="button"
                variant="outline"
                class="w-full flex items-center justify-center gap-2.5 py-6 border border-slate-200 bg-white hover:bg-slate-50 rounded-xl text-slate-700 font-semibold text-sm transition-all cursor-pointer shadow-xs"
                @click="redirectGoogle"
              >
                <svg class="h-4 w-4" viewBox="0 0 533.5 544.3" xmlns="http://www.w3.org/2000/svg">
                  <path fill="#4285f4" d="M533.5 278.4c0-17.4-1.6-34.1-4.6-50.4H272.1v95.4h147c-6.3 34.3-25 63.4-53.4 83v68h86.4c50.6-46.6 81.4-115.3 81.4-196z"/>
                  <path fill="#34a853" d="M272.1 544.3c72.4 0 133.1-23.9 177.5-64.9l-86.4-68c-24 16.1-54.7 25.6-91.1 25.6-69.9 0-129.1-47.2-150.4-110.7h-89.5v69.5c44.2 87.3 135.6 148.5 239.9 148.5z"/>
                  <path fill="#fbbc04" d="M121.7 326.3c-10.9-32.8-10.9-68.2 0-101l-89.5-69.5c-39.1 77.9-39.1 162.6 0 240.5z"/>
                  <path fill="#ea4335" d="M272.1 107.7c38.6 0 73.2 13.3 100.5 39.5l75.4-75.4C405.2 24.6 344.5 0 272.1 0 167.8 0 76.3 61.2 32.1 148.5l89.6 69.5c21.3-63.5 80.5-110.7 150.4-110.7z"/>
                </svg>
                <span>Autentikasi Akun Google</span>
              </Button>

            </form>

          </CardContent>
        </Card>

        <!-- MODERN SAAS COMPLIANCE FOOTER -->
        <div class="mt-6 flex items-center justify-center gap-1.5 text-[11px] font-medium text-slate-400 text-center">
          <ShieldCheck class="w-3.5 h-3.5 text-emerald-600" />
          SIMAKU Enterprise Security Gateway Verified
        </div>
      </div>

    </div>
  </GuestLayout>
</template>