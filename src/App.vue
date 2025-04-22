<script setup>
import { ref } from 'vue';

const artikli = ref([]);

const naziv = ref("");
const kolicina = ref(1);
const cijena = ref(0);

function dodajProizvod() {
  const nazivUnosa = naziv.value.trim().toLowerCase();

  const existing = artikli.value.find(a => a.naziv.toLowerCase() === nazivUnosa);

  if (existing) {
    existing.kolicina++;
    existing.cijena = cijena.value.toFixed(2);
    existing.ukupno += existing.cijena;
  } else {
    const ukupno = cijena.value;
    artikli.value.push({
      naziv: naziv.value,
      kolicina: kolicina.value,
      cijena: cijena.value.toFixed(2),
      ukupno: ukupno.toFixed(2)
    });
  }

  naziv.value = "";
  cijena.value = 0;
}

function povecaj(artikl) {
  artikl.kolicina++;
  artikl.ukupno = (artikl.cijena * artikl.kolicina).toFixed(2);
}
function smanji(artikl) {
  if (artikl.kolicina > 1) {
    artikl.kolicina--;
    artikl.ukupno = (artikl.cijena * artikl.kolicina).toFixed(2);
  }
}
</script>

<template>
    <div class="flex flex-col bg-gray-100 p-20">
        <p class="font-bold text-3xl mb-8">Košarica</p>
        <div class="flex flex-row items-center border-y-1 py-4">
            <span>Naziv proizvoda:</span>
            <input type="text" v-model="naziv" class="border bg-slate-50 border-slate-300 rounded shadow p-2 mx-5" placeholder="Upiši naziv proizvoda" />
            <span>Cijena proizvoda:</span>
            <input type="number" v-model="cijena" class="border bg-slate-50 border-slate-300 rounded shadow p-2 mx-5" placeholder="Upiši cijenu proizvoda" min="0" />
            <button
  :class="[
    'px-4 py-2 rounded transition',
    (!naziv || cijena <= 0)
      ? 'bg-gray-300 cursor-not-allowed'
      : 'bg-green-300 hover:bg-green-400 cursor-pointer'
  ]"
  id="botun"
  :disabled="!naziv || cijena <= 0"
  @click="dodajProizvod"
>Dodaj artikl</button>
        </div>
        <div>
            <table class="table-auto w-9/10 mt-5 border-separate border-spacing-2">
                <thead>
                    <tr class="mb-18">
                        <th>Naziv</th>
                        <th>Količina</th>
                        <th>Cijena</th>
                        <th>Ukupno</th>
                        <th></th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(artikl, index) in artikli">                        
                        <td class="text-center">{{ artikl.naziv }}</td>
                        <td class="text-center"><span class="text-2xl font-bold" :class="Number(artikl.kolicina) === 1 ? 'cursor-not-allowed' : 'cursor-pointer'"
                          @click="smanji(artikl)">-</span><input class="border bg-slate-50 border-slate-300 rounded shadow p-2 w-12 mx-5 text-center" type="text" v-model="artikl.kolicina"><span class="text-2xl font-bold cursor-pointer" @click="povecaj(artikl)">+</span></td>
                        <td class="text-center">{{ artikl.cijena }} €</td>
                        <td class="text-center">{{ artikl.ukupno }} €</td>
                        <td class="text-center"><button class="bg-red-300 hover:bg-red-400 px-2 py-1 rounded" @click="artikli.splice(index, 1)">Ukloni</button></td>
                    </tr>

                    <tr v-if="artikli.length === 0">
                        <td colspan="5" class="text-center text-gray-500 italic py-4">
                            Nema podataka
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="mt-5">
            <p class="font-bold text-xl">Ukupno: {{ artikli.reduce((acc, artikl) => acc + Number(artikl.ukupno), 0).toFixed(2) }} €</p>
          </div>
    </div>
</template>

<style scoped>

</style>
