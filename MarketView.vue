<template>
        <!-- INICIO -->
    <div class="container">
        <div class="wrapperr">
            <h6><b> BonIce! muito refrescante e delicioso! </b></h6>
            <span>
                    <button @click="karting(1)" >
                        <i class="shopping-cart"></i>
                    </button>   
            </span>
            <!-- ITEMS -->
                <!-- EM LISTA -->
            <div class="items" v-for="(produto,id) in productsList" :key="id">
                <div class="item">
                    <img :src="produto.url" alt="item" class="img"/>
                    <h2>{{produto.nome}}</h2>

                    <p>Price: <em> <b> {{produto.preco}}€ </b> </em>
                    </p>
                    <button class="add-to-cart" type="button" @click="validation(id)">Add to cart</button>
                    <div v-if="alerta==id">
                        <h2>Quantity:</h2>
                        <input type="number" class="input" v-model="quantityINname">
                        <button @click="add(id, quantityINname)">ADD</button>
                    </div>
                </div>
            </div>
                <!-- EM FIREBASE -->
            <div class="items" v-for="(produto,id) in baseproducts" :key="id">
                <div class="item">
                    <img :src="produto.url" alt="item" class="img"/>
                    <h2>{{produto.nome}}</h2>

                    <p>Price: <em> <b> {{produto.preco}}€ </b> </em>
                    </p>
                    <button class="add-to-cart" type="button" @click="validation(id)">Add to cart</button>
                    <div v-if="alerta==id">
                        <h2>Quantity:</h2>
                        <input type="number" class="input" v-model="quantityINname">
                        <h2>{{produto}}</h2>
                        <h2>{{id}}</h2>
                        <button @click="add2(id, quantityINname)">ADD</button>
                    </div>
                </div>
            </div>
            <!-- KART -->
        </div>
        <div v-if="kart==1" class="kart">
            <h1> Shopping Kart </h1>
            <button @click="karting(0)">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x-circle-fill" viewBox="0 0 16 16">
                    <path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zM5.354 4.646a.5.5 0 1 0-.708.708L7.293 8l-2.647 2.646a.5.5 0 0 0 .708.708L8 8.707l2.646 2.647a.5.5 0 0 0 .708-.708L8.707 8l2.647-2.646a.5.5 0 0 0-.708-.708L8 7.293 5.354 4.646z"/>
                </svg>
                Close
            </button>

            <div class="produto">    
                <ul v-for="(product, id) in productsList" :key="id">
                    <tr v-if="product.qtd>0">{{product.nome}}</tr>
                    <tr v-if="product.qtd>0">
                        <p>quantidade:  {{product.qtd}} 
                            <button @click="add(id, 0)" v-if="product.qtd>0">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-arrow-down-left" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" d="M2 13.5a.5.5 0 0 0 .5.5h6a.5.5 0 0 0 0-1H3.707L13.854 2.854a.5.5 0 0 0-.708-.708L3 12.293V7.5a.5.5 0 0 0-1 0v6z"/>
                                </svg>
                                Delete
                            </button>
                        </p>
                    </tr>
                    
                </ul>
                <div v-if="total!=0">
                    <p> Total a pagar: <b> {{total}} €</b></p>
                </div>
                <div v-if="total==0">
                    <p>
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-exclamation" viewBox="0 0 16 16">
                            <path d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.553.553 0 0 1-1.1 0L7.1 4.995z"/>
                        </svg>
                        Não há produtos no carrinho.
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-exclamation" viewBox="0 0 16 16">
                            <path d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.553.553 0 0 1-1.1 0L7.1 4.995z"/>
                        </svg>
                    </p>
                </div>
                
            </div>
            
        </div>
    </div>
    <!--/ wrapper -->
