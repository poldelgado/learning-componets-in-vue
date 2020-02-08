<template>
    <div>
        <h2 class="text-center">Captura tus ideas</h2>
        <div class="well">
            <h4>¿En qué estas pensando?</h4>
            <form v-on:submit.prevent="createIdea">
                <div class="input-group">
                    <input type="text" class="form-control input-sm" maxlength="256" v-model="newIdea">
                    <span class="input-group-btn">
                        <button type="submit" class="btn btn-primary btn-sm">Agregar</button>
                    </span>
                </div>
            </form>
            <hr>
            <ul class="list-unstyled">
                <li v-for="idea in ideas">
                    <p>
                        <small class="text-muted">
                            <em>{{ since(idea.created_at) }}</em>
                        </small>
                        {{ idea.description }}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import toastr from 'toastr'
    import moment from 'moment'

    moment.locale('es');

    export default {
        data () {
            return {
                ideas:      [],
                newIdea:    '',
            }
        },
        methods: {
            since(fecha) {
                return moment(fecha).fromNow();
            },
            getIdeas() {
                var urlIdeas = 'mis-ideas';
                axios.get(urlIdeas).then(response => {
                    this.ideas = response.data
                });
            },
            createIdea() {
                var url = 'guardar-idea';
                axios.post(url, {
                    description: this.newIdea
                }).then(response => {
                    this.getIdeas();
                    this.newIdea = '';
                    toastr.success('Nueva idea creada');
                }).catch(error => {
                    toastr.error('Error');
                });
            }
        },
        created: function() {
            this.getIdeas();
        }
    }
    
</script>
