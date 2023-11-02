<template>
    <div class="container">
        <h5>Editar Activo</h5>

        <div class="card">
            <div class="card-content">
                <form @submit.prevent="update()">
                    <p>
                        Tipo Activo:
                        <input type="text" v-model="payload.tipoActivo" required />
                    </p>
                    <p>Marca: <input type="text" v-model="payload.marca" required /></p>
                    <p>Modelo: <input type="text" v-model="payload.modelo" required /></p>
                    <p>Estado: <input type="text" v-model="payload.estado" required /></p>
                    <p v-if="areas.length > 0">
                        <select v-model="payload.areaId">
                            <option :value="area.id" v-for="area in areas">
                                {{ area.nombre }}
                            </option>
                        </select>
                    </p>
                    <button type="submit" class="waves-effect waves-light btn-small">
                        Editar
                    </button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "AreaView",
    data() {
        const api = process.env.VUE_APP_API;
        return {
            api,

            areas: [],
            payload: {
                tipoActivo: null,
                marca: null,
                modelo: null,
                estado: null,
                areaId: null,
            },
        };
    },
    methods: {
        getOne() {
            this.axios({
                method: "get",
                url: this.api + "/areas/" + this.$route.params.id,
            })
                .then((response) => {
                    this.payload = response.data;
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        update() {
            if (confirm("Esta seguro de editar?.")) {
                this.axios({
                    method: "patch",
                    url: this.api + "/areas/" + this.$route.params.id,
                    data: this.payload,
                })
                    .then((response) => {
                        console.log(response);
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            }
        },
        getAreaList() {
            this.axios({
                method: "get",
                url: this.api + "/areas",
            })
                .then((response) => {
                    this.areas = response.data;
                    const intervalo = setTimeout(() => {
                        this.intSelect();
                        clearTimeout(intervalo);
                    }, 3000);
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        intSelect(){
           
            this.getAreaList();
            var elems = document.querySelectorAll('select');
            var instances = M.FormSelect.init(elems);
        }
    },
    components: {},
    mounted() {
        this.getOne();
        this.getAreaList() 
    },
};
</script>
