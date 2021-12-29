<template>
    <v-app>
    <v-app-bar app></v-app-bar>

    <v-main>
        <v-container>
            <v-btn @click="totalCasos">Mostrar dados</v-btn>
            <br>
            <br>
            <v-row >
                <v-col cols="6">
                    <p >Total de casos: {{casos}}</p>
                    <p >Total de mortes: {{mortes}}</p>
                </v-col>
            </v-row>
            <v-btn @click="casosPorEstado">Mostrar dados por estado</v-btn>
            <br>
            <!-- funcionando com UL -->
            <!-- <ul >
               
                <li v-for="(dado, id) in dados" :key="id"> Estado: {{dado.state}}
                    <br>
                    Casos: {{dado.cases}}
                    <br>
                    mortes: {{dado.deaths}}
                    <v-spacer></v-spacer>
                    
                </li>
              
            </ul> -->
            <!-- <div v-for="(dado, id) in dados" :key="id">
                <p>Estado: {{dado.state}}</p>
                <p>Casos: {{dado.cases}}</p>
                <p>mortes: {{dado.deaths}}</p>
                <v-spacer></v-spacer>
            </div> -->
        </v-container>
    </v-main>
    </v-app>
    
</template>

<script>


export default ({
     data:() => ({
         //   variaves da api total
       casos: 0,
       mortes: 0,
       dados:[
           
       ],
    //   variaves da api por estado
       cases:[],
       states:[],
       deaths:[],

       teste:0,
       
    }),
    methods: {
        // consumo da api total de tados
        async totalCasos(){
            const response = await fetch('https://covid19-brazil-api.vercel.app/api/report/v1/brazil');
            const data = await response.json();
            this.casos = data.data.confirmed;
            this.mortes = data.data.deaths;
            console.log(data);
            console.log(this.casos);
            
        },
        //consumo da api por estado
        async casosPorEstado(){
            const response = await fetch('https://covid19-brazil-api.vercel.app/api/report/v1');
            const data = await response.json();
            console.log("dadios ", data);
            this.dados = data.data;
            console.log("array: ",this.dados);
            this.teste = this.dados[1].cases;
            for(let i = 0; i < this.dados.length; i++){
                this.cases.push(this.dados[i].cases);
            }
            for(let i = 0; i < this.dados.length; i++){
                this.states.push(this.dados[i].state.format);
            }
            for(let i = 0; i < this.dados.length; i++){
                this.deaths.push(this.dados[i].deaths);
            }
            
            console.log("casos: ", this.cases);
            console.log("mortes: ", this.deaths);
             console.log("estado: ", this.states);
            

        }
    },
        
    
})
</script>

<style scoped>

</style>
