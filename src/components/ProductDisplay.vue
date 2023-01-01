<template>
    <div class="container">
        <div v-if="isLoading" class="card">
            <div class="productContainer">
                <div class="loader"></div>
            </div>
        </div>
        <div v-else class="container" :class="!isProductAvailable ? 'bgUnavailable' : product.data.category === 'men\'s clothing' ? 'bgMan' : 'bgWoman'">
            <div class="overlay">
                <img src="../assets/bg/bg-pattern.svg" alt="background overlay">
            </div>
            <div class="card">
                <div v-if="!isProductAvailable" class="productUnavailableContainer">
                    <div class="overlay">
                        <img src="../assets/bg/sad-face.svg" alt="background unavailable product" srcset="">
                    </div>
                    <div class="productDetails">
                        <p>This product is unavailable to show</p>
                        <div class="button">
                            <button type="button" @click="getSingleProduct()" class="buttonNext">Next Product</button>
                        </div>
                    </div>
                </div>
                <div v-else class="productContainer">
                    <div class="productThumbnail">
                        <img :src="product.data.image" alt="">
                    </div>
                    <div class="productDetails">
                        <div class="top">
                            <h3 :class="product.data.category === 'men\'s clothing' ? 'fontNavy' : 'fontPurple'" class="title">{{ product.data.title }}</h3>
                            <div class="subTitle">
                                <span>{{ product.data.category }}</span>
                                <div class="rating">
                                    <span>{{ product.data.rating.rate }}/5</span>
                                    <div class="rating">
                                        <span :class="product.data.category === 'men\'s clothing' ? 'colorNavy' : 'colorPurple'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'colorNavy' : 'colorPurple'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'colorNavy' : 'colorPurple'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'colorNavy' : 'colorPurple'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'colorNavy' : 'colorPurple'" class="circle"></span>
                                    </div>
                                </div>
                            </div>
                            <div class="description">
                                <p>{{ product.data.description }}</p>
                            </div>
                        </div>
                        <div class="bottom">
                            <span :class="product.data.category === 'men\'s clothing' ? 'fontNavy' : 'fontPurple'" class="price">${{ product.data.price }}</span>
                            <div class="button">
                                <button type="button" :class="product.data.category === 'men\'s clothing' ? 'colorNavy' : 'colorPurple'" class="buttonBuy">Buy Now</button>
                                <button type="button" @click="getSingleProduct()" :class="product.data.category === 'men\'s clothing' ? 'borderNavy fontNavy' : 'borderPurple fontPurple'" class="buttonNext">Next Product</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductDisplay',
    data() {
        return {
            isLoading: false,
            index: 0,
            isProductAvailable: false,
            product: {}
        }
    },
    methods: {
        async getProductAPI() {
            const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
            const result = await response.json();
            return result;
        },
        async getSingleProduct() {
            this.isLoading = true;
            if (this.index !== 20) {
                this.index++
            } else {
                this.index = 1;
            }
            let data = await this.getProductAPI()
            if (data.category === "men's clothing" || data.category === "women's clothing") {
                this.product = { data }
                this.isProductAvailable = true;
            } else {
                this.isProductAvailable = false;
            }
            this.isLoading = false;
        }
    },
    mounted() {
        this.getSingleProduct();
    },
}
</script>

<style scoped>
    @import '../assets/style/page.css'
</style>