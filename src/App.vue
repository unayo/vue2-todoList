<template>
  <div id="app">
    <div class="outLayer position-relative">
      <div class="todo position-absolute">
        <div class="main">
          <div class="icon position-relative">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
          </div>
          <h1>To Do List {{ text }}</h1>
          <div class="btnGroup">
            <b-link
              @click.prevent=" btn ='all' "
              :class=" btn==='all' ? 'activeBtn' : '' "
              class="btn-outline-light text-decoration-none mr-1" type="submit"
            >
              <i class="bi bi-house-door"></i>
              Home
            </b-link>
            <b-link
              @click.prevent="btn='work'"
              :class=" btn==='work'?'activeBtn':''"
              class="btn-outline-light text-decoration-none mr-1" type="submit"
            >
              <i class="bi bi-briefcase"></i>
              Work
            </b-link>
            <b-link 
              @click.prevent=" btn='important' " 
              :class=" btn ==='important' ? 'activeBtn': '' " 
              class="btn-outline-light text-decoration-none mr-1" type="submit"
            >
              <i class="bi bi-star"></i>
              Important
            </b-link>
            <b-link 
              @click.prevent=" btn ='done' "
              :class=" btn==='done' ? 'activeBtn' : '' "
              class="btn-outline-light text-decoration-none" type="submit"
            >
              <i class="bi bi-check-all"></i>
              Done
            </b-link>
          </div>
        </div>
        <div class="content">
          <!-- 增加內容 -->
          <div class="items">
            <input 
              class="inputTxt" type="text" placeholder="Next Step"
              @keyup.enter="submit" 
              v-model="inputTxt" 
            >
          </div>
          <!-- 顯示內容 -->
          <div class="itemsAdd">
            <div v-for="(item, key) in filterData" :key="key" class="items">
              <div @click="starToggle(item)" 
              >
                <i v-if="item.starIcon" class="bi bi-star"></i>
                <i v-else class="bi bi-star-fill"></i>
              </div>
              <input 
                :id="item.id" 
                type="checkbox" 
                :checked="isChecked" 
                @click="checkToggle(item)"
              >
              <label 
                :for="item.id" 
                :class="{ 'checkTrue': item.isChecked }"
              >
                {{ item.txt }}
              </label>
              <b-link @click.prevent="trash(key)" href="#">
                <i class="bi bi-trash"></i>
              </b-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data(){
    return {
      inputTxt: "",
      data: [],
      btn: 'all'
    }
  },
  methods: {
    // 新增
    submit() {
      this.data.push(
        {
          txt: this.inputTxt,
          id : new Date().getTime(),
          readonly: 'readonly',
          isChecked: false,
          visibility: "all",
          starIcon: true,
        }
      );
      console.log(this.data);
      this.inputTxt = "";
    },
    // 打勾
    checkToggle(item) {
      console.log(item)
      item.isChecked = !item.isChecked;
    },
    // 打星星
    starToggle(item) {
      item.starIcon = !item.starIcon
    },
    // 刪除
    trash(key) {
      this.data.splice(key, 1)
    }
  },
  computed: {
    // 過濾
    filterData() {
      switch ( this.btn ){
        case 'work':
          return this.data;
        case 'important':
          return this.data.filter( item=>{
            item.starIcon === true
          });
        case 'done':
          return this.data.filter( item=>{
            item.isChecked === true
          });
        default:
          return this.data;
      }

    }
  },
  mounted() {
  }
}
</script>



<style>
@import url('https://fonts.googleapis.com/css2?family=Luckiest+Guy&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400&display=swap');
@import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.5.0/font/bootstrap-icons.css");
</style>
<style lang="scss">
@import '~@/assets/style/main.scss';
</style>