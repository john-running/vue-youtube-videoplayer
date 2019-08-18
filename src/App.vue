<template>
    <div class="container">
        <SearchBar @termChange="onTermChange"></SearchBar>
        <div class="row">
            <VideoDetail :video="selectedVideo" class="col-lg-8"/>
            <VideoList @videoSelect="onVideoSelect" :videos="videos" class="col-lg-4"></VideoList>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';

const API_KEY = 'AIzaSyB0ToJFPdt3Dp2stb8ynXqHkNe9U0ivB0c';

export default {
    name: 'App',
    components: {
        SearchBar,
        VideoList,
        VideoDetail
    },
    data() {
        return { 
            videos: [],
            selectedVideo: null
        };
    },
    methods: {
        // same as onTermChange: function() {} (es6 syntax)
        onTermChange(searchTerm) {
            console.log(searchTerm)
            //initiate search for videos from youtube api
            axios.get('https://www.googleapis.com/youtube/v3/search', {
                params: {
                    part: 'snippet',
                    maxResults: 3,
                    key: API_KEY,
                    type: 'video',
                    q: searchTerm
                }
            }).then((response) => {
                this.videos = response.data.items;
                this.selectedVideo = response.data.items[0];
            });
        },
        onVideoSelect(video){
            this.selectedVideo = video;
        }
    }
}
</script>