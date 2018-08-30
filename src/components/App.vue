<template>
    <div id="app">
        <div class="container">
            <div class="sample">
                <form v-if="!flags.successForm" @submit.prevent="flags.successForm = true">
                    <div class="progress">
                        <div class="progress-bar" :style="properties"></div>
                    </div>
                    <div>
                        <form-field v-for="(item, index) in info"
                                    :name="item.name"
                                    :value="item.value"
                                    :pattern="item.pattern"
                                    :errorMessage="item.errorMessage"
                                    :key="index"
                                    @changedata="onChangeData(index, $event)">
                        </form-field>
                    </div>
                    <button class="btn btn-primary" :disabled="done < info.length">
                        Send Data
                    </button>
                </form>
                <div v-else="flags.successForm">
                    <table class="table table-bordered">
                        <tr v-for="item in info">
                            <td style="width: 50%">{{item.name}}</td>
                            <td style="width: 50%">{{item.value}}</td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import FormField from './FormField.vue'
    export default {
        data(){
            return {
                info: [
                    {
                        name: 'Name',
                        value: '',
                        type: 'text',
                        pattern: /^[a-zA-Z ]{2,30}$/,
                        errorMessage: 'Имя должно быть на латинице и содержать от 2 до 30 символов'
                    },
                    {
                        name: 'Phone',
                        value: '',
                        type: 'text',
                        pattern: /^\+380\d{3}\d{2}\d{2}\d{2}$/,
                        errorMessage: 'Телефон должен содержать код Украины и начинаться на +380'
                    },
                    {
                        name: 'Email',
                        value: '',
                        type: 'email',
                        pattern: /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
                        errorMessage: 'Не корректный email'
                    },
                    {
                        name: 'Address',
                        value: '',
                        type: 'text',
                        pattern: /^[a-zA-Z0-9 ]{10,100}$/,
                        errorMessage: 'Адрес должен быть на латинице и содержать от 10 до 100 символов'
                    },
                    {
                        name: 'PostCode',
                        value: '',
                        type: 'text',
                        pattern: /^[0-9]{5}$/,
                        errorMessage: 'Индекс должен состоять ровно из пяти цыфр'
                    }
                ],
                flags: {
                    successForm: false,
                },
                controls: [],
                done: 0,
            }
        },
        created(){
            this.info.forEach(function(item){
                this.controls.push(item.pattern.test(item.value));
            });
        },
        methods: {
            onChangeData(index, data){
                this.info[index].value = data.value;
                this.controls[index] = data.valid;

                let done = 0;
                
                this.controls.map(function(item){
                    if (item) done++;
                });

                this.done = done;
            }
        },
        computed: {
            properties(){
                return {
                    width: (this.done / this.info.length * 100) + '%'
                }
            }
        },
        components: {
            FormField
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    h1, h2 {
        font-weight: normal;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }

    label{
        font-weight: bold;
        text-align: left;
    }

    i.fa-exclamation-circle:before{
        color: #dc3545;
    }

    i.fa-check-circle:before{
        color: #28a745;
    }

    .progress{
        margin-bottom: 30px;
    }

    .sample .form-group{
        text-align: left;
    }
</style>
