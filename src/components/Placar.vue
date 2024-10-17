<template>
    <div>
            Player {{ winCount }} X {{loseCount}} Computador
    </div>
</template>
<script>

    export default{
        name:'AppPlaca',
        props:{
            winCount:{ type: Number , required:true},
            loseCount:{ type: Number , required:true}
        }
    }
</script>