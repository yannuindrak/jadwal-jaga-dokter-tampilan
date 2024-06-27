<style scoped>
@import url("~/assets/css/dashboard.css");
</style>
<template>
  <split-carousel
    cycle
    :interval="1000"
    height="620px"
    width="600px"
    hide-delimiters
    :speed="400"
  >
    <div class="d-flex flex-no-wrap justify-space-between">
      <split-carousel-item v-for="(bebas, i) in tampungKSM" :key="i" dense>
        <v-row align="center" justify="center" dense>
          <v-col align="center" cols="12" md="2" dense>
            <v-card
              variant="flat"
              width="420px"
              height="580px"
              class="bg-white rounded-xl"
              id="warnacard"
              elevation="16"
            >
              <!-- Nama KSM -->
              <v-sheet
                align="center"
                height="100px"
                class="font-bold text-white"
                id="warnasheet"
                ><br />
                <h2 align="center">
                  {{ bebas.ket_ksm }}
                </h2>
              </v-sheet>

              <v-card-text
                v-for="(jadwal, indexJadwal) in bebas.jadwal"
                :key="indexJadwal"
                rounded="xl"
                id="warnacard2"
              >
                <v-sheet
                  v-if="
                    new Date() >= new Date(jadwal.Tanggal_awal) &&
                    new Date() <= new Date(jadwal.Tanggal_akhir)
                  "
                  rounded="xl"
                  align="left"
                >
                  <v-sheet-item class="mt-2 rounded-xl" id="vsheetitem">
                    <h2>{{ jadwal.Tingkat_supervisi }}</h2>
                  </v-sheet-item>
                  <!-- Tampil nama + gelar -->
                  <h3 id="namasheetitem">
                    <v-sheet-item
                      class="mt-2"
                      v-for="(gelar, index) in jadwal.Nama_dpjp.Gelar_depan"
                      :key="index"
                    >
                      {{ gelar.daftar_gelar_depan_id.Gelar_depan }}
                    </v-sheet-item>
                    {{ jadwal.Dokter_jaga }},
                    <v-sheet-item
                      class="mt-2"
                      v-for="(gelar, index) in jadwal.Nama_dpjp.Gelar_belakang"
                      :key="index"
                    >
                      {{ gelar.daftar_gelar_belakang_id.Gelar_belakang }}
                    </v-sheet-item>
                  </h3>

                  <v-divider class="border-opacity-70"></v-divider>
                  <!-- End of tampil nama + gelar -->
                </v-sheet>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </split-carousel-item>
    </div>
  </split-carousel>
</template>

<script>
import { SplitCarousel, SplitCarouselItem } from "vue-split-carousel";
export default {
  components: {
    SplitCarousel,
    SplitCarouselItem,
  },
};
</script>
<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const waktuSekarang = ref({
  tanggal: new Date().getDate(),
  bulan: new Date().getMonth() + 1, // Bulan dimulai dari 0, jadi tambahkan 1
  tahun: new Date().getFullYear(),
  jam: new Date().getHours(),
  menit: new Date().getMinutes(),
  detik: new Date().getSeconds(),
});

const formatDuaDigit = (nilai) => (nilai <= 9 ? `0${nilai}` : nilai);

const updateWaktu = () => {
  const sekarang = new Date();
  waktuSekarang.value.tanggal = formatDuaDigit(sekarang.getDate());
  waktuSekarang.value.bulan = formatDuaDigit(sekarang.getMonth() + 1);
  waktuSekarang.value.tahun = sekarang.getFullYear();
  waktuSekarang.value.jam = formatDuaDigit(sekarang.getHours());
  waktuSekarang.value.menit = formatDuaDigit(sekarang.getMinutes());
  waktuSekarang.value.detik = formatDuaDigit(sekarang.getSeconds());
};

const tanggalSekarang = new Date();
const tanggalKemarin = new Date(
  tanggalSekarang.getFullYear(),
  tanggalSekarang.getMonth(),
  tanggalSekarang.getDate() - 7
)
  .toISOString()
  .split("T")[0];
const tanggalBesok = new Date(
  tanggalSekarang.getFullYear(),
  tanggalSekarang.getMonth(),
  tanggalSekarang.getDate() + 7
)
  .toISOString()
  .split("T")[0];

let tampungKSM = ref([]);
const updateKSM = async () => {
  console.log("ini refresh");
  tampungKSM.value = []; // Kosongkan array sebelum mengisi ulang
  const _dataKSM = await $fetch(`https://satu.rssa.top/items/daftar_ksm`);
  const ksm = _dataKSM.data;
  const _jdwlDokter = await $fetch(
    `https://satu.rssa.top/items/data_jadwal_jaga_igd?fields=id,Dokter_jaga,KSM,Tanggal_awal,Tanggal_akhir,Tingkat_supervisi,Nama_dpjp.Gelar_depan.daftar_gelar_depan_id.Gelar_depan,Nama_dpjp.Gelar_belakang.daftar_gelar_belakang_id.Gelar_belakang&filter[Tanggal_awal][_between]=[${tanggalKemarin}, ${tanggalBesok}]`
  );

  const jdwlDokters = _jdwlDokter.data;

  ksm.forEach((_ksm) => {
    let idksm = _ksm["id"];

    let filterjdwl = jdwlDokters.filter((jadwal) => jadwal["KSM"] === idksm); //ksm berisi jadwal di dalamnya
    if (filterjdwl && filterjdwl.length > 0) {
      //logikanya berarti, utk nama & gelar
      let tampiljadwalBaru = {
        //1 nama mempunyai beberapa gelar
        idksm: _ksm["id"],
        ket_ksm: _ksm["Nama_ksm"],
        jadwal: [],
      };
      const sekarang = new Date();
      filterjdwl.forEach((jdwl) => {
        const awal = new Date(jdwl.Tanggal_awal);
        const akhir = new Date(jdwl.Tanggal_akhir);
        if (sekarang >= awal && sekarang <= akhir) {
          tampiljadwalBaru.jadwal.push(jdwl);
        }
      });
      if (tampiljadwalBaru.jadwal.length > 0) {
        tampungKSM.value.push(tampiljadwalBaru);
      }
    }
  });
  setTimeout(updateKSM, 60000); // Jadwalkan pembaruan berikutnya setiap 10 detik
};

updateKSM(); // Panggilan awal untuk memulai proses
</script>
