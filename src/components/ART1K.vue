<template>
  <div class="container">
    <button @click="copy()">Zaznacz tabele</button>
    <p><b>Artykuły do profili z kolorami BIAŁE-DEKOR</b></p>
    <table id="tabela">
      <thead>
        <td>Kod</td>
        <td>Opis</td>
        <td>Cena</td>
      </thead>
      <tbody>
        <tr v-for="elem of this.output" :key="elem.kod">
          <td>{{elem.kod}}</td>
          <td>{{elem.opis}}</td>
          <td>{{elem.cena}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import kolory from '../kolory.js'


export default {
  data() {
    return {
      output: [],
      profile: [
        // { kod: "3110", cena: 1.95, cenab: 0.94},
        // { kod: "5524", cena: 1.84, cenab: 1.35 },
        // { kod: "5525", cena: 3.11, cenab: 1.35 },
        // { kod: "5526", cena: 2.96, cenab: 1.44 },
        // { kod: "6124", cena: 1.55, cenab: 1.02 },
        // { kod: "6126", cena: 1.95, cenab: 0.98 },
        // { kod: "6128", cena: 1.93, cenab:0.95  },
        // { kod: "6130", cena: 1.93, cenab: 0.96 },
        // { kod: "6132", cena: 1.93, cenab: 0.96 },
       // { kod: "8300", cena: 3.58, cenab: 2.33 },
        //{ kod: "8309", cena: 3.3, cenab: 3.3 },
        // { kod: "5869", cena: 3.86, cenab: 3.3 },
        // {
        //   "kod": 3284,
        //   opis:'Łącznik H 3284',
        //   "cenab": 0.62,
        //   "cena": 1.37
        // },
        // {
        //   "kod": 5204,
        //   opis:'Łącznik statyczny 5204',
        //   "cenab": 3.36,
        //   "cena": 4.69
        // },
        {
          "kod": 2325,
          opis:'Ćwierćwałek 2325 ',
          "cenab": 0,
          "cena": 1.72
        }



      ],
      kolory: kolory,
      trybkoloru: 2
    };
  },
  methods: {
    selectElementContents(el) {
      var body = document.body,
        range,
        sel;
      if (document.createRange && window.getSelection) {
        range = document.createRange();
        sel = window.getSelection();
        sel.removeAllRanges();
        try {
          range.selectNodeContents(el);
          sel.addRange(range);
        } catch (e) {
          range.selectNode(el);
          sel.addRange(range);
        }
      } else if (body.createTextRange) {
        range = body.createTextRange();
        range.moveToElementText(el);
        range.select();
      }
    },
    generate() {
        let koloryfilter = []
        if(this.trybkoloru == 2){
         koloryfilter = this.kolory.filter((el)=>el.descriptio.includes('OBUSTRONNY'))
        }

        if(this.trybkoloru == 1){
         koloryfilter = this.kolory.filter((el)=>el.descriptio.includes('ZEWNĘTRZNY'))
        }
        console.log(koloryfilter);


      for (let profil of this.profile) {
        for (let kolor of koloryfilter) {
            let opiskoloru  = this.trybkoloru == 1 ? kolor.descriptio.replace('ZEWNĘTRZNY - ','') : kolor.descriptio.replace('OBUSTRONNY - ','') 
       

          let kod = "GE_" + profil.kod + kolor.CODE;
          let opis = profil.opis + " " + opiskoloru;
          let cena = kolor.kod == "10" ? profil.cenab : profil.cena;
          
        cena = Math.round(parseFloat(cena) * (1 + parseFloat(kolor.grupa) / 100) * 100) / 100  
        this.output.push({ kod: kod, opis: opis,cena:cena });
        }
      }
    },
    copy() {
      this.selectElementContents(document.getElementById("tabela"));
    }
  },
  mounted() {
    this.generate();
  }
};
</script>


<style scoped>
p {
  margin: 0px;
  padding: 0px;
  font-weight: bold;
}

td {
  border: 1px black solid;
}
</style>
