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
export default {
    data() {

        return {
            output: [],
            profile: [
                // {
                //     "kod": 6045,
                //     opis:'Ościeżnica 6045',
                //     "cenab": 7.31,
                //     "cena1k": 7.68,
                //     "cena2k": 9.58
                // },
                {
                    "kod": 6012,
                    opis:'Słupek ruchomy 6012',
                    "cenab": 0,
                    "cena1k": 4.7,
                    "cena2k": 5.66
                },
                // {
                //     "kod": 6009,
                //     opis:'Ościeżnica 6009',
                //     "cenab": 4.4,
                //     "cena1k": 5.2,
                //     "cena2k": 6.1
                // },
                //   {
                //     "kod": 6004,
                //     opis:'Ościeżnica 6004',
                //     "cenab": 2.56,
                //     "cena1k": 3.64,
                //     "cena2k": 4.59
                // },
                // {
                //     "kod": 6001,
                //     opis:'Ościeżnica 6001',
                //     "cenab": 2.56,
                //     "cena1k": 3.47,
                //     "cena2k": 4.43
                // },
                // {
                //     "kod": 6015,
                //     opis:'Ościeżnica 6015',
                //     "cenab": 3.88,
                //     "cena1k": 4.27,
                //     "cena2k": 5.23
                // },
                // {
                //     "kod": 6026,
                //     opis:'Ościeżnica 6026',
                //     "cenab": 4.47,
                //     "cena1k": 5.55,
                //     "cena2k": 7.23
                // },
                //  {
                //     "kod": 6030,
                //     opis:'Ościeżnica 6030',
                //     "cenab": 4.14,
                //     "cena1k": 5.21,
                //     "cena2k": 6.89
                // },
                // {
                //     "kod": 6055,
                //     opis:'Skrzydło 6055',
                //     "cenab": 2.75,
                //     "cena1k": 3.64,
                //     "cena2k": 4.59
                // },
                // {
                //     "kod": 6023,
                //     opis:'Skrzydło 6023',
                //     "cenab": 3.15,
                //     "cena1k": 4.16,
                //     "cena2k": 5.23
                // },
                // {
                //     "kod": 6079,
                //     opis:'Skrzydło 6079',
                //     "cenab": 2.37,
                //     "cena1k": 3.35,
                //     "cena2k": 4.38
                // },
                // {
                //     "kod": 6017,
                //     opis:'Skrzydło 6017',
                //     "cenab": 4.89,
                //     "cena1k": 6.52,
                //     "cena2k": 7.48
                // },
                // {
                //     "kod": 6025,
                //     opis:'Skrzydło 6025',
                //     "cenab": 5.74,
                //     "cena1k": 6.94,
                //     "cena2k": 8.23
                // },
                // {
                //     "kod": 6033,
                //     opis:'Skrzydło 6033',
                //     "cenab": 5.29,
                //     "cena1k": 7.16,
                //     "cena2k": 8.23
                // },
                // {
                //     "kod": 6027,
                //     opis:'Skrzydło 6027',
                //     "cenab": 5.74,
                //     "cena1k": 7.16,
                //     "cena2k": 8.33
                // },
                // {
                //     "kod": 6039,
                //     opis:'Skrzydło 6039',
                //     "cenab": 6.03,
                //     "cena1k": 8.65,
                //     "cena2k": 999
                // },
                // {
                //     "kod": 6057,
                //     opis:'Słupek 6057',
                //     "cenab": 3.64,
                //     "cena1k": 4.59,
                //     "cena2k": 5.55
                // },
                // {
                //     "kod": 6058,
                //     opis:'Słupek 6058',
                //     "cenab": 7.91,
                //     "cena1k": 9.18,
                //     "cena2k": 10.53
                // },
                //   {
                //     "kod": 6053,
                //     opis:'Słupek 6053',
                //     "cenab": 5.74,
                //     "cena1k": 8.01,
                //     "cena2k": 9.1
                // },
                // {
                //     "kod": 6049,
                //     opis:'Słupek 6049',
                //     "cenab": 3.58,
                //     "cena1k": 4.38,
                //     "cena2k": 5.35
                // }
            ],
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
