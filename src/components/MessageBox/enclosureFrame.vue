<template>
    <section id="enclosureFrame">
      <div class="box bounce-enter-active">
        <!-- <ElectronicFence :ElectronicProp="ElectronicDataS"> -->
        <ElectronicFence >
            <span slot="iconCha" class="iconClose iconfont icon-iconfontguanbi" @click="closeEnclosureFrame"></span>
        </ElectronicFence>
      </div>
    </section>
</template>

<script>
  import ElectronicFence from '@/components/mapTool/ElectronicFence2.vue' // 左侧电子围栏
  export default {
    name: '',
    components: {
      ElectronicFence
    },
    // props: ['frameProp'],
    // data() {
    // return {
    // ElectronicDataS:this.frameProp
    // }
    // },
    mounted () {

    },
    methods: {
      // 关闭电子围栏
      closeEnclosureFrame () {
        this.$store.commit('carBoxOpenOrClose', {$closeClass: 'dzwk'});
      }
    },
    filters: {},
    watch: {}

  }
</script>

<style scoped>

</style>