</template>
<script>
import axios from 'axios';
    export default {
        name: "HomeView",
        data() {
            return{
                baseproducts: null,
                alerta:-1,
                kart:0,
                quantityINname: 0,
                productsList: [
                    {
                        url: "https://m.media-amazon.com/images/I/4104V96q6sS._SX466_.jpg",
                        nome: "Bonice Twin 10und",
                        qtd: 0,
                        preco: 10.00,
                    },
                    {
                        url: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTVe4F42I5Ls_J-ujiTLRtGKKkVogNUWRKE94THvpq0KEPk_seEIRjkF_F9L6UHvHOWwZo&usqp=CAU",
                        nome: "Bonice Pessego 10und",
                        qtd: 0,
                        preco: 9.00,
                    },
                    {
                        url: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSZ6PSKgfwhfH4Sjyj-9Fu-OxD8FcHevfIqCFWZt0gbO_pkqVNk2t_ye9HFLUNDDcQPfsM&usqp=CAU",
                        nome: "Bonice Limão 10und",
                        qtd: 0,
                        preco: 10.00,
                    },
                ],
                total:0
            }
        },
        async created(){
            //GET firebase
            const response = await axios.get(
                "https://proyecto1-3146f-default-rtdb.firebaseio.com/.json"
            );
            this.baseproducts=response.data;//variavel baseproducts tem os produtos
        },

        methods:{
            validation(x){
                this.alerta=x;
            },
            karting(x){
                this.kart=x;
            },
            add(x,y){
                this.productsList[x].qtd=y;
                this.calculo();
                this.quantityINname=0;
                this.alerta=-1;
            },
            add2(x,y){
                this.baseproducts[x].qtd=y;
                this.calculo();
                this.quantityINname=0;
                this.alerta=-1;
            },
            calculo(){
                this.total=0
                for (let index = 0; index < this.productsList.length; index++) {
                    if (this.productsList[index].qtd<0){
                        this.productsList[index].qtd=0
                    }
                    this.total+=this.productsList[index].qtd*this.productsList[index].preco;
                }
            },
            calculo2(){
                this.total=0
                for (let index = 0; index < this.baseproducts.length; index++) {
                    if (this.baseproducts[index].qtd<0){
                        this.baseproducts[index].qtd=0
                    }
                    this.total+=this.baseproducts[index].qtd*this.baseproducts[index].preco;
                }
            }
        }
    }
</script>

<style>
        .container{
            display: flex;
            flex-direction: row;
            justify-content: flex-start;

        }
        .produto{
            background-color: #ffffff71;
            border-radius:10px;
            border-color: black;
        }
        .kart{
            background-color: #00eeff71;
            width: 300px;
            height: 300%;
            border-radius: 20px;
            padding: 20px;
        }
        .wrapperr {
            width: 705px;
            margin: 20px auto;
            padding: 20px;
        }
        h6 {
            display: inline-block;
            color: rgb(49, 49, 49);
            font-size: 20px;
            font-weight: normal;
            text-transform: uppercase;
            padding: 4px 20px;
            border-radius: 20px;
        }
        .clear {
            clear: both;
        }
        .items {
            display: block;
            margin: 20px 0;
        }
        .item {
            background-color: rgba(0, 176, 207, 0.267);
            float: left;
            margin: 0 10px 10px 0;
            width: 205px;
            padding: 10px;
            height: auto;
            border-radius: 20px;
        }
        .item img {
            display: block;
            margin: auto;
            border-radius: 20px;
            height: 190px;
            width: 190px
        }
        h2 {
            font-size: 16px;
            display: block;
            border-bottom: 1px solid #ccc;
            margin: 0 0 10px 0;
            padding: 0 0 5px 0;
        }
        button {
            border: 1px solid #00eeff;
            padding: 4px 14px;
            background-color: #fff;
            color: #1b4b72;
            text-transform: uppercase;
            margin: 5px;
            font-weight: bold;
            cursor: pointer;
            border-radius: 20px;
        }
        span {
            float: right;
        }
        .shopping-cart {
            display: inline-block;
            background: url('http://cdn1.iconfinder.com/data/icons/jigsoar-icons/24/_cart.png') no-repeat 0 0;
            width: 24px;
            height: 24px;
            margin: 0 0 0 0;
        }
        .input{
            border-radius: 10px;
            font-size:14pt;
            padding:5px;
            width:100px;
            background-color:rgb(182, 253, 255);
            color:rgb(48, 0, 126)
        }
    </style>