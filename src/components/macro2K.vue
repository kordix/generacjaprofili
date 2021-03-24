<template>
<div>
       <p><b>Artykuły na makra BIAŁE-ZEWNĄTRZ-OBUSTRONNIE</b></p>
    <button @click="copy()" style="left:0">zaznacz tabelę</button>
    <table id="tabela">
        <thead>
            <tr>
                <td>Kod</td>
                <td>Opis</td>
                <td>Cena</td>
                <!-- <td>Cena</td> -->
            </tr>
        </thead>
        <tbody>
            <tr v-for="elem in output" :key="elem.kod">
                <td>
                    {{elem.kod}}

                </td>
                <td>{{elem.opis}}</td>
                <td>{{elem.cena}}</td>

                <!-- <td>{{elem.cena}}</td> -->
            </tr>
        </tbody>
    </table>

</div>
</template>

<script>
import profile from '../profilesmoovio.js';

export default {
    data() {

        return {
            output: [],
            profile: profile,
            strony:[
                {kod:'Z',opis:'zewnatrz'},
                // {kod:'W',opis:'wewnatrz'},
                {kod:'O',opis:'obustronnie'}
            ],
            uszczelki:[
                {kod:'S',opis:'usz.szara'},
                {kod:'C',opis:'usz.czarna'}
            ],
            grupy:[
                '0','6','10','20'
            ],
    
        }
    },
    methods:{
    selectElementContents(el) {
        var body = document.body,
            range, sel;
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
    generate(){
    for(let profil of this.profile){
           for(let strona of this.strony){
                    for(let grupa of this.grupy){
                    let kod = `GE_${profil.kod}_${strona.kod}_${grupa}`
                    let opis = `${profil.opis} ${strona.opis} ${grupa}%`
                    let cena = 0;
                    let cenabase = 0;
                    if(strona.kod == 'Z' || strona.kod == 'W'){
                        cenabase = profil.cena1k
                    }

                    if(strona.kod == 'O'){
                        cenabase = profil.cena2k
                    }
                    cena = Math.round(parseFloat(cenabase) * (1 + parseFloat(grupa) / 100) * 100) / 100

                    this.output.push({kod:kod,opis:opis,cena:cena})
                    }
           }
       }
    },
    copy() {
        this.selectElementContents(document.getElementById('tabela'));
    }

},
mounted() {
    let self = this;
    this.generate();


}

}
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
