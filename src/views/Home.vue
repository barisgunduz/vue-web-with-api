<template>
    <div>
        <h2>News</h2>
        <div class="container">
            <div class="row">
                <div
                    class="col-md-4"
                    v-for="allData in allDatas"
                    v-bind:key="allData.source.id + randomNumber()"
                >
                    <div class="card mb-3" style="max-width: 540px;">
                        <div class="row g-0">
                            <div class="col-md-12">
                                <img
                                    :src="allData.urlToImage"
                                    class="img-fluid rounded-start"
                                    :alt="allData.title"
                                />
                            </div>
                        </div>
                        <div class="row g-0">
                            <div class="col-md-12">
                                <div class="card-body">
                                    <h5 class="card-title">
                                        <a
                                            :href="allData.url"
                                            :title="allData.title"
                                            target="_blank"
                                        >
                                            {{ allData.title }}
                                        </a>
                                    </h5>
                                    <p class="card-text">
                                        {{ allData.description }}
                                    </p>
                                    <p class="card-text">
                                        <small class="text-muted">{{
                                            allData.publishedAt
                                        }}</small>
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "Home",
    data: () => ({
        countryCode: "us",
        allDatas: null,
    }),
    created: function() {
        this.getDataFromApi();
        this.randomNumber();
    },
    methods: {
        getDataFromApi() {
            let fullResponse = [];
            const url = `https://newsapi.org/v2/top-headlines?country=${this.countryCode}&apiKey=c524fd11899e40358afec60bd7d0f87b`;
            axios
                .get(url)
                .then((res) => {
                    fullResponse = res.data.articles.filter(
                        (article) =>
                            article.author &&
                            article.url &&
                            article.urlToImage &&
                            article.description &&
                            article.title
                    );
                    this.allDatas = fullResponse;
                    console.log("response from api : ", fullResponse);
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        randomNumber: function() {
            return Math.floor(Math.random() * 1000) + 1;
        },
    },
};
</script>

<style scoped></style>
