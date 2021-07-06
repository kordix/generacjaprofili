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

  
   <p><b>Article</b></p>  
    <textarea name="" id="" cols="30" rows="10" v-model="article">
    </textarea>

    <p></p>

<p><b>Artfou</b></p>
  <label for="">Artfou</label>
      <textarea name="" id="" cols="30" rows="10" v-model="artfou">
    </textarea>

  </div>
</template>

<script>
import kolory from '../kolory.js';


export default {
  data() {
    return {
      output: [],
      profile: [
        // {
        //   "kod": 7134,
        //   opis:'7134 ',
        //   // "cenab": 1.03,
        //   "cena": 1.3
        // },
        // {
        //   "kod": 7248,
        //   opis:'7248 ',
        //   // "cenab": 1.03,
        //   "cena": 1.21
        // },
        // {
        //   "kod": 7142,
        //   opis:'7142 ',
        //   // "cenab": 1.03,
        //   "cena": 1.66
        // },
        // {
        //   "kod": 7116,
        //   opis:'7116 ',
        //   // "cenab": 1.03,
        //   "cena": 1.65
        // },
        // {
        //   "kod": 7180,
        //   opis:'7180 ',
        //   // "cenab": 1.03,
        //   "cena": 1.64
        // },
        // {
        //   "kod": 7162,
        //   opis:'7162 ',
        //   // "cenab": 1.03,
        //   "cena": 1.39
        // },
        // {
        //   "kod": 7146,
        //   opis:'7146 ',
        //   // "cenab": 1.03,
        //   "cena": 1.7
        // },
        // {
        //   "kod": 7248,
        //   opis:'7248 ',
        //   // "cenab": 1.03,
        //   "cena": 1.79
        // },
        {
          "kod": 3325,
          opis:'Okapnik 3325 ',
          // "cenab": 1.03,
          "cena": 1.79
        },
        {
          "kod": 3326,
          opis:'Okapnik 3326 ',
          // "cenab": 1.03,
          "cena": 2.27
        },

      ],
      kolory: kolory,
      article:'',
      artfou:'',
      trybkoloru: 2,
      inserty:''
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

      this.generujarticle();
      this.generujartfou();

    },
    generujarticle(){

      for(let element of this.output){
        this.article += 'INSERT INTO article (code, ftimestamp, descriptio, categorie, unite, prtarif, prcourant, ne, valorise, marge, pv1, pv2,';
        this.article += 'pv3, qte2, qte3, forfait1,delaicli, compose, artnegoce, inclcompos, dxf, dxfdoc, dxffou, typeprix, largeurs, hauteurs, lmin, hmin, profcomp, profint1, ';
        this.article += 'profint2, profext1, profext2, barrette1, barrette2, barrette3, barrette4, nbarrette1, nbarrette2, nbarrette3, ';
        this.article += 'nbarrette4, alaquer, laque2, qlaq2, laquer1, laquer2, laquer3, societe, dxfplan, sansclr, perte, poids, prixkg, desactive,';
        this.article += 'datedesac, comptea, comptes, compte55, compte196, navision, codenav, dxfpicture, systeme, formulef, codefou, artfou, datecreate, ';
        this.article += 'dimension, prixnet, pv1cover, forf1cover, necover, valcover, libre1, libre2, fichierbmp, surface, champs, cptlarg, cpthaut, ';
        this.article += 'labour, locked, translate, stkmin01, stkmin02, stkmin03, stkmin04, stkmin05, stkmin06, stkmin07, stkmin08, stkmin09, ';
        this.article += 'stkmin10, stkmin11, stkmin12, stkmax01, stkmax02, stkmax03, stkmax04, stkmax05, stkmax06, stkmax07, stkmax08, stkmax09,';
        this.article += 'stkmax10, stkmax11, stkmax12, periode, plan, colisage, controle, prepa, horsstock, modifie, composplan, composcfou, devis, ';
        this.article += 'propcde, coeffres, stockoptim, netiq, imput, invisdevis, surmesure, formule, qlaq1, transfert, stkpos, stknumcas, stkqtecas, ';
        this.article += 'colis, colis2, stkdateout, verifier, promo, prccolis, prccolis2, coeffcas, catcas, gencode, cletri, xml, poste, artbos, ';
        this.article += 'lastupdate, avantprod, avantlivr, web, nostockdec, withfail, noweb, level, info, recno, categorie2, categorie3, blade, bl_wearlen)';
        this.article += "VALUES (\'";
        this.article += element.kod;
        this.article += "\', 0,\'";
        this.article += element.opis;
        this.article += "\', NULL, \'M\', 24.07806, 24.07806, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, NULL, 0, 0, 0, NULL, NULL, 0, 0, 0,";
        this.article += "NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, 0, 0, 0, 0, 0, NULL, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, \'0000-00-00\', ";
        this.article += 'NULL, NULL, NULL, NULL, 38, NULL, 29, NULL, NULL, NULL, NULL, \'0000-00-00\', 0, 0, 0, 0, 0, 0, 0, NULL, NULL, 0, 0, 0, 0, 0, 0, NULL, ';
        this.article += '0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, NULL, 28, NULL, 0, 0, NULL, 0, 0, NULL, NULL, 38, NULL, NULL, ';
        this.article += "\'0000-00-00\', 0, 0, 0, 0, 0, 0, NULL, NULL, 38, NULL, 38, \'0000-00-00\', 0, 0, 0, 0, 0, 0, 0, NULL, default, NULL, NULL, 0, 0);";
        

    this.article += "\r\n";

      }



    },
    generujartfou(){


            for(let element of this.output){

       this.artfou += 'INSERT INTO artfou (article, codefou, numero, artfou, livraison, delai, cdemin, q1, q2, q3, q4, padev1, padev2, padev3, padev4, rem1, rem2,';
      this.artfou += 'rem3, rem4, devise, pafb, frais, pr, idfourn, idfourn2, idfourn3, idfourn4, prixkg, comment, codeedi, couleur, libre1, libre2, livcli,'; 
      this.artfou += 'delaicli, recno) VALUES';
      this.artfou += "(\'";
      this.artfou += element.kod;
      this.artfou += "\', \'GEALAN\', 0, \'";
      this.artfou += element.kod;
      this.artfou += "\', 0, 0, 0, 1, 0, 0, 0, ";
      this.artfou += element.cena;
      
      this.artfou += ", 0, 0, 0, 0, 0, 0, 0, \'EUR\', 0, 1.08, 0, \'\', \'\', \'\', \'\', 0, \'\', \'\', \'\'," ;
      this.artfou += "\'\', \'\', 0, 0, default);";
    this.artfou += "\r\n";


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
