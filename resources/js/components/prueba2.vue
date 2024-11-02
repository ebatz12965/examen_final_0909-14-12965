<script>
import axios from 'axios';

export default {
    name: 'MarsPhotosCarousel',
    data() {
        return {
            marsPhotos: [],
            loading: true,
            currentIndex: 0, // Para controlar el índice de la foto actual
        };
    },
    mounted() {
        this.fetchMarsPhotos();
    },
    methods: {
        async fetchMarsPhotos() {
            const token = 'DEMO_KEY'; // Usa tu token o el token de demostración
            try {
                const response = await axios.get('https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity/photos', {
                    params: { sol: 1000, api_key: token }
                });
                this.marsPhotos = response.data.photos; // Almacena las fotos
                this.loading = false; // Cambia el estado de carga

                // Barajar las fotos
                this.shufflePhotos();
            } catch (error) {
                console.error('Error al obtener fotos de Marte:', error);
                this.loading = false; // Cambia el estado de carga incluso si hay un error
            }
        },
        shufflePhotos() {
            // Barajar las fotos utilizando Fisher-Yates Shuffle
            for (let i = this.marsPhotos.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [this.marsPhotos[i], this.marsPhotos[j]] = [this.marsPhotos[j], this.marsPhotos[i]];
            }
        },
        nextPhoto() {
            this.currentIndex = (this.currentIndex + 1) % this.marsPhotos.length;
        },
        previousPhoto() {
            this.currentIndex = (this.currentIndex - 1 + this.marsPhotos.length) % this.marsPhotos.length;
        },
    },
    created() {
        // Cambiar la foto automáticamente cada 5 segundos
        setInterval(this.nextPhoto, 5000);
    }
};
</script>

<template>
    <div class="container-sm">
        <h2 class="text-center mt-4" style="background: linear-gradient(to bottom right, #c0c0c0, #666); color: #fff; padding: 10px;">
            Fotos del Rover Curiosity
        </h2>
        <div v-if="loading" class="text-center">Cargando fotos de Marte...</div>
        <div v-else>
            <div id="marsPhotosCarousel" class="carousel slide" data-ride="carousel">
                <div class="carousel-inner">
                    <div
                        class="carousel-item"
                        v-for="(photo, index) in marsPhotos"
                        :key="photo.id"
                        :class="{ active: index === currentIndex }"
                    >
                        <img :src="photo.img_src" class="d-block w-100" :alt="`Foto de Marte ${photo.id}`" />
                        <div class="carousel-caption d-none d-md-block">
                            <h5>Foto ID: {{ photo.id }}</h5>
                            <p><small class="text-muted">Tomada en el sol: {{ photo.sol }}</small></p>
                        </div>
                    </div>
                </div>
                <button class="carousel-control-prev" @click="previousPhoto">
                    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                    <span class="sr-only">Anterior</span>
                </button>
                <button class="carousel-control-next" @click="nextPhoto">
                    <span class="carousel-control-next-icon" aria-hidden="true"></span>
                    <span class="sr-only">Siguiente</span>
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.carousel-item {
    transition: transform 0.5s ease, opacity 0.5s ease;
}
</style>
