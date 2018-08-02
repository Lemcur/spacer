<template>
    <div class="itemWrapper">
        <div :style="style" class="item" />
        <h3> {{ title }} </h3>
    </div>
</template>

<script>
export default{
    name: 'Item',
    props:{
        item: {
            type: Object,
            required: true,
        },
    },
    data() {
        return{
            photo: this.item.links[0].href,
            title: this.item.data[0].title,
        };
    },
    computed: {
        style() {
            return `background-image: url("${this.photo}")`
        }
    }
};
</script>

<style lang="scss" scoped>

    .item{
        width:80vw;
        height: 300px;
        margin: 0 auto;
        background-repeat: no-repeat;
        background-size:cover;
        @media (min-width:768px){
            width: 25vw;
        }
    }

    .itemWrapper {
        text-align:center;
        margin-bottom: -20px;
    }

</style>
