<template>
    <div class="container">
        <h5>Areas</h5>
    
        <div class="card">
            <div class="card-content">
                <form @submit.prevent="nuevo()">
                    <h5>Nueva Area</h5>
                    <p>Nombre Area: <input type="text" v-model="payload.nombre" required/></p>
                    <p>Ecargado: <input type="text" v-model="payload.encargado" required/></p>
                    <p>Nro Funcionarios: <input type="number" v-model="payload.nroFuncionarios" required/></p>
                    <button type="submit" class="waves-effect waves-light btn-small">Agregar</button>
                </form>
            </div>
        </div>
    
        <div class="card">
            <div class="card-content">
                <form @submit.prevent="getList()">
                    <h5>Buscar categoria</h5>
                    <p>Nombre categoria: <input type="search" v-model="search" @search="getList()" /></p>
                    <button type="submit" class="waves-effect waves-light btn-small">buscar</button>
                </form>
            </div>
        </div>
    
        
        <div class="card">
            <div class="card-content">
                <table>
                    <thead>
                        <tr>
                            <th>Id</th>
                            <th>Area</th>
                            <th>Encargado</th>
                            <th>Nro de Empleados</th>
                            <th>Operaciones</th>
                        </tr>
                    </thead>
    
                    <tbody>
                        <tr v-for="item in items">
                            <td>{{item.id}}</td>
                            <td>{{item.nombre}}</td>
                            <td>{{item.encargado}}</td>
                            <td>{{item.nroFuncionarios}}</td>
                            
                            <td>
                                <a class="app-btn btn-small btn-floating btn-large waves-effect waves-light red"><i class="material-icons" @click="eliminar(item.id)" >delete</i></a>
                                <a class="app-btn btn-small btn-floating btn-large waves-effect waves-light blue "><i class="material-icons" @click="update(item.id)" >edit</i></a>
                            </td>
                        </tr>
                    </tbody>
                </table>
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
            search: '',
            
            payload: {
                nombre: null,
                encargado: null,
                nroFuncionarios:1,
            }
        }
    },
    methods: {
       
        update(id) {
            this.$router.push('/area/' + id);
        },
        eliminar(id) {
            if (confirm("Esta seguro de eliminar?.")) {
                this.axios({
                    method: 'delete',
                    url: this.api + '/areas/' + id
                }).
                then((response) => {
                    this.getList();
                }).
                catch((error) => {
                    console.log(error);
                });
            }
        },
        nuevo() {
            this.axios({
                method: 'post',
                url: this.api + '/areas',
                data: this.payload
            }).
            then((response) => {
                this.getList();
                console.log(response);
            }).
            catch((error) => {
                console.log(error);
            });
        },
        getList(name, value) {

            this.axios({
                method: 'get',
                url: this.api + '/areas?q=' + this.search 
            }).
            then((response) => {
                this.items = response.data;
            }).
            catch((error) => {
                console.log(error);
            })
        }
    },
    components: {

    },
    mounted() {
        this.getList();
        var elems = document.querySelectorAll('select');
        var instances = M.FormSelect.init(elems);
    }
}
</script>
