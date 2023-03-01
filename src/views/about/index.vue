<script lang="ts">
  import Header from '/@/components/Header/index.vue';
  import {
    reactive,watch
  } from 'vue';
import { Watch } from '@element-plus/icons-vue';
import { tr } from 'element-plus/es/locale';
  export default {
    setup() {
      // 初始化信息
      const msg = ref(1)

      const handleNotice = () => {
        msg.value +=20
      }

      const datav = reactive([{
          num: 73,
          unit: '百万+',
          text: 'GitHub 上的开发人员总数',
          bg: 'bg-1'
        },
        {
          num: 16,
          unit: '百万+',
          text: '2021年新增用户',
          bg: 'bg-2'
        },
        {
          num: 61,
          unit: '百万+',
          text: '创建的新存储库数量',
          bg: 'bg-0'
        },
        {
          num: 170,
          unit: '百万',
          text: '合并拉去请求',
          bg: 'bg-3'
        },
      ]);

      const  count = ref(2)
      const obj = reactive({})
      obj.count = count  

      watch(obj, (newV , oldV) => { 
        console.log(newV.count , oldV.count) 
        if(newV ==7) {
          stop()
          console.log(newV)
        }
      },
      {deep:true})

      const state = reactive({foo:3, bar: 1})

      const fooRef = toRef(state, 'foo')
      

      const handleNotice2 = () => {
        obj.count +=2 
        fooRef.value++
        console.log(state.foo)
        linkState()
      }
      const stop = watchEffect(() => {},{
        flush: 'post'
      })

      if(isRef(count) ){
        console.log('show count',  count)
      }

      const stateRefs = reactive({
        a: 1,
        b: 2
      })
      const stateAsRefs = toRefs(stateRefs)

      const linkState = () => {
        stateRefs.b++
        stateAsRefs.a.value++

        console.log('stateRefs a',stateAsRefs.a.value)
        console.log('stateRefs b',stateRefs.b)
      }

      onMounted(() => {
        stateAsRefs.a
      })

      const plusOne = computed(() => msg.value )
      return {
        obj: obj,
        plusOne: plusOne, 
        datav,
        handleNotice,
        handleNotice2
      }
    }
  }
</script>
<template>
  <Header />
  <section class="container max-w-screen-xl px-6 mx-auto 2xl:px-0">
    <div class="flex flex-wrap">
      {{plusOne}}
      <el-button type="primary" @click="handleNotice">增加</el-button>
    </div>
    <div>{{obj.count}}
      <el-button type="primary" @click="handleNotice2">增加</el-button>

    </div>
    <div>列表</div>
    <ul>
      <li v-for="item in datav" :key="item.num">
        <span>{{item.unit}}</span>
        <span>{{item.text}}</span>
      </li>
        
    </ul>
  </section>
</template>
<style lang="less">
  .bg-0 {
    background-color: #fff;
  }

  .bg-1 {
    background-color: #f9f8ff;
  }

  .bg-2 {
    background-color: rgba(244, 248, 253, 1);
  }

  .bg-3 {
    background-color: rgba(240, 248, 246, 1);
  }

  font {
    color: #ff6900;
  }
</style>