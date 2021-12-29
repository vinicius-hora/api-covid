<template>
    <v-app>
    <v-app-bar app></v-app-bar>

    <v-main>
        <v-container>
            <v-btn @click="totalCasos">Mostrar dados</v-btn>
            <br>
            <br>
            <v-row align="center">
                <v-col cols="6">
                    <p>Total de casos: {{casos}}</p>
                    <p>Total de mortes: {{mortes}}</p>
                </v-col>
            </v-row>
            <v-btn @click="totalCasos">Mostrar dados por estado</v-btn>
        </v-container>
    </v-main>
    </v-app>
    
</template>

<script>


export default ({
     data:() => ({
       casos: 0,
       mortes: 0,
    }),
    methods: {
        
        async totalCasos(){
            const response = await fetch('https://covid19-brazil-api.vercel.app/api/report/v1/brazil');
            const data = await response.json();
            this.casos = data.data.confirmed;
            this.mortes = data.data.deaths;
            console.log(data);
            console.log(this.casos);
            
        },
        exibirCasos(){
            console.log(this.data.data);
        }
    },
        
    
})
</script>

<style scoped>

</style>
