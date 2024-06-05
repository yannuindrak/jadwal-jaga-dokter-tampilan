<template>
  <!-- {{ tampungKSM }} -->
  <!-- 
  <div align="center" mb="50">
    <h1 class="text-lg font-bold text-white">JADWAL JAGA DOKTER IGD</h1>
  </div>

  <div align="center" mb="50">
    <h2 class="text-lg font-bold text-white"><jam /></h2>
  </div>
  <br /> -->

  <!-- <v-divider :thickness="4" class="border-opacity-50" color="black"></v-divider> -->
  <br />
  <split-carousel cycle :interval="1500" height="550px" hide-delimiters>
    <div class="d-flex flex-no-wrap justify-space-between">
      <split-carousel-item v-for="(bebas, i) in tampungKSM" :key="i" dense>
        <v-row align="center" justify="center" dense>
          <v-col align="center" cols="12" md="2" dense>
            <v-card
              variant="tonal"
              width="425px"
              height="520px"
              class="bg-white rounded-xl border-surface-variant"
              id="warnacard"
            >
              <!-- Nama KSM -->
              <v-sheet
                align="center"
                height="100px"
                class="font-bold text-white"
                id="warnasheet"
                ><br />
                <h3 align="center">
                  {{ bebas.ket_ksm }}
                </h3>
              </v-sheet>

              <v-card-text
                v-for="(jadwal, indexJadwal) in bebas.jadwal"
                :key="indexJadwal"
                rounded="xl"
                id="warnacard2"
              >
                <v-sheet
                  v-if="
                    new Date() >= new Date(jadwal.Jaga_awal) &&
                    new Date() <= new Date(jadwal.Jaga_akhir)
                  "
                >
                  <v-sheet-item id="warnacard3">
                    <h2>{{ jadwal.Level.Nama_level_igd }}</h2>
                  </v-sheet-item>
                  <v-sheet-item id="warnacard4">
                    <h3>{{ jadwal.Nama_petugas }}</h3>
                  </v-sheet-item>
                </v-sheet>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </split-carousel-item>
    </div>
  </split-carousel>
  <v-footer></v-footer>
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
// const { data: daftar_ksm, } = await useFetch(() => 'https://satu.dev.rssa.id/items/daftar_ksm');

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
  const _dataKSM = await $fetch(`https://satu.dev.rssa.id/items/daftar_ksm`);
  const ksm = _dataKSM.data;
  const _jdwlDokter = await $fetch(
    `https://satu.dev.rssa.id/items/data_jadwal_jaga_dokter?fields=id,Nama_petugas,Ksm,Level.Nama_level_igd,Jaga_awal,Jaga_akhir&filter[Jaga_awal][_between]=[${tanggalKemarin}, ${tanggalBesok}]`
  );
  const jdwlDokters = _jdwlDokter.data;

  ksm.forEach((_ksm) => {
    let idksm = _ksm["id"];

    let filterjdwl = jdwlDokters.filter((jadwal) => jadwal["Ksm"] === idksm);
    if (filterjdwl && filterjdwl.length > 0) {
      let tampiljadwalBaru = {
        idksm: _ksm["id"],
        ket_ksm: _ksm["Nama_ksm"],
        jadwal: [],
      };
      const sekarang = new Date();
      filterjdwl.forEach((jdwl) => {
        const awal = new Date(jdwl.Jaga_awal);
        const akhir = new Date(jdwl.Jaga_akhir);
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

<style>
#warnasheet {
  background: linear-gradient(180deg, #315f43 0%, #44f57a 100%);
  text-transform: uppercase;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
#warnacard {
  background-color: transparent;
}

#warnacard3 {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  font-style: unset;
  text-transform: uppercase;
  text-align: left;
}

#warnacard2 {
}

#warnacard4 {
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
  text-align: left;
}
/* #warnaitemcard {
  background: white;
} */
</style>
background-color: #315f43; background-image: linear-gradient(180deg, #315f43 0%,
#44f57a 100%);
