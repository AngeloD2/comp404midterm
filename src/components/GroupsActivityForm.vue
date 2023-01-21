<script setup>
import axios from 'axios'
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'
import { v4 as uuidv4 } from 'uuid';

</script>

<script>

export default {
    components: { Datepicker},
    data: () => ({
        environmentalgroups: [],
        activities:[],
        filteredData: [],
        count: '',
        dialog: false,
        form: false,
        loading: false,
        activityname: '',
        datestarted: new Date(),
        dateended: new Date(),
        activitydetails: '',
        result: '',
        options: ['Pending', 'Success', 'Failure'],
        date: '',
    }),

    created() {
        // load environmentalgroups data
        axios.get('http://localhost:3000/environmentalgroups/')
            .then(response => {
                console.log(response.data)
                this.environmentalgroups = response.data
            })
            .catch(error => {
                console.log(error)
            })
    },
    methods: {

        async fetchActivities(id) {
            try {
                const res = await axios.get(`http://localhost:3000/EnvironmentalGroups/${id}`)
                this.activities = res.data.activities
            } catch (error) {
                console.log(error)
            }
        },

        openDialog(id) {
            this.dialog = true
            this.id = id
            this.fetchActivities(id)
            this.index = this.activities.length + 1
        },

        onSubmit() {
            if (!this.form) return

            axios.patch(`http://localhost:3000/EnvironmentalGroups/${this.id}/`, {
                ...this.environmentalgroups.activities,
                activities: [
                    {
                        "id": uuidv4(),
                        "activityname": this.activityname,
                        "datestarted": this.datestarted,
                        "dateended": this.dateended,
                        "actdetails": this.activitydetails,
                        "result": this.result
                    }
                ]
            })
                .then(response => {
                    this.loading = false
                    console.log(response)
                    this.$router.push({ name: 'environmentalgroups' })
                })
                .catch(error => {
                    console.log(error)
                })
        },
    }
}

</script>

<template>
    <v-app>
        <v-card>
            <v-card-title>
                <span class="text-h5">Group Activities</span>
            </v-card-title>
            <v-container id="card-container">
                <div v-for="group in environmentalgroups" :key="group.id">
                    <v-card class="equal-card mx-auto" max-width="344" variant="outlined">
                        <v-card-item>
                            <div>
                                <div class="text-overline mb-1">
                                    Environmental Group
                                </div>
                                <div class="text-h6 mb-1">
                                    {{ group.groupname }}
                                </div>
                                <div class="text-caption">{{ group.description }}</div>
                            </div>
                        </v-card-item>

                        <v-card-actions>
                            <v-dialog v-model="dialog">
                                <template v-slot:activator="{ props }">
                                    <v-btn variant="tonal" color="warning" v-bind="props" @click="openDialog(group.id)">
                                        See Activities
                                    </v-btn>
                                </template>
                                <v-card class="modalcont bg-primary d-flex flex-row my-auto mx-auto">

                                    <v-card id="actform" class="px-5 my-auto ms-5 me-5" max-width="400px" height:="200px">
                                        <v-form class="py-3" v-model="form" @submit.prevent="onSubmit">
                                            <input type="hidden" v-model="group.id" />

                                            <v-text-field v-model="activityname" :readonly="loading" :rules="[required]"
                                                class="actname" clearable label="Activity Name">
                                            </v-text-field>
                                            <label for="dateStarted"> Starting Date </label>
                                            <Datepicker name="dateStarted" dark v-model="datestarted"
                                                :readonly="loading" :rules="[required]" class="mb-2" clearable
                                                label="Date started">
                                            </Datepicker>
                                            <label for="dateEnded"> End Date </label>
                                            <Datepicker name="dateEnded" dark v-model="dateended" :readonly="loading"
                                                :rules="[required]" class="mb-2" clearable label="Date ended">
                                            </Datepicker>
                                            <v-textarea multi-line v-model="activitydetails" :readonly="loading"
                                                :rules="[required]" class="mb-2" clearable label="Activity details">
                                            </v-textarea>
                                            <v-select v-model="result" :items="options"
                                                label="Activity Result"></v-select>
                                            <br>
                                            <v-btn class="actsubmit" :disabled="!form" :loading="loading" block
                                                color="success" size="large" type="submit" variant="elevated">
                                                Submit
                                            </v-btn>
                                        </v-form>
                                    </v-card>

                                    <v-card id="actlist" class="ms-5 px-5 my-auto" min-height="570px" min-width="400px" max-width="500px"
                                        height:="200px">
                                        <span> List of Activities </span>
                                        <v-list v-for="(activity, index) of activities" :key="activity.id" class="d-flex justify-content-center align-items-center text-center">
                                             <v-list-item >
                                               <span class="text-light"> Activity</span> {{ index + 1 }}.) {{ activity.activityname }}
                                             </v-list-item> 
                                        </v-list>
                                    </v-card>
                                    <v-card-actions class="d-block mt-5">
                                        <v-btn color="secondary bg-black" block @click="dialog = false">Close</v-btn>
                                    </v-card-actions>
                                </v-card>
                            </v-dialog>
                        </v-card-actions>
                    </v-card>
                </div>
            </v-container>
        </v-card>
    </v-app>
</template>

  

<style scoped>
.equal-card {
    width: 250px;
    /* set the width of the card to 100% */
    height: 200px;
    /* set the height of the card to 100% */
}

#card-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 25px;
}

.modalcont {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    min-height: 90vh;
}

</style>