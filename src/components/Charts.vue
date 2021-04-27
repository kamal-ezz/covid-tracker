<template>
    
  
    <h1 class="font-semibold text-2xl">Top Countries By Cases</h1>
    <div id="canvas1"></div>
    <h1 class="mt-10 font-semibold text-2xl">Top Countries By Deaths</h1>
    <div id="canvas2"></div>
    
   
</template>


<script>


export default {
    name:'Charts',
    props: ['data'],
    methods: {
        getTopCountriesByCases(){

            function filter(country){
                return {
                    country: country.Country,
                    total: parseInt(country.TotalConfirmed)
                }
            }


            const countryNames = this.data.Countries.map(filter)
                                        .sort((a,b) => a.total < b.total ? 1 : -1)
                                        .slice(0,10)
                                        .map(c => c.country)

            const countryCases = this.data.Countries.map(filter)
                                        .sort((a,b) => a.total < b.total ? 1 : -1)
                                        .slice(0,10)
                                        .map(c => c.total)
            return [countryNames, countryCases]
        },
        getTopCountriesByDeaths(){

              function filter(country){
                return {
                    country: country.Country,
                    total: parseInt(country.TotalDeaths)
                }
            }

            const countryNames = this.data.Countries.map(filter)
                                        .sort((a,b) => a.total < b.total ? 1 : -1)
                                        .slice(0,10)
                                        .map(c => c.country)

            const countryDeaths = this.data.Countries.map(filter)
                                        .sort((a,b) => a.total < b.total ? 1 : -1)
                                        .slice(0,10)
                                        .map(c => c.total)
            return [countryNames, countryDeaths]
           
        },

        buildChart(){
           let dataOfCases = [
                {
                    x: this.getTopCountriesByCases()[0],
                    y: this.getTopCountriesByCases()[1],
                     marker:{
                         color: [
                             'red',
                             'blue',
                             'green',
                             'yellow',
                             'purple',
                             'orange',
                             'pink',
                             'brown',
                             'gray',
                             'black',
                        
                        ]
                     },
                    type: 'bar'
                }
            ];

            let dataOfDeaths = [
                {
                    x: this.getTopCountriesByDeaths()[0],
                    y: this.getTopCountriesByDeaths()[1],
                     marker:{
                         color: [
                             'red',
                             'blue',
                             'green',
                             'yellow',
                             'purple',
                             'orange',
                             'pink',
                             'brown',
                             'gray',
                             'black',
                        
                        ]
                     },
                    type: 'bar'
                }
            ];

            Plotly.newPlot('canvas1', dataOfCases);
            Plotly.newPlot('canvas2', dataOfDeaths);
        }
    },  
    data(){
        return {
            
        }
    },
    mounted() {
        this.buildChart()
    }
}
</script>