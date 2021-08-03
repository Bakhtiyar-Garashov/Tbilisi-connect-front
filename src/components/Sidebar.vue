<template>
    <div id="sidebar">
        <div id="logo">
            <img src="../assets/logo.svg" alt="" srcset="">
        </div>
        <div id="about">
            <a href="/">About</a>
        </div>
    </div>
</template>

<script>
export default {
    components:{
    },
    data(){
        return {

        }
    },
    computed:{

    },
    methods:{

    }
}
</script>

<style scoped>
#sidebar{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 500px;
    height: 139px;
    background-color: white;
    position: absolute;
    top:0%;
    right: 0%;
}
#logo img {
    margin-top: 16px;
    margin-left: 20px;
}

#about {
    margin-top: 16px;
    margin-right: 12px;
}

#about a {
    text-decoration: none;
    font-size: 12px;
    font-weight: 400;
    line-height: 18px;
    color: #A5A5A5;
}
</style>
