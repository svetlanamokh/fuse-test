<template>
    <section>
        <div class="container">
            <h1>Our Latest Developments</h1>
            <div class="filter">
                <p class="filter__text">Filter</p>
                <input class="filter__placeholder" type="text" v-if="homes" v-model="search">
            </div>

            <div class="cards" v-if="homes">
                <a :href="'/details/' + home.id" class="card" v-for="home in cardsFilter" :key="home.id">
                    <img src="img/house.png" />
                    <h3 class="card__rectangle" 
                        :class=" { blue: home.type === 'IndependentLiving', 
                        orange: home.type === 'SupportAvailable'} ">{{ home.type }}
                    </h3>
                    <div class="card__content" >
                        <h2>{{ home.title }}</h2>
                        <p class="standart"> {{ home.address }} <br></p>
                        <p class="standart__down"> New Properties for Sale from <b>£{{ home.price }}</b></p>
                        <p class="small">Shared Ownership Available</p>
                    </div>
                </a>

            </div>

            <button class="btn">
                See more 
                <svg width="7" height="17" viewBox="0 0 7 17" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path id="Chevron" d="M1 1L5.043 8.41667L1 15.8333" stroke="#363636" stroke-width="2" stroke-linecap="round"/>
                </svg>
            </button>

        </div>
    </section>
</template>

<script>
import axios from 'axios'
export default {
    data () {
        return {
            homes: null,
            search: ''
        }
    },
    methods: {
        getCardsInfo () {
            axios
                .get('https://603e38c548171b0017b2ecf7.mockapi.io/homes')
                    .then(response => {
                        this.homes = response.data
                        console.log(this.homes)
                    })
        }
    },
    mounted () {
        this.getCardsInfo()
    },
    computed: {
        cardsFilter () {
            let array = this.homes,
                search = this.search

                if (!search || search.length < 4) return array

                search = search.trim().toLowerCase()

                array = array.filter(function (item) {
                    if (item.title.toLowerCase().indexOf(search) !== -1) {
                        return item
                    }
                })
                return array
        }
    }
}
</script>
