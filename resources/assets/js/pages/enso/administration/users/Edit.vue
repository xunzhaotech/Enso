<template>

    <div class="container">
        <div class="columns is-centered">
            <div class="column is-three-quarters">
                <vue-form :data="form"
                    class="box animated fadeIn"
                    v-if="initialised">
                    <template slot="owner_id" slot-scope="props">
                        <vue-select name="owner_id"
                            v-model="props.field.value"
                            @input="pivotParams.owners.id=$event;props.errors.clear(props.field.column)"
                            :source="props.field.meta.source">
                        </vue-select>
                    </template>
                    <template slot="role_id" slot-scope="props">
                        <vue-select name="role_id"
                            :pivot-params="pivotParams"
                            v-model="props.field.value"
                            @input="props.errors.clear(props.field.column);"
                            :source="props.field.meta.source">
                        </vue-select>
                    </template>
                </vue-form>
            </div>
        </div>
    </div>

</template>

<script>

import VueForm from '../../../../components/enso/vueforms/VueForm.vue';
import VueSelect from '../../../../components/enso/select/VueSelect.vue';

export default {
    components: { VueForm, VueSelect },

    data() {
        return {
            initialised: false,
            form: {},
            pivotParams: { owners: { id: null } },
        };
    },

    created() {
        axios.get(route(this.$route.name, this.$route.params.id, false)).then(({ data }) => {
            this.form = data.form;
            this.initialised = true;
            this.pivotParams.owners.id = this.getOwnerId();
        }).catch(error => this.handleError(error));
    },

    methods: {
        getOwnerId() {
            const attribute = this.form.fields.find(attr => attr.column === 'owner_id');

            return attribute.value;
        },
    },
};

</script>
