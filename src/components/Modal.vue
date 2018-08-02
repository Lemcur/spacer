<template>
    <div>
    <div class="modalWrapper" @click="$emit('closeModal')"></div>
        <div class="outerWrapper" >
            <div class="innerWrapper">
                <div class="photo">
                    <img :src="photo" />
                </div>
                <div class="description">
                    <h2 class="title"> {{ title }}</h2>
                    <p class="description"> 
                        {{ description }} 
                        <a
                        class="readMore"
                        v-if="readMore === true" 
                        @click="fullDescription"
                        > Read More... 
                        </a>
                    </p>
                </div>
            </div>
            <div class="close" @click="$emit('closeModal')" />
        </div>
    </div>
</template>

<script>
export default {
    name: 'Modal',
    props:{
        item:{
            type: Object,
            required: true,
        },
    },
    data() {
        return{
            photo: null,
            title: null,
            description: null,
            readMore: false,
        };
    },
    mounted() {
        this.photo = this.item.links[0].href;
        this.title = this.item.data[0].title;
        this.description = this.longDescription(this.item.data[0].description);
    },
    methods: {
        longDescription: function(desc){
            if(desc.length > 200){
                this.readMore = true;
                console.log(desc);
                return desc.substring(0, 200);
            }
            return desc;
        },
        fullDescription: function(desc){
            this.description = this.item.data[0].description;
            this.readMore = false;
        }
    },
}

</script>

<style lang="scss" scoped>

    .modalWrapper {
        z-index:9;
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: rgba( 0,0,0, .6);
    }

    .outerWrapper{
        z-index: 10;
        background: #f6f6f6;
        max-width: 100%;
        height: 100%;
        position: fixed;
        top: 0;
        left: 0;

        @media (min-width: 768px){
            width: 80%;
            height: 80%;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            margin: auto;
            box-shadow: 0 30px 30px -10px rgba( 0,0,0, .3);
        }
        @media (min-width: 1024px){
            width: 70%;
            height: 60%;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            margin: auto;
            box-shadow: 0 30px 30px -10px rgba( 0,0,0, .3);
        }

        @media (min-width: 1440px){
            width: 50%;
            height: 60%;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            margin: auto;
            box-shadow: 0 30px 30px -10px rgba( 0,0,0, .3);
        }
    }

    .close{
        position: absolute;
        right: 0;
        top: 0;
        width: 30px;
        height: 30px;
        padding: 30px;
        cursor: pointer;
        
        &::before,
        &::after{
            position: absolute;
            top: 30px;
            right: 20px;
            content: '';
            width: 10px;
            height: 2px;
            background: black;
            display: block;
        }

        &::before{
            transform: rotate(45deg);
        }
        
        &::after{
            transform: rotate(-45deg);
        }
    }

    .innerWrapper{
        display: flex;
        height: 100%;
        padding: 50px;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        @media (min-width: 768px){
            flex-direction: row;

            .photo{
                margin-right: 20px;

                img {
                    max-height:70vh;
                }
            }
        }

        @media (min-width: 1024px){
            flex-direction: row;
            margin: 0 auto;

            .photo{
                margin-right: 20px;
                    width: fit-content;
                    max-width: 500px;
                img{

                    max-height:50vh ;
                }
            }
        }

        .photo {
            width: 100%;
            height: auto;
            background: black;

            
            img{
                width: 100%;
            }
        }
    }

    .description, .title {
        overflow-wrap: break-word;
        max-width:300px;
        max-height: 100%;
        overflow-y: auto;
        // margin: 10px 20px;
        // // @media (min-width: 1024px){
        // //     width:80%;
        // // }
    }
    .readMore {
        color:blue;
    }
</style>
