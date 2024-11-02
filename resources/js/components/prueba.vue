<script>
import axios from 'axios';

export default {
    name: 'NasaInfo',
    data() {
        return {
            nasaData: null,
        };
    },
    mounted() {
        this.fetchNasaData();
    },
    methods: {
        async fetchNasaData() {
            const token = 'aQMfzbXymZGC1GFGaroeLHUESAhRnqk3m66YKCBX'; // Tu token de API
            try {
                const response = await axios.get('https://api.nasa.gov/planetary/apod', {
                    params: { api_key: token }
                });
                this.nasaData = response.data;
            } catch (error) {
                console.error('Error al obtener datos de la API:', error);
            }
        }
    }
};
</script>

<template>
    <div class="container-sm">
        <h2 class="text-center mt-4" style="background: linear-gradient(to bottom right, #003366, #3399ff); color: #fff; padding: 10px;">
            Astronomy Picture of the Day
        </h2>
        <div v-if="nasaData" class="card mt-3 shadow">
            <img :src="nasaData.url" class="card-img-top" :alt="nasaData.title" v-if="nasaData.media_type === 'image'" />
            <div class="card-body">
                <h5 class="card-title">{{ nasaData.title }}</h5>
                <p class="card-text">{{ nasaData.explanation }}</p>
                <p class="card-text"><small class="text-muted">Fecha: {{ nasaData.date }}</small></p>
                <p class="card-text"><small class="text-muted">Autor: {{ nasaData.copyright }}</small></p>
            </div>
        </div>
        <div v-else>
            <p class="text-center">Cargando datos de la NASA...</p>
        </div>
    </div>
</template>

<style scoped>
.text-center {
    text-align: center;
}
.card {
    margin: auto;
    max-width: 600px;
}
.card-img-top {
    max-width: 100%;
    height: auto;
}
.shadow {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
</style>
