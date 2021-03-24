<template>
  <div class="container">
    <p><b>Artykuły do profili z kolorami BIAŁE-ZEWNĄTRZ-OBUSTRONNIE</b></p>
    <button @click="copy()">Zaznacz tabele</button>
    <table id="tabela">
      <thead>
        <td>code</td>
        <td>descriptio</td>
        <td>padev1</td>
        <td>artfou</td>
      </thead>
      <tbody>
        <tr v-for="elem of this.output" :key="elem.id">
          <td>{{ elem.kod }}</td>
          <td>{{ elem.opis }}</td>
          <td>{{ elem.cena }}</td>
          <td>{{ elem.referencja }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import kolory from "../kolory.js";
import profile from "../profile9000.js";

export default {
  data() {
    return {
      output: [],
      profile: [
        {
          kod: 1111,
          opis: "Oscieznica",
          makro: "tak",
          typ: "2K",
          cenab: 2.2,
          cena1k: 2.7,
          cena2k: 3.55,
        },
        {
          kod: 2222,
          opis: "Skrzydlo ",
          makro: "tak",
          typ: "2K",
          cenab: 2.3,
          cena1k: 2.77,
          cena2k: 3.62,
        },
        {
          kod: 3333,
          opis: "Poszerzenie ",
          makro: "nie",
          typ: "2K",
          cenab: 999,
          cena1k: 3.95,
          cena2k: 5.1,
        },
        {
          kod: 4444,
          opis: "Listwa przyszybowa",
          makro: "tak",
          typ: "1K2",
          cenab: 999,
          // cena1k: 99999,
          cena2k: 6.1,
        },
        {
          kod: 5555,
          opis: "Artykuł z całym kolorem, 1 w kodzie koloru",
          makro: "tak",
          typ: "1K1",
          cenab: 999,
          cena1k: 5,
          // cena2k: 6.1,
        },
      ],
      kolory: kolory,
      strony: [
        { kod: "Z", opis: "zewnatrz" },
        // {kod:'W',opis:'wewnatrz'},
        { kod: "O", opis: "obustronnie" },
      ],
      grupy: ["0", "6", "10", "20"],
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
      for (let profil of this.profile) {
        for (let kolor of this.kolory) {
          let kod = "GE_" + profil.kod + "" + kolor.CODE;
          let referencja = profil.kod + "" + kolor.referencja;
          let opis = profil.opis + " " + " " + kolor.descriptio;
          let cena = 0;
          if (kolor.strona == "B") {
            cena = profil.cenab;
          } else if (kolor.strona == "1K") {
            cena = profil.cena1k;
          } else {
            cena = profil.cena2k;
          }

          cena =
            Math.round(
              parseFloat(cena) * (1 + parseFloat(kolor.grupa) / 100) * 100
            ) / 100;

          let generuj = true;

          if (profil.makro === "tak" && kolor.artykul != "tak") {
            generuj = false;
          }

          if (profil.typ === "1K2" && kolor.strona === "1K") {
            generuj = false;
          }

          if (profil.typ === "1K1" && kolor.strona === "2K") {
            generuj = false;
          }

          if (generuj == true) {
            if (kod.length > 16) {
              kod = kod.replace("GE_", "GE");
              console.log(kod.length);
            }

            this.output.push({
              kod: kod,
              opis: opis,
              cena: cena,
              referencja: referencja,
            });
          }
        }
      }
    },
    generateMacro2K() {
      for (let profil of this.profile) {
        for (let strona of this.strony) {
          for (let grupa of this.grupy) {
            let kod = `GE_${profil.kod}_${strona.kod}_${grupa}`;
            let opis = `${profil.opis} ${strona.opis} ${grupa}%`;
            let cena = 0;
            let cenabase = 0;
            if (strona.kod == "Z" || strona.kod == "W") {
              cenabase = profil.cena1k;
            }

            if (strona.kod == "O") {
              cenabase = profil.cena2k;
            }
            cena =
              Math.round(
                parseFloat(cenabase) * (1 + parseFloat(grupa) / 100) * 100
              ) / 100;

            if (profil.makro === "tak" && profil.typ === "2K") {
              this.output.push({ kod: kod, opis: opis, cena: cena });
            }
          }
        }
      }
    },
    generateMacro1K() {
      let kolory = [
        // {kod:'B',opis:'biale'},
        { kod: "D", opis: "dekor" },
      ];

      for (let profil of this.profile) {
        for (let kolor of kolory) {
          for (let grupa of this.grupy) {
            let kod = `GE_${profil.kod}_${kolor.kod}_${grupa}`;
            let opis = `profil ${profil.kod} ${kolor.opis} ${grupa} %`;
            let cenabase = 0;

            if(profil.typ == '1K1'){
              cenabase = profil.cena1k
            }

            if(profil.typ == '1K2'){
              cenabase = profil.cena2k
            }

            let cena =
              Math.round(
                parseFloat(cenabase) * (1 + parseFloat(grupa) / 100) * 100
              ) / 100;

            if ((profil.makro === "tak" && profil.typ != "2K")) {
              this.output.push({ kod: kod, opis: opis, cena: cena });
            }
          }
        }
      }
    },
    generateBiale(){
      for(let profil of this.profile){
            let kod = `GE_${profil.kod}00`;
            let opis = `profil ${profil.kod} biale`;
            let referencja = `${profil.kod}00G`;
              this.output.push({ kod: kod, opis: opis, cena: profil.cenab, referencja:referencja });
        
      }
    },


    copy() {
      this.selectElementContents(document.getElementById("tabela"));
    },
  },
  mounted() {
    this.generate();
    this.generateMacro2K();
    this.generateMacro1K();
    this.generateBiale();
  },
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
