<template>
  <div class="tab-control">
    <div v-for="(item, index) in titles" :key="item" class="tab-control-item"
     :class="{active: index === currentIndex}" 
     @click="itemclick(index)">
        <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
export default {

    props: {
        titles: {
            type: Array,
            default() {
                return []
            }
        }
    },
      data() {
            return {
                currentIndex: 0
            }

        },
        methods: {
            itemclick(index) {
                this.currentIndex = index;
                this.$emit('tabClick',index)
            }           
        }
}
</script>

<style scoped>
    .tab-control {
        display: flex;
        text-align: center;
        font-size: 16px;
        height: 40px;
        line-height: 40px;
        background-color: #fff;
    }
    .tab-control-item {
        flex: 1;   
    }
    .tab-control-item span {
        padding: 5px;
    }
    .active {
        color: var(--color-high-text);
        
    }
    .active span {
        border-bottom: 2px solid var(--color-tint);
    }
</style>