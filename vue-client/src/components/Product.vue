<template>
  <div id="app">
    <div class="panel panel-primary">
      <div class="panel-heading">
        <h3 class="panel-title">Add Product</h3>
      </div>
      <div class="panel-body form-inline">
        <label>
          Id:
          <input type="text" class="form-control" v-model="id">
        </label>
        
        <label>
          Name:
          <input type="text" class="form-control" v-model="name" @keyup.enter="add">
        </label>
        
        <label>Keywords Search:
          <!-- 注意 ：Vue中所有指令，在调用的时候，都以v- 开头-->
          <input type="text" class="form-control" v-model="keywords">
        </label>
        
        <input
          type="button"
          value="Add"
          class="btn btn-primary"
          @click="add"
          style="margin-left:20px"
        >
      </div>
    </div>
    <br>

    <table class="table table-hover table-bordered table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Time</th>
          <th>Operation</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in list" :key="item.car_id">
          <td>{{item.car_id}}</td>
          <td>{{item.car_name }}</td>
          <td>{{item.create_time}}</td>
          <td>
            <a href="#" @click.prevent="del(item.car_id)">Delete</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
export default {
  name: "Product",
  data() {
    return {
      list: [],
      id: "",
      name: "",
      keywords: ""
    };
  },
  mounted() {
    this.getAllList();
  },
  methods: {
    getAllList() {
      // 获取所有品牌列表
      //console.log(this.axios);
      this.axios.get("/api/getinfo").then(res => {
        if (res.data.code == 200) {
          this.list = res.data.list;
        } else {
          alert("FAIL");
        }
      });
    },

    add() {
      this.axios
        .post("/api/addcar", { uid: this.id, name: this.name })
        .then(response => {
          console.log(response.data);
          if (response.data.code == 200) {
            this.getAllList();
            this.name = "";
            this.id = "";
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    del(id) {
      // 根据Id删除数据;
      this.axios.get("/api/deletecar/" + id).then(res => {
        if (res.data.code == 200) {
          this.getAllList();
        } else {
          alert("delete fail");
        }
      });
    }

    // search(keywords) {
    //   var newList = [];
    //   // this.list.forEach(item => {
    //   //     if (item.name.indexOf(keywords) != -1) {
    //   //         newList.push(item)
    //   //     }
    //   // });
    //   // return newList;

    //   // forEach some fliter findIndex 这些都属于数组的新方法，
    //   // 都会对数组的每一项，进行遍历，执行相关的操作
    //   return this.list.filter(item => {
    //     //注意:在ES6中，为字符串提供了一个新方法，叫做 String.prototype.includes("要包含的字符串")
    //     // 如果包含，返回true，反之false
    //     if (item.name.includes(keywords)) {
    //       return item;
    //     }
    //   });
    // }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
