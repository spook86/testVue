c<template>
  <div class="main_box">
    <h1>localStorage 存储</h1>
    <input type="text" ref="iptTxt" @keyup.enter="add">
    <button @click="cleared">清空</button>
    <ul>
      <li v-for=" (list,index) in lists">{{list[index]}} <a href="javascript:;" @click="delStorage(index)"
                                                            class="close">X</a>
      </li>
    </ul>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    data() {
      return {
        lists: []
      };
    },
    created () {
      this.getStorage();
    },
    methods: {
      add () {
        var tmpVal = this.$refs.iptTxt.value;
        var storage = window.localStorage;
        if (storage.length) {
          var index;
          for (let i = 0; i < storage.length; i++) {
            var key = storage.key(i);
//            index = parseInt(key.charAt(key.length - 1));
            index = key;
          }
          index++;
        } else {
          index = 0;
        }
        ;
        if (tmpVal) {
          var obj = {};
          obj[index] = tmpVal;
          this.lists.push(obj);
          this.saveStorage(index, tmpVal);
          this.$refs.iptTxt.value = '';
        }
        ;
      },
      saveStorage(key, listVal) {
        if (!window.localStorage) {
          window.alert('浏览器不支持 storage');
        } else {
          var storage = window.localStorage;
          storage.setItem(key, listVal);
        }
      },
      getStorage() {
        var storage = window.localStorage;
        for (let i = 0; i < storage.length; i++) {
          var obj = {};
          var key = storage.key(i);
          obj[key] = storage.getItem(key);
          this.lists.push(obj);
        };
      },
      delStorage(key) {
        var storage = window.localStorage;
        storage.removeItem(key);
        delete this.lists[key];
        console.log(key, this.lists);
      },
      cleared() {
        var lists = this.lists;
        window.localStorage.clear();
        lists.splice(0);
      }
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">

  .main_box {
    width: 300px;
    margin: 0 auto;
    padding-bottom: 10px;
    border: 1px solid #000;
    h1 {
      font-size: 28px;
      padding: 20px 0;
    }
    p {
      margin: 10px 0;
    }
    input {
      border: 1px solid #000;
      outline: none;
    }
    li {
      padding: 20px 5px;
      text-align: left;
      border-bottom: 1px dotted #ccc;
      .close {
        display: inline-block;
        width: 20px;
        height: 20px;
        float: right;
        text-align: center;
        font-size: 20px;
        line-height: 20px;
        background: #CCC;
      }
    }
  }
</style>
