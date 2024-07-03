<script>
    import quarz from '../assets/quarz.jpg'
    import curren from '../assets/curren.jpeg'
    import currenSport from '../assets/curren-sport.jpg'
    import jaragar from '../assets/jaragar.jpg'
    import liges from '../assets/liges.jpg'
    import maserati from '../assets/maserati.jpg'
    import mecanique from '../assets/mecanique.jpg'
    import designer from '../assets/designer.jpg'
    import relogio from '../assets/relogio.jpg'
    import tissot from '../assets/tissot.jpg'
    import gold from '../assets/gold.jpg'
    import genova from '../assets/genova.jpg'
    export default{
        data(){
            return{
                cart: [],
                searchKey: '',
                shoppingList:[
                    {id: 1, description: 'Quarz Luxe', price: 12, image: quarz},
                    {id: 2, description: 'Curren Business', price: 20, image: curren},
                    {id: 3, description: 'Curren Sport', price: 5, image: currenSport},
                    { id: 4, description: 'Jaragar Racing', price: 8, image: jaragar},
                    { id: 5, description: 'Liges Hommes', price: 3, image: liges},
                    { id: 6, description: 'Maserati Mechanical', price: 65, image: maserati},
                    { id: 7, description: 'Montre Mecanique', price: 25, image: mecanique},
                    { id: 8, description: 'Brand designer', price: 28, image: designer},
                    { id: 9, description: 'Relogio Masculino', price: 4, image: relogio},
                    { id: 10, description: 'Tissot Multifunction', price: 29, image: tissot},
                    { id: 11, description: 'Hip Hop Gold', price: 87, image: gold},
                    { id: 12, description: 'Mesh Genova', price: 6, image: genova},
                ]
            }
        },
        computed:{
            filterShoppingList(){
                return this.shoppingList.filter(item => {
                    return item.description.toLocaleLowerCase().includes(this.searchKey.toLocaleLowerCase())
                })
            },
            prixTotal(){
                let total = 0;
                for(let item in this.cart){
                    total = total + (this.cart[item].quantity * this.cart[item].price)
                }
                return total
            },
            quantityTotal(){
                let quantite = 0;
                for(let item in this.cart){
                    quantite = quantite + this.cart[item].quantity
                }
                return quantite
            }
        },
        methods:{
            addCart(item){
                // permet d'ajouter l'element qu'une seule fois dans cart
                for(let i=0; i<this.cart.length; i++){
                    if(this.cart[i].id === item.id){
                        return this.cart[i]
                    }
                }
                this.cart.push({
                    id: item.id,
                    description: item.description,
                    price: item.price,
                    image: item.image,
                    quantity: 1
                })
            },
            cartPlusOne(item){
                item.quantity = item.quantity + 1;
            },
            cartMinusOne(item, id){
                if(item.quantity == 1){
                    this.deleteItem(id)
                }else{
                    item.quantity = item.quantity - 1;
                }
            },
            deleteItem(id){
                this.cart.splice(id, 1)
            }
        }
    }
</script>

<template>
    <h1 class="ml-20 m-2">les articles</h1>
    <p><input type="search" placeholder="rechercher..." v-model="searchKey" class="border-2 rounded-full w-5/6 h-10 ml-16"/></p>
    <div class="ml-20 m-2 text-2xl font-bold" v-if="searchKey && filterShoppingList.length >=1">
        {{ filterShoppingList.length}} resultat<span v-if="searchKey && filterShoppingList.length >=2">s</span> pour "{{ searchKey }}"
    </div>
    <div class="ml-20 m-2" v-if="filterShoppingList == 0">
        <div>
            <h1 class="text-2xl font-bold">Aucun resultat pour "{{ searchKey }}"</h1>
            <h3>Avez-vous essaye de: vérifier l'orthographe ?</h3>
        </div>
    </div>
   <div class="flex justify-around mt-10">
        <div class="grid grid-rows-2 grid-cols-5 gap-4" >
            <div v-for="item in filterShoppingList" key="id" class="border-2 w-52">
                <img :src="item.image" alt="montre" class="object-cover w-full h-48"/>
                <p class="font-medium">{{ item.description }}</p>
                <p>{{item.price }} €</p>
                <div>
                    <button @click="addCart(item)"><i class="fa-solid fa-cart-shopping"></i></button>
                </div>
            </div>
        </div>

        <div v-if="cart.length > 0" class="w-80 border-2 bg-sky-400 p-5">
                <h1 class="text-center text-white font-medium">Panier</h1>
                <div v-for="(item, id) in cart" :key="item.id">
                    <p><img :src="item.image" class="objectif-cover w-10 rounded-full"/></p>
                    <p class="text-white font-medium">{{ item.description }}</p>
                    <p class="text-slate-200">{{ item.price }} €</p>
                    <span class="text-xs font-medium">quantite: {{ item.quantity }}</span>
                    <button class="ml-1" @click="cartPlusOne(item)"><i class="fa-solid fa-circle-plus text-slate-100"></i></button>
                    <button class="ml-1" @click="cartMinusOne(item)"><i class="fa-solid fa-circle-minus text-slate-100"></i></button>
                    <button class="ml-1" @click="deleteItem(id)"><i class="fa-solid fa-trash text-slate-100"></i></button>
                    <div class="w-64 h-0.5 bg-sky-500 my-3"></div>
                </div>
                <p class="pl-5 text-white font-medium">Total: {{ prixTotal }} €</p>
                <p class="pl-5 text-xs font-medium">Total d'article: {{ quantityTotal }}</p>
                <div class="w-64 h-10 py-2 my-2 rounded-md bg-white m-auto text-center font-medium">Commander</div>
        </div>
   </div>
</template>

<style>
  
</style>