<template>
<main class="main">
    <!-- Breadcrumb -->
    <ol class="breadcrumb">
        <li class="breadcrumb-item"><a href="/">Escritorio</a></li>
    </ol>
    <div class="alert alert-success mx-4 alert-dismissable">
    <button type="button" class="close" data-dismiss="alert">&times;</button>
    <strong>!Hola¡</strong> Bienvenido al Sistema</div>
    

</main>
</template>
<script>
    export default {
        data (){
            return {
                varIngreso:null,
                charIngreso:null,
                ingresos:[],
                varTotalIngreso:[],
                varMesIngreso:[], 
                
                varVenta:null,
                charVenta:null,
                ventas:[],
                varTotalVenta:[],
                varMesVenta:[],
            }
        },
        methods : {
            getIngresos(){
                let me=this;
                var url= '/dashboard';
                axios.get(url).then(function (response) {
                    var respuesta= response.data;
                    me.ingresos = respuesta.ingresos;
                    //cargamos los datos del chart
                    me.loadIngresos();
                })
                .catch(function (error) {
                    console.log(error);
                });
            },
            getVentas(){
                let me=this;
                var url= '/dashboard';
                axios.get(url).then(function (response) {
                    var respuesta= response.data;
                    me.ventas = respuesta.ventas;
                    //cargamos los datos del chart
                    me.loadVentas();
                })
                .catch(function (error) {
                    console.log(error);
                });
            },
            loadIngresos(){
                let me=this;
                me.ingresos.map(function(x){
                    me.varMesIngreso.push(x.mes);
                    me.varTotalIngreso.push(x.total);
                });
                me.varIngreso=document.getElementById('ingresos').getContext('2d');

                me.charIngreso = new Chart(me.varIngreso, {
                    type: 'bar',
                    data: {
                        labels: me.varMesIngreso,
                        datasets: [{
                            label: 'Ingresos',
                            data: me.varTotalIngreso,
                            backgroundColor: '#ffc107',
                            borderColor: 'rgba(255, 99, 132, 0.2)',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        scales: {
                            yAxes: [{
                                ticks: {
                                    beginAtZero:true
                                }
                            }]
                        }
                    }
                });
            },
            loadVentas(){
                let me=this;
                me.ventas.map(function(x){
                    me.varMesVenta.push(x.mes);
                    me.varTotalVenta.push(x.total);
                });
                me.varVenta=document.getElementById('ventas').getContext('2d');

                me.charVenta = new Chart(me.varVenta, {
                    type: 'bar',
                    data: {
                        labels: me.varMesVenta,
                        datasets: [{
                            label: 'Ventas',
                            data: me.varTotalVenta,
                            backgroundColor: '#007bff',
                            borderColor: 'rgba(54, 162, 235, 0.2)',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        scales: {
                            yAxes: [{
                                ticks: {
                                    beginAtZero:true
                                }
                            }]
                        }
                    }
                });
            }
        },
        mounted() {
            this.getIngresos();
            this.getVentas();
        }
    }
</script>
