<template>
    <div class="form-group">
        <label>{{name}} <i class="fa" v-if="activated"
                           :class="validClass"></i></label>
        <input type="text" class="form-control"
               :value="value"
               @input="onInput">
        <div class="alert alert-danger" v-if="!pattern.test(value) && activated">{{ errorMessage }}</div>
    </div>
</template>

<script>
    export default {
        props: ['name', 'value', 'pattern', 'errorMessage'],
        data(){
            return {
                activated: this.value !== ''
            }
        },
        computed: {
            validClass(){
                return this.pattern.test(this.value) ? 'fa-check-circle' : 'fa-exclamation-circle';
            }
        },
        methods: {
            onInput(e){
                this.activated = true;

                this.$emit('changedata', {
                    value: e.target.value,
                    valid: this.pattern.test(e.target.value)
                });
            }
        }
    }
</script>