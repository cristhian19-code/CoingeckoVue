<template>
    <tr>
        <td>{{coin.name}}</td>
        <td>{{coin.symbol}}</td>
        <td>
            <img :src="coin.image" alt="">
        </td>
        <td>${{coin.current_price}}</td>
        <td :class="coin.price_change_percentage_24h > 0?'uk-text-success' : 'uk-text-danger'">{{coin.price_change_percentage_24h}}%</td>
        <td>${{coin.total_volume.toLocaleString()}}</td>

    </tr>
</template>

<script>
export default {
    props: {
        coin: {
            type: Object
        }
    }
}
</script>

<style>
img{
    height: 30px;
    width: 30px;
}
</style>