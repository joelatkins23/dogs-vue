
<template>
    <div class="container-fulid">
        <div class="header text-center">
            <h2>{{name}}</h2>
        </div>
        <div class="sidebar">
            <ul class="nav flex-column">
               <li class="nav-item" v-for="item of dogslist" :key="item.childname+item.name">
                <a class="nav-link " href="#" v-on:click="getimg(item.childname, item.name)" >{{item.childname}} {{item.name}}</a>
            </li>
            </ul>
        </div>
        <div class="content mt-5">
            <div class="container text-center mt-5">
                <div class="img-content">
                    <img class="image-rsponisve" :src="img" alt="" />
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
            return{
                dogslist: [],
                name:'Dogs',
                img:''
            }
        },

        created: function()
        {
            this.fetchItems();
        },

        methods: {
            fetchItems()
            {
              let uri = 'https://dog.ceo/api/breeds/list/all';
              this.axios.get(uri).then((resp) => {
                  const dogs = resp.data.message; 
                    const dogslist=[]; 
                    Object.keys(dogs).map((key) => {
                    const item={};
                    if(dogs[key].length>0){
                        dogs[key].map((catergoty_dogs) => {  
                        const itemchild={};
                        itemchild['name']=key;
                        itemchild['childname']=catergoty_dogs;
                        dogslist.push(itemchild);
                        })
                    }else{
                        item['name']=key;
                        item['childname']='';
                        dogslist.push(item);
                    }
                    } );
                    this.dogslist=dogslist;  
              });
            },
            getimg(childname, name){
                const url=(childname) ? 'https://dog.ceo/api/breed/'+name+'/'+childname+'/images/random': 'https://dog.ceo/api/breed/'+name+'/images/random';
                this.axios.get(url).then((resp) => {
                    this.img= resp.data.message; 
                    this.name=childname+' '+name;
                });
            }
        }
}
</script>
<style>
   
.sidebar {
    margin-top: 50px;
    width: 200px;
    background: #eee;
    min-height: calc(100vh - 50px);
    float: left;
    max-height: calc(100vh - 50px);
    overflow: auto;
}

.header {
    background: red;
    height: 50px;
    width: 100%;
    position: fixed;
}

.nav-item {
    border-bottom: 1px solid #fff;
}

.nav-item a {
    color: #000;
}

.content {
    width: calc(100% - 200px);
    float: left;
}

.img-content img {
    width: 500px;
}

.nav-link:hover {
    color: red;
}

.active {
    color: red;
}

.header h2 {
    margin-left: 10px;
    color: white;
    padding-top: 4px;
    text-transform: uppercase;
}
</style>