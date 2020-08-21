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
                <td>{{elem.kod}}</td>
                <td>{{elem.opis}}</td>
                <td>{{elem.cena}}</td>
                <td>{{elem.referencja}}</td>
            </tr>
        </tbody>
    </table>
</div>
</template>

<script>
import kolory from '../kolory.js';
import profile from '../profile9000.js';

export default {
    data() {
        return {
            output: [],
            profile: [  
                // {
                //     "kod": 5206,
                //     opis:'Łącznik rurowy',
                //     "cenab": 6.82,
                //     "cena1k": 8.63,
                //     "cena2k": 10.94
                // },
                // {
                //     "kod": 5207,
                //     opis:'Łącznik rurowy',
                //     "cenab": 3.57,
                //     "cena1k": 4.48,
                //     "cena2k": 5.47
                // },
                 {
                    "kod": 6059,
                    opis:'Monostulp 6059',
                    "cenab": 3.52,
                    "cena1k": 4.49,
                    "cena2k": 5.55
                }
                ]
                ,
            kolory: kolory
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
                    let kod = "GE_" + profil.kod + '' + kolor.CODE;
                    let referencja = profil.kod + '' + kolor.referencja;
                    let opis = profil.opis + ' ' + " " + kolor.descriptio;
                    let cena = 0;
                    if (kolor.strona == "B") {
                        cena = profil.cenab
                    } else if (kolor.strona == '1K') {
                        cena = profil.cena1k
                    } else {
                        cena = profil.cena2k
                    }

                    this.output.push({
                        kod: kod,
                        opis: opis,
                        cena: cena,
                        referencja: referencja
                    });
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
