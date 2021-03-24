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
          kod: 8001,
          opis: "8001",
          // "cenab": 999,
          cena1k: 2.7,
          cena2k: 3.55,
        },
        {
          kod: 8092,
          opis: "8092",
          // "cenab": 999,
          cena1k: 2.77,
          cena2k: 3.62,
        },
        {
          kod: 8038,
          opis: "8038",
          // "cenab": 999,
          cena1k: 3.95,
          cena2k: 5.1,
        },
        {
          kod: 8040,
          opis: "8040",
          // "cenab": 999,
          cena1k: 4.8,
          cena2k: 6.1,
        },
        {
          kod: 7134,
          opis: "7134 ",
          // "cenab": 1.03,
          cena1k: 99999,
          cena2k: 1.3,
        },
        {
          kod: 7248,
          opis: "7248 ",
          // "cenab": 1.03,
          cena1k: 99999,
          cena2k: 1.21,

        },
        {
          kod: 7142,
          opis: "7142 ",
          // "cenab": 1.03,
          cena1k: 99999,
          cena2k: 1.66,
        },
        {
          kod: 7116,
          opis: "7116 ",
          // "cenab": 1.03,
          cena1k: 9999,
          cena2k: 1.65,

        },
        {
          kod: 7180,
          opis: "7180 ",
          // "cenab": 1.03,
          cena1k: 99999,
          cena2k: 1.64,

        },
        {
          kod: 7162,
          opis: "7162 ",
          // "cenab": 1.03,
          cena1k: 99999,
          cena2k: 1.39,

        },
        {
          kod: 7146,
          opis: "7146 ",
          // "cenab": 1.03,
          cena1k: 99999,
          cena2k: 1.7,
        },
        {
          kod: 8039,
          opis: "8039 ",
          // "cenab": 1.03,
          cena1k: 3.95,
          cena2k: 5.1,
        },
        {
          kod: 8040,
          opis: "8039 ",
          // "cenab": 1.03,
          cena1k: 4.8,
          cena2k: 6.1,
        },
        {
          kod: 8304,
          opis: "8039 ",
          // "cenab": 1.03,
          cena1k: 4.1,
          cena2k: 99999,
        },
        {
          kod: 8080,
          opis: "8039 ",
          // "cenab": 1.03,
          cena1k: 99999,
          cena2k: 5.2,
        },
      ],
      kolory: kolory,
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

          this.output.push({
            kod: kod,
            opis: opis,
            cena: cena,
            referencja: referencja,
          });
        }
      }
    },
    copy() {
      this.selectElementContents(document.getElementById("tabela"));
    },
  },
  mounted() {
    this.generate();
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
