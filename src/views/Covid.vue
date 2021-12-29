
<template>

    <v-app>
    <v-app-bar app></v-app-bar>

    <v-main>
        <v-container >
            <div class="dark" id="div-total">
                <v-row >
                    <v-col cols="6">
                          <v-btn @click="totalCasos">Mostrar total Brasil</v-btn>
                            <br>
                            <br>
                            <v-row >
                                <v-col cols="6">
                                    <p >Total de casos: {{casos}}</p>
                                    <p >Total de mortes: {{mortes}}</p>
                                 </v-col>
                            </v-row>    
                    </v-col>
                    <v-col cols="6">
                        <v-btn @click="totalPais">Pesquisar pais</v-btn>
                         <v-text-field label="Pais" v-model="pais" >{{pais}}</v-text-field>
                         <p >Total de casos: {{casosPais}}</p>
                         <p >Total de mortes: {{mortesPais}}</p>
                    </v-col>
                </v-row>
              
                
                <br>
                <v-spacer></v-spacer>
            </div>
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
            <!-- funcionando com div -->
            <!-- <div v-for="(dado, id) in dados" :key="id">
                <p>Estado: {{dado.state}}</p>
                <p>Casos: {{dado.cases}}</p>
                <p>mortes: {{dado.deaths}}</p>
                <v-spacer></v-spacer>
            </div> -->
            <div id="div-estado">
                <v-btn @click="casosPorEstado">Mostrar dados por estado</v-btn>
                <v-simple-table dark>
                    <template v-slot:default>
                    <thead>
                        <tr>
                        <th class="text-center">
                            <h1>Estado</h1>
                        </th>
                        <th class="text-center">
                            <h1>Casos</h1>
                        </th>
                        <th class="text-center">
                            <h1>Mortes</h1>
                        </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(dado, id) in dados" :key="id">
                            <td>{{ dado.state}}</td>
                            <td>{{ dado.cases.toLocaleString('pt-BR') }}</td>
                            <td>{{ dado.deaths.toLocaleString('pt-BR') }}</td>
                        </tr>
                    </tbody>
                    </template>
                </v-simple-table>
            </div>
        </v-container>
    </v-main>
    </v-app>
    
</template>

<script>


export default ({
     data:() => ({
         //   variaves da api total
       casos: null,
       mortes: null,
       dados:[
           
       ],
    //   variaves da api por estado
       cases:[],
       states:[],
       deaths:[],

       teste:0,

    //variaveis de pais
    pais: '',
    dadosPais:[],
    casosPais: null,
    mortesPais: null,
       
    }),
    methods: {
        // consumo da api total de tados
        async totalCasos(){
            const response = await fetch('https://covid19-brazil-api.vercel.app/api/report/v1/brazil');
            const data = await response.json();
            this.casos = data.data.confirmed.toLocaleString('pt-BR');
            this.mortes = data.data.deaths.toLocaleString('pt-BR');
            
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
            
            
        },
        //total por pais
        async totalPais(){
            const pais = this.pais.trim();
            const response = await fetch("https://covid19-brazil-api.vercel.app/api/report/v1/"+pais+"");
            const dataPais = await response.json();
            this.casosPais = dataPais.data.confirmed.toLocaleString('pt-BR') 
            this.mortesPais = dataPais.data.deaths.toLocaleString('pt-BR') 
            console.log("dados pais", dataPais);
           
            
        },
    },
        
    
})
</script>

<style scoped>
    .dark{
        background-color: #1e1e1e;
        color: white;
    }
    #div-total{
        margin: 10px;
        padding: 10px;
        margin-top: 20px;
    }

</style>
