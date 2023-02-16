<template>
<section>
    <base-card>
        <h2>Submitted Experiences</h2>
        <div>
            <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
        </div>
        <ul>
            <p v-if="isLoading">Loading</p>
            <survey-result v-else v-for="result in results" :key="result.id" :name="result.name" :rating="result.rating"></survey-result>
        </ul>
    </base-card>
</section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
    components: {
        SurveyResult,
    },
    data() {
        return {
            results: [],
            isLoading: true,
        }
    },
    methods: {
        loadExperiences() {
            fetch('https://vue-http-demo-f598c-default-rtdb.firebaseio.com/surveys.json')
                .then((resp) => {
                    if (resp.ok) {
                        return resp.json()
                    }
                })
                .then((data) => {
                    const result = []
                    for (const id in data) {
                        result.push({
                            id,
                            name: data[id].name,
                            rating: data[id].rating
                        })
                    }
                    this.results = result
                    this.isLoading = false
                })
        }
    },
    mounted() {
        this.loadExperiences()
    }
};
</script>

<style scoped>
ul {
    list-style: none;
    margin: 0;
    padding: 0;
}
</style>
