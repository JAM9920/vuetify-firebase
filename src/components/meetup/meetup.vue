<template>
    <v-container grid-list-xs>
        <v-layout row wrap>
            <v-flex xs12>
                <v-card>
                    <v-card-title primary-title>
                        <h6 class="primary--text">{{meetup.title}}</h6>
                        <template v-if="true">
                        <v-spacer></v-spacer>
                        <app-edit-meetup-details-dialog></app-edit-meetup-details-dialog>                            
                        </template>
                    </v-card-title>
                </v-card>
                <v-img
                    height="400px"
                    :src="meetup.imageUrl"
                ></v-img>
                <v-card-text>
                    <div class="info--text">{{meetup.date | date}} -{{meetup.location}}</div>
                    <div>{{meetup.description}}</div>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary">Register</v-btn>
                </v-card-actions>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
export default {
    props: ['id'],
    computed: {
        meetup() {
            return this.$store.getters.loadedMeetup(this.id)
        }
    },
}
</script>

<style>
    
</style>