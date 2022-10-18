<template>
<div class="p-1">
    <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3">
        <div class="col" v-for="(sn, key) in this.resp" :key="key">
            <div class="card rounded shadow mb-4">
                <div class="card-body" :style="{background: sn.color}">
                    <h5 class="card-title">Subnet {{sn.id}}</h5>
                    <h6 class="card-subtitle mb-2 text-muted"><strong>R: </strong>{{sn.subnet}}<br></h6>
                    <p class="card-text">
                        <strong>P: </strong>{{sn.first}}<br>
                        <strong>U: </strong>{{sn.last}}<br>
                        <strong>G: </strong>{{sn.gateway}}<br>
                        <strong>B: </strong>{{sn.broadcast}}<br>
                    </p>
                    <!-- 
                <a href="#" class="card-link">Card link</a>
                <a href="#" class="card-link">Another link</a>
                -->
                </div>
            </div>
        </div>
    </div>

</div>
</template>

<script>
export default {
    data() {

    },
    props: {
        resp: []
    }
}
</script>
