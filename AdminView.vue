<template>
    <div class="container">
        <div class="wrapperr">
            <h1>Admin</h1>
            <div class="adm">
                <p><b>Url imagem: </b></p>
                <input type="text" class="inputadmin inputurl" v-model="url">
            </div>
            <div class="adm">
                <p><b>Nome: </b></p>
                <input type="text" class="inputadmin" v-model="nome">
            </div>
            <div class="adm">
                <p><b>Preço: </b></p>
                <input type="number" class="inputadmin" v-model="preco">
            </div>
            <button @click="step1()"><h5>Publicar produto</h5> </button>
            <div v-if="alert==1" class="alert">
                <p>em falta: campo 'URL'</p>
            </div>
            <div v-if="alert==2" class="alert">
                <p>em falta: campo 'Nome'</p>
            </div>
            <div v-if="alert==3" class="alert">
                <p>em falta: campo 'Preco'</p>
            </div>
            <div v-if="alert==4" class="alert">
                <p>'Preco' não pode ser negativo</p>
            </div>       
        </div>
        

        <div v-if="alert==0">
            <ul>
                <ol><h2>Vista previa</h2></ol>
                <ol>
                        <div class="item">
                            <img :src="url" alt="item" class="img"/>
                            <h2>{{nome}}</h2>

                            <p>Price: <em> <b> {{preco}}€ </b> </em></p>
                            <button class="add-to-cart" type="button">Add to cart</button>
                        </div>
                </ol>
                <ol><button @click="guardar()">Confirmar Publicação</button></ol>
            </ul>       
        </div>
    </div>
</template>


<script>
import axios from "axios";
export default {
    data(){
        return{
            url:'',
            nome:'',
            preco:0,
            alert:-1,
        }
    },
    methods:{
        guardar(){
            axios
                //guardar em firebase
                .post("https://proyecto1-3146f-default-rtdb.firebaseio.com/.json",{
                    url:this.url,
                    nome:this.nome,
                    qtd: 0,
                    preco:this.preco
                })
                .then(
                    (response) => ((
                        this.articleID=response.data.id),
                        this.$$router.push("/MarketView")) //encaminhar para a pagina de comentarios
                )
        },
        step1(){
            if (this.url==''){
                this.alert=1;
            }
            else if (this.nome=='')
            {
                this.alert=2;
            }
            else if (this.preco==''){
                this.alert=3;
            }
            else if (this.preco<=0){
                this.alert=4;
            }
            else {
                this.alert=0;
            }
        }
    }
}
</script>


<style>
.adm{
    display: flex;
    justify-content: space-between;
    margin: 20px auto;
}

.inputadmin{
    width: 600px ;
    color: darkslategrey;
    background-color:lightcyan;
    font-size: 15pt;
    border-radius: 20px;
}
.inputurl{
    font-size: 10pt;
}

</style>