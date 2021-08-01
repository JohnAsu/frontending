<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Tv's</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Tv's</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="tv in tvs" :key="tv.id" @click="onSelected(tv)">
                            {{tv.brand}} <span class="float-right">{{tv.value}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <TvView :tv="selectedTv" @saved="onChange" @newTv="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tvs: [],
            selectedTv: {}
        }
    },
    methods: {
        async getMyTvs() {
            await this.$axios.get('/api/tvs')
            .then((res)=>{
                if(res.status==200) {
                    this.tvs = res.data
                }
            })
        },
        onChange() {
            this.getMyTvs()
            this.selectedTv = {}
        },
        onSelected(tv) {
            this.selectedTv = tv;
        },
        onNew() {
            this.selectedTv = {}
        },
    },
    created() {
        this.getMyTvs()
    }
}
</script>

<style>
.row {
    padding-top: 30px;
}
.container {
    margin-top: 20px;
}
h1 {
    text-align: center;
}
.btn-li {
    cursor: pointer;
}
.btn-li:hover {
    background-color: rgb(19, 6, 17);
}
</style>
