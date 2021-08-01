<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New TV
        </button>
        <h5>Television View</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
            <b-form-group label="Brand">
                <b-form-input v-model="tv.brand"></b-form-input>
            </b-form-group>
            <b-form-group label="Model">
                <b-form-input v-model="tv.model"></b-form-input>
            </b-form-group>
            <b-form-group label="Description">
                <b-form-input v-model="tv.description"></b-form-input>
            </b-form-group>
            <b-form-group label="Price">
                <b-form-input v-model="tv.price"></b-form-input>
            </b-form-group>
            <b-form-group
              label="Type"
              label-for="Type"
            >
              <b-form-select v-model="tv.type" :options="types"></b-form-select>
            </b-form-group>
            <b-form-group>
                <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="tv.id">Delete TV</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        tv: {},
    },
    data() {
      return {
        types: [
          {value: 'vintage tv', text: 'Vintage Tv'},
          {value: 'modern tv', text: 'Modern Tv'}
        ]
      }
    },
    methods: {
        async onSave() {
            try {
                if(!this.tv.id) {
                    await this.$axios.post('/api/tvs', this.tv)
                }else{
                    await this.$axios.put('/api/tvs/' + this.tv.id, this.tv)
                }

                this.$emit('saved')
            } catch (err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newTv')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/tvs/' + this.tv.id)
                this.$emit('deleted')
            } catch (err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>
