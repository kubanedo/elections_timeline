<template>
  <div v-if="itemData.registered && itemData.name && itemData.about" class="item-wrapper">
      <div class="item">
        <div class="item__date">{{date}}</div>
        <div v-if="getName" class="item__name">{{getName}}</div>
        <div class="item__about">{{itemData.about.split('. ')[0]}}.</div>
      </div>
  </div>
</template>

<script>
export default {
    props: {
       itemData: {
           type: Object,
           required: true
       } 
    },
    data() {
        return {
            date: this.formatDate(this.itemData.registered)
        }
    },
    computed: {
        getName() {
            return (this.itemData.name.first && this.itemData.name.last) 
            ? this.itemData.name.first + ' ' + this.itemData.name.last 
            : this.itemData.name.first || this.itemData.name.last;
        }
    },
    methods: {
        formatDate(date) {
           const monthNames = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

           let d = new Date(date);
           return d.getDate()  + " " + monthNames[d.getMonth()] + " " + d.getFullYear();
        }
    }
}
</script>

<style lang="scss">
$primary-color: #095184;

.item-wrapper {
   position: relative;

    @media screen and (min-width: 539px) {
        width: 50%;       
    }

    &::before {
        content: '';
        position: absolute;
        width: 3rem;
        height: 5px;
        border-radius: 5px;
        left: 5px;
        top: calc(50% - 5px);
        background: $primary-color;
        z-index: 1;   

         @media screen and (min-width: 539px) {
            width: calc(100% - 13rem);
            right: 0;
            left: unset;
         }         
    }
    &::after {
        content: '';
        position: absolute;
        width: 26px;
        height: 26px;
        top: calc(50% - 16px);
        left: 5px;
        background: white;
        border: 5px solid $primary-color;
        border-radius: 50%;
        z-index: 1;
        box-sizing: border-box;

        @media screen and (min-width: 539px) {
            right: -8px;
            left: unset;
        }         
    }  
    @media screen and (min-width: 539px) {
            &:nth-child(odd) {
                .item {
                    margin-left: 0;
                }
            }   
        
            &:nth-child(even) {
                margin-left: auto;
                .item {
                    margin-left: auto; 
                    &::after {
                        right: 0;
                        left: -26px;
                    } 
                }
                &::after {
                    left: -16px;
                }
                &::before {
                    left: 0;
                }
            }                  
    }
}
.item {
    position: relative;
    font-family: sans-serif;
    text-align: center;
    width: calc(100% - 5rem);
    padding: .625rem;
    margin-left: auto;

    @media screen and (min-width: 539px) {
        width: 11.25rem;  

        &::after {
            content: '';
            position: absolute;
            width: 26px;
            height: 26px;
            top: calc(50% - 16px);
            right: -26px;
            background: $primary-color;
            border: 5px solid $primary-color;
            border-radius: 50%;
            z-index: 1;
            box-sizing: border-box;
        } 
    }
 
    &__date {
        color: $primary-color; 
        font-weight: bold;
        margin-bottom: .625rem;
    }
    &__name, &__about {
        font-size: .875rem;
    }
}
</style>