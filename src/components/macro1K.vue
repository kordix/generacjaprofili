<template>
<div class="container">
       <p><b>Artykuły na makra BIAŁE-DEKOR</b></p>
    <button @click="copy()">zaznacz tabelę</button>
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
            profile:[
                // {kod:'7116',opis:'profil',cena:{bialeszara:'0,66',bialeczarna:'0,66',kolor:'1,65'}},
                {kod:'6039',opis:'Skrzydło',cena:{bialeszara:'0',bialeczarna:'0',kolor:'8,65'}},                
                ]
            ,
            strony:[
                {kod:'Z',opis:'zewnatrz'},
                {kod:'W',opis:'wewnatrz'},
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
        let kolory = [
            // {kod:'B',opis:'biale'},
            {kod:'D',opis:'dekor'}
        ]

        for(let profil of this.profile){
            for(let kolor of kolory){
                for(let grupa of this.grupy){
                let kod = `GE_${profil.kod}_${kolor.kod}_${grupa}`
                let opis = `profil ${profil.kod} ${kolor.opis} ${grupa} %`
                let cenabase =  profil.cena.kolor;

                let cena = Math.round(parseFloat(cenabase.replace(',', '.')) * (1 + parseFloat(grupa) / 100) * 100) / 100;

                this.output.push({kod:kod,opis:opis,cena:cena})

                }
            }
        }

    },
    generateBiale(){
         let kolory = [
            {kod:'B',opis:'biale'},
        ]

        let uszczelki = [
            {kod:'S',opis:'usz.szara'},
            // {kod:'C',opis:'usz.czarna'},

        ]
        for(let profil of this.profile){
            for(let kolor of kolory){
                for(let uszczelka of uszczelki){
                let kod = `GE_${profil.kod}_${kolor.kod}`
                let opis = `profil ${profil.kod} ${kolor.opis}`
                let cena = 0;

                if(uszczelka.kod == 'S'){
                    cena = profil.cena.bialeszara
                }

                if(uszczelka.kod == 'C'){
                    cena = profil.cena.bialeczarna
                }

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
    // this.generateBiale()

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
