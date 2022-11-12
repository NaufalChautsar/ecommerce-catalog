<template>
    <div class="container">
        <img
            v-if="product.category === 'women\'s clothing'"
            class="background"
            src="../assets/images/background-women.svg"
            alt="Women" />
        <img
            v-if="product.category === 'men\'s clothing'"
            class="background"
            src="../assets/images/background-men.svg"
            alt="Men" />
        <img
            v-if="unvailableProduct === true"
            class="background"
            src="../assets/images/background-undifined.svg"
            alt="Unvailable" />

        <div
            v-if="loading === true"
            class="loader"></div>

        <main v-if="unvailableProduct === false">
            <section>
                <div class="box">
                    <img
                        :src="product.image"
                        :alt="product.title"
                        class="products-image" />
                    <div class="description-products">
                        <h1 :class="color">{{ product.title }}</h1>
                        <div class="detail-products">
                            <p class="category">{{ product.category }}</p>
                            <div class="rating">
                                <p>{{ product.rating.rate }}/5.0</p>
                                <div :class="elipse"></div>
                                <div :class="elipse"></div>
                                <div :class="elipse"></div>
                                <div class="elipse-blank"></div>
                                <div class="elipse-blank"></div>
                            </div>
                        </div>
                        <h6 class="description">
                            {{ product.description }}
                        </h6>
                        <h5 :class="price">${{ product.price }}</h5>
                        <div class="button">
                            <button :class="button">Buy now</button>
                            <button
                                :class="borderButton"
                                @click.prevent="nextProduct">
                                Next product
                            </button>
                        </div>
                    </div>
                </div>
            </section>
        </main>

        <main v-else>
            <section>
                <div class="box">
                    <img
                        class="image-product-unvailable"
                        src="../assets/images/product-unvailable.svg"
                        alt="Product unvailable" />
                    <div class="description-unvailable">
                        <h1 :class="color">This product is unvailable to show</h1>
                        <button
                            :class="borderButton"
                            @click.prevent="nextProduct">
                            Next product
                        </button>
                    </div>
                </div>
            </section>
        </main>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'List-products',
    data() {
        return {
            product: [],
            unvailableProduct: false,
            index: null,
            color: '',
            button: '',
            borderButton: '',
            elipse: '',
            price: '',
            loading: false,
        };
    },
    methods: {
        setProduct(data) {
            this.product = data;
        },
        nextProduct() {
            if (this.index < 20) {
                this.index++;
                this.getData();
            } else {
                this.index = 1;
                this.getData();
            }
        },
        getData() {
            axios
                .get(`https://fakestoreapi.com/products/${this.index}`)
                .then((response) => {
                    this.loading;
                    if (response.data.category === "women's clothing") {
                        this.loading = false;
                        this.color = 'title-women';
                        this.button = 'cta-buy-women';
                        this.price = 'price-women';
                        this.borderButton = 'cta-next-women';
                        this.elipse = 'elipse-color-women';
                        this.unvailableProduct = false;
                    } else if (response.data.category === "men's clothing") {
                        this.loading = false;
                        this.color = 'title-men';
                        this.button = 'cta-buy-men';
                        this.price = 'price-men';
                        this.borderButton = 'cta-next-men';
                        this.elipse = 'elipse-color-men';
                        this.unvailableProduct = false;
                    } else {
                        this.loading = false;
                        this.color = 'title-unvailable';
                        this.borderButton = 'cta-next-unvailable';
                        this.unvailableProduct = true;
                    }

                    this.setProduct(response.data);
                })
                .catch((error) => {
                    console.error('Gagal : ', error);
                });
        },
    },
    beforeMount() {
        this.index = 1;
    },
    mounted() {
        this.getData();
    },
};
</script>
