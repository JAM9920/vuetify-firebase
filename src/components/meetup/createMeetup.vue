<template>
    <v-container grid-list-xs>
        <v-layout row wrap>
            <v-flex xs12>
                <h4 class="primary--text">Create new Meetup</h4>
            </v-flex>
        </v-layout>
        <v-layout row wrap>
            <v-flex xs12>
                <form @submit.prevent="onCreateMeetup">
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <v-text-field
                                name="title"
                                label="Title"
                                id="title" 
                                v-model="title"
                                required
                            ></v-text-field>
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <v-text-field
                                name="location"
                                label="Location"
                                id="location" 
                                v-model="location" 
                                required
                            ></v-text-field>
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <v-btn color="success" raised @click="onPickFile">Upload Image</v-btn>
                            <input type="file" 
                            style="display: none;" 
                            ref="fileInput" 
                            accept="image/*"
                            @change="onFilePicked">
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <img :src="imageUrl" alt="" height="150">
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <v-text-field
                                name="description"
                                label="Description"
                                id="description"
                                multi-line
                                v-model="description" 
                                required
                            ></v-text-field>
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <h4>Choose a date & Time</h4> 
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap class="mb-2">
                        <v-flex xs12 sm6 offset-sm3>
                            <v-date-picker v-model="date"></v-date-picker>
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <v-time-picker v-model="time" format="24hr"></v-time-picker>
                        </v-flex>
                    </v-layout>
                    <v-layout row wrap>
                        <v-flex xs12 sm6 offset-sm3>
                            <v-btn color="primary" :disabled="!formIsValid"
                            type="submit" 
                            >Create Meetup</v-btn>
                        </v-flex>
                    </v-layout>
                </form>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            title: '',
            location: '',
            imageUrl: '',
            description: '',
            date: new Date().toISOString().substr(0, 10),
            time: null,
            image: null
        }
    },
    computed: {
        formIsValid () {
            return this.title !== '' && 
            this.location !== '' && 
            this.imageUrl !== '' && 
            this.description !== ''
        },
        submittableDateTime() {
            const date = new Date(this.date)
            if(typeof this.time === 'string') {
                let hours = this.time.match(/^(\d+)/)[1]
                const minutes = this.time.match(/:(\d+)/)[1]
                date.setHours(hours)
                date.setMinutes(minutes)
            } else {
                date.setHours(this.time.getHours())
                date.setMinutes(this.time.getMinutes()) 
            }
            return date
        }
    },
    methods: {
        onCreateMeetup () {
            if (!this.formIsValid) {
                return 
            }
            if(!this.image) {
                return
            }
            const meetupData = {
                title: this.title,
                location: this.location,
                image: this.image,
                description: this.description,
                date: this.submittableDateTime
            }
            this.$store.dispatch('createMeetup', meetupData)
            this.$router.push('/meetups')
        },
        onPickFile() {
            this.$refs.fileInput.click();
        },
        onFilePicked(event) {
            const files = event.target.files
            let filename = files[0].name
            if(filename.lastIndexOf('.') <= 0) {
                return alert('Please add a valid file!')
            }
            console.log('kirdi')
            const fileReader = new FileReader()
            fileReader.addEventListener('load', () => {
                this.imageUrl = fileReader.result
            })
            fileReader.readAsDataURL(files[0])
            this.image = files[0]
        }
    },
}
</script>

<style>
    
</style> 