<template>
  <div class="container">
    <!-- <p><b>Artykuły do profili z kolorami BIAŁE-ZEWNĄTRZ-OBUSTRONNIE</b></p> -->
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
    <button
      @click="
        generujarticle();
        generujartfou();
      "
    >
      Generuj insterty
    </button>
    <p><b>Article</b></p>
    <textarea
      name=""
      id=""
      cols="30"
      rows="10"
      style="width: 800px"
      v-model="article"
    >
    </textarea>

    <p></p>

    <p><b>Artfou</b></p>
    <textarea
      name=""
      id=""
      cols="30"
      rows="10"
      style="width: 800px"
      v-model="artfou"
    >
    </textarea>
  </div>
</template>

<script>
import kolory from "../kolorydecco.js";
import profile from "../profileall.js";

//6051-0-2-1K-1306

export default {
  data() {
    return {
      output: [],
      article: "",
      artfou: "",
      profile: [
        {
          kod: 8210,
          opis: "Ościeżnica 8210",
          makro: "nie",
          typ: "2K",
          cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        },
        {
          kod: 8224,
          opis: "Skrzydło 8224",
          makro: "nie",
          typ: "2K",
       cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        },
        {
          kod: 8225,
          opis: "Skrzydło 8225",
          makro: "nie",
          typ: "2K",
          cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        },
        {
          kod: 8226,
          opis: "Skrzydło 8226",
          makro: "nie",
          typ: "2K",
          cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        },
        {
          kod: 8227,
          opis: "Skrzydło 8227",
          makro: "nie",
          typ: "2K",
          cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        },
        {
          kod: 8230,
          opis: "Skrzydło 8230",
          makro: "nie",
          typ: "2K",
          cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        },
        {
          kod: 8236,
          opis: "Skrzydło 8236",
          makro: "nie",
          typ: "2K",
          cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        },
        {
          kod: 8232,
          opis: "Słupek ruchomy 8232",
          makro: "nie",
          typ: "2K",
          cenab: 3,
          cena1k: 4,
          cena2k: 5,
          uszczelkabiale: "00",
          uszczelka: " ",
        }
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
//6051-0-2-1K-1306

console.log(this.profile);
console.log(this.kolory);
      for (let profil of this.profile) {
        for (let kolor of this.kolory) {
          let kod = profil.kod + "-" + kolor.CODE;
          let referencja = profil.kod + "-" + kolor.referencja;
          let opis = profil.opis + " " + " " + kolor.opis;

          referencja = referencja.replace(" ", profil.uszczelka);

          if (referencja.length < 10 && profil.uszczelka != " ") {
            referencja += profil.uszczelka;
          }

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

          if (profil.makro === "nie") {
            // generuj = false;
          }

          if (kolor.artykul === "tak") {
            //generuj = false;
          }

          if (profil.typ === "1K2" && kolor.strona === "1K") {
            generuj = false;
          }

          if (profil.typ === "1K1" && kolor.strona === "2K") {
            generuj = false;
          }

          if (profil.typ === "1K1") {
            opis = opis.replace("ZEWNĘTRZNY -", "");
          }

          if (profil.typ === "1K2") {
            opis = opis.replace("OBUSTRONNY -", "");
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
    generateBiale() {
      for (let profil of this.profile) {
        let kod = `GE_${profil.kod}00`;
        let opis = `${profil.opis} ${profil.kod} biale`;
        let referencja = `${profil.kod}${profil.uszczelkabiale}`;
        this.output.push({
          kod: kod,
          opis: opis,
          cena: profil.cenab,
          referencja: referencja,
        });
      }
    },
    generujarticle() {
      for (let element of this.output) {
        this.article +=
          "INSERT INTO article (code, ftimestamp, descriptio, categorie, unite, prtarif, prcourant, ne, valorise, marge, pv1, pv2,";
        this.article +=
          "pv3, qte2, qte3, forfait1,delaicli, compose, artnegoce, inclcompos, dxf, dxfdoc, dxffou, typeprix, largeurs, hauteurs, lmin, hmin, profcomp, profint1, ";
        this.article +=
          "profint2, profext1, profext2, barrette1, barrette2, barrette3, barrette4, nbarrette1, nbarrette2, nbarrette3, ";
        this.article +=
          "nbarrette4, alaquer, laque2, qlaq2, laquer1, laquer2, laquer3, societe, dxfplan, sansclr, perte, poids, prixkg, desactive,";
        this.article +=
          "datedesac, comptea, comptes, compte55, compte196, navision, codenav, dxfpicture, systeme, formulef, codefou, artfou, datecreate, ";
        this.article +=
          "dimension, prixnet, pv1cover, forf1cover, necover, valcover, libre1, libre2, fichierbmp, surface, champs, cptlarg, cpthaut, ";
        this.article +=
          "labour, locked, translate, stkmin01, stkmin02, stkmin03, stkmin04, stkmin05, stkmin06, stkmin07, stkmin08, stkmin09, ";
        this.article +=
          "stkmin10, stkmin11, stkmin12, stkmax01, stkmax02, stkmax03, stkmax04, stkmax05, stkmax06, stkmax07, stkmax08, stkmax09,";
        this.article +=
          "stkmax10, stkmax11, stkmax12, periode, plan, colisage, controle, prepa, horsstock, modifie, composplan, composcfou, devis, ";
        this.article +=
          "propcde, coeffres, stockoptim, netiq, imput, invisdevis, surmesure, formule, qlaq1, transfert, stkpos, stknumcas, stkqtecas, ";
        this.article +=
          "colis, colis2, stkdateout, verifier, promo, prccolis, prccolis2, coeffcas, catcas, gencode, cletri, xml, poste, artbos, ";
        this.article +=
          "lastupdate, avantprod, avantlivr, web, nostockdec, withfail, noweb, level, info, recno, categorie2, categorie3, blade, bl_wearlen)";
        this.article += "VALUES ('";
        this.article += element.kod;
        this.article += "', 0,'";
        this.article += element.opis;
        this.article +=
          "', NULL, 'M', 24.07806, 24.07806, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, NULL, 0, 0, 0, NULL, NULL, 0, 0, 0,";
        this.article +=
          "NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, 0, 0, 0, 0, 0, NULL, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, '0000-00-00', ";
        this.article +=
          "NULL, NULL, NULL, NULL, 38, NULL, 29, NULL, NULL, NULL, NULL, '0000-00-00', 0, 0, 0, 0, 0, 0, 0, NULL, NULL, 0, 0, 0, 0, 0, 0, NULL, ";
        this.article +=
          "0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, NULL, 28, NULL, 0, 0, NULL, 0, 0, NULL, NULL, 38, NULL, NULL, ";
        this.article +=
          "'0000-00-00', 0, 0, 0, 0, 0, 0, NULL, NULL, 38, NULL, 38, '0000-00-00', 0, 0, 0, 0, 0, 0, 0, NULL, default, NULL, NULL, 0, 0);";

        this.article += "\r\n";
      }
    },
    generujartfou() {
      for (let element of this.output) {
        this.artfou +=
          "INSERT INTO artfou (article, codefou, numero, artfou, livraison, delai, cdemin, q1, q2, q3, q4, padev1, padev2, padev3, padev4, rem1, rem2,";
        this.artfou +=
          "rem3, rem4, devise, pafb, frais, pr, idfourn, idfourn2, idfourn3, idfourn4, prixkg, comment, codeedi, couleur, libre1, libre2, livcli,";
        this.artfou += "delaicli, recno) VALUES";
        this.artfou += "('";
        this.artfou += element.kod;
        this.artfou += "', 'GEALAN', 0, '";
        this.artfou += element.referencja;
        this.artfou += "', 0, 0, 0, 1, 0, 0, 0, ";
        this.artfou += element.cena;

        this.artfou +=
          ", 0, 0, 0, 0, 0, 0, 0, 'EUR', 0, 1.08, 0, '', '', '', '', 0, '', '', '',";
        this.artfou += "'', '', 0, 0, default);";
        this.artfou += "\r\n";
      }
    },

    copy() {
      this.selectElementContents(document.getElementById("tabela"));
    },
  },
  mounted() {
    this.generate();
    // this.generateMacro2K();
    // this.generateMacro1K();
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
