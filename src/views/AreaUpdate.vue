<template>
    <div class="container">
        <h5>Editar Area</h5>
    
        <div class="card">
            <div class="card-content">
                <form @submit.prevent="update()">
                    <p>Nombre Area: <input type="text" v-model="payload.nombre" required/></p>
                    <p>Encargado: <input type="text" v-model="payload.encargado" required/></p>
                    <p>Nro Funcionarios: <input type="text" v-model="payload.nroFuncionarios" required/></p>
                    <button type="submit" class="waves-effect waves-light btn-small">Editar</button>
                </form>
            </div>
        </div>
    
    </div>
</template>

<script>
export default {
    name: 'AreaView',
    data() {
        const api = process.env.VUE_APP_API;
        return {
            api,
            items: [],
            payload: {
                nombre: null,
                encargado: null,
                nroFuncionarios:1,
            }
        }
    },
    methods: {
        getOne() {
            this.axios({
                method: 'get',
                url: this.api + '/areas/' + this.$route.params.id
            }).
            then((response) => {
                this.payload = response.data;
            }).
            catch((error) => {
                console.log(error);
            });
        },
        update() {
            if (confirm("Esta seguro de editar?.")) {
                this.axios({
                    method: 'patch',
                    url: this.api + '/areas/' + this.$route.params.id,
                    data: this.payload
                }).
                then((response) => {
                    console.log(response);
                }).
                catch((error) => {
                    console.log(error);
                });
            }
        }
    },
    components: {

    },
    mounted() {
        this.getOne();
    }
}
</script>
