<template>
  <div>
    <div class="navbar">
      <el-button @click="logout" type="primary">登出</el-button>
    </div>

    <el-card class="app-container">
      <el-row style="margin:5px">
        <el-collapse v-model="activeNames" @change="handleChange">
          <el-collapse-item name="1">
            <template slot="title">
              <h2>
                查詢條件
                <i class="el-icon-search"></i>
              </h2>
            </template>
            <el-col :xs="7" :sm="3" :md="3" :lg="3">
              只查下料單號
            </el-col>
            <el-col
              style="padding-left:2.5px;padding-right:2.5px;"
              :xs="17"
              :sm="9"
              :md="9"
              :lg="9"
            >
              <el-checkbox v-model="father"></el-checkbox>
            </el-col>
            <el-col :xs="7" :sm="3" :md="3" :lg="3">
              <el-checkbox v-model="c_sname" label="客戶簡稱"></el-checkbox>
            </el-col>
            <el-col
              style="padding-left:2.5px;padding-right:2.5px;"
              :xs="17"
              :sm="9"
              :md="9"
              :lg="9"
            >
              <el-input
                v-model="csname"
                style="width:80%"
                placeholder="客戶簡稱"
              >
              </el-input>
            </el-col>
            <el-col :xs="7" :sm="3" :md="3" :lg="3">
              <el-checkbox
                v-model="o_custbillcode"
                label="客戶單號"
              ></el-checkbox>
            </el-col>
            <el-col
              style="padding-left:2.5px;padding-right:2.5px;"
              :xs="17"
              :sm="9"
              :md="9"
              :lg="9"
            >
              <el-input
                v-model="ocode"
                style="width:80%"
                placeholder="客戶單號"
              >
              </el-input>
            </el-col>
            <el-col :xs="7" :sm="3" :md="3" :lg="3">
              <el-checkbox v-model="o_date" label="訂單日期"></el-checkbox>
            </el-col>
            <el-col
              style="padding-left:2.5px; padding-right:2.5px; "
              :xs="17"
              :sm="9"
              :md="9"
              :lg="9"
            >
              <el-date-picker
                type="date"
                placeholder="選擇日期"
                v-model="odate1"
                value-format="yyyy-MM-dd"
              ></el-date-picker>
              ~
              <el-date-picker
                type="date"
                placeholder="選擇日期"
                v-model="odate2"
                value-format="yyyy-MM-dd"
              ></el-date-picker>
            </el-col>
            <el-col :xs="7" :sm="3" :md="3" :lg="3">
              <el-checkbox
                v-model="o_targetdate"
                label="預交日期"
              ></el-checkbox>
            </el-col>
            <el-col
              style="padding-left:2.5px; padding-right:2.5px;"
              :xs="17"
              :sm="9"
              :md="9"
              :lg="9"
            >
              <el-date-picker
                type="date"
                placeholder="選擇日期"
                v-model="otargetdate1"
                value-format="yyyy-MM-dd"
              ></el-date-picker>
              ~
              <el-date-picker
                type="date"
                placeholder="選擇日期"
                v-model="otargetdate2"
                value-format="yyyy-MM-dd"
              ></el-date-picker>
            </el-col>
            <el-col :xs="7" :sm="3" :md="3" :lg="3">
              <el-checkbox v-model="o_prodno" label="產品編號"></el-checkbox>
            </el-col>
            <el-col
              style="padding-left:2.5px;padding-right:2.5px;"
              :xs="17"
              :sm="9"
              :md="9"
              :lg="9"
            >
              <el-input v-model="pno" style="width:80%" placeholder="產品編號">
              </el-input>
            </el-col>
            <el-col :xs="7" :sm="3" :md="3" :lg="3">
              <el-checkbox v-model="p_name" label="產品名稱"></el-checkbox>
            </el-col>
            <el-col
              style="padding-left:2.5px;padding-right:2.5px;"
              :xs="17"
              :sm="9"
              :md="9"
              :lg="9"
            >
              <el-input
                v-model="pname"
                style="width:80%"
                placeholder="產品名稱"
              >
              </el-input>
            </el-col>

            <el-col :xs="24" :sm="2" :md="2" :lg="12">
              <el-button type="danger" icon="el-icon-close">
                清除
              </el-button>
              <el-button @click="search" type="primary" icon="el-icon-search">
                查詢
              </el-button>
            </el-col>
          </el-collapse-item>
        </el-collapse>
      </el-row>
    </el-card>

    <datatable
      v-bind:fatherChecked="father"
      v-bind:c_sname="c_sname"
      v-bind:o_custbillcode="o_custbillcode"
      v-bind:o_date="o_date"
      v-bind:o_targetdate="o_targetdate"
      v-bind:o_prodno="o_prodno"
      v-bind:p_name="p_name"
    ></datatable>
    
  </div>
</template>

<script>
import datatable from "../components/Table.vue";
export default {
  name: "Home",
  components: { datatable },
  data() {
    return {
      activeNames: ["1"],


      father: true,
      c_sname: true,
      o_custbillcode: true,
      o_date: false,
      o_targetdate: false,
      o_prodno: true,
      p_name: false,
      pname: "",
      pno: "",
      csname: "",
      ocode: "",
      odate1: "",
      odate2: "",
      otargetdate1: "",
      otargetdate2: ""
    };
  },
  methods: {
    handleChange(val) {
      console.log(val);
    },
    search() {
      this.activeNames = [];
      var data = {
        csname: this.csname,
        pname: this.pname,
        pno: this.pno,
        ocode: this.ocode,
        odate1: this.odate1,
        odate2: this.odate2,
        otargetdate1: this.otargetdate1,
        otargetdate2: this.otargetdate2
      };
      console.log(data);
      this.$axios({
        url: "/fatherwhere",
        method: "post",
        data: data
        // headers: {
        //   Authorization: "Cadtech " + localStorage.getItem("jwt")
        // }
      }).then(res => {
        // console.log(res.data);
        var tableData = [];
        res.data.forEach(element => {
          if (element.o_status != 7) {
            tableData.push({
              p_id: element.p_id,
              b_process: this.convertArray(element.b_process),
              c_sname: element.c_sname,
              o_billqty: element.o_billqty,
              o_custbillcode: element.o_code,
              o_date: this.convertDate(element.o_date),
              o_qty: element.o_qty,
              o_targetdate: this.convertDate(element.o_targetdate),
              p_name: element.p_name,
              o_prodno: element.o_prodno
            });
          }
        });
        this.$store.commit("tableData", tableData);
      });

      this.$axios({
        url: "/childwhere",
        method: "post",
        data: data
        // headers: {
        //   Authorization: "Cadtech " + localStorage.getItem("jwt")
        // }
      }).then(res => {
        var tableDatas = [];

        res.data.forEach(element => {
          if (element.o_status != 7) {
            tableDatas.push({
              p_id: element.p_id,
              mp_over: element.mp_over,
              b_process: element.mp_process,
              c_sname: element.c_sname,
              o_billqty: element.o_billqty,
              o_custbillcode: element.o_code,
              o_date: this.convertDate(element.o_date),
              o_qty: element.o_qty,
              o_targetdate: this.convertDate(element.o_targetdate),
              p_name: element.p_name,
              o_prodno: element.o_prodno
            });
          }
        });

        let obj = {};

        for (let i = 0; i < tableDatas.length; i++) {
          if (typeof obj[tableDatas[i].p_id] === "undefined") {
            obj[tableDatas[i].p_id] = {
              p_id: tableDatas[i].p_id,
              o_custbillcode: tableDatas[i].o_custbillcode,

              o_qty: tableDatas[i].o_qty,
              o_prodno: tableDatas[i].o_prodno,

              o_targetdate: tableDatas[i].o_targetdate,

              o_billqty: tableDatas[i].o_billqty,

              c_sname: tableDatas[i].c_sname,

              b_process_mp_over: [
                {
                  mp_process: tableDatas[i].b_process,
                  mp_over: tableDatas[i].mp_over
                }
              ],

              o_date: tableDatas[i].o_date,

              p_name: tableDatas[i].p_name
            };
          } else {
            obj[tableDatas[i].p_id].b_process_mp_over.push({
              mp_process: tableDatas[i].b_process,
              mp_over: tableDatas[i].mp_over
            });
          }
        }

        let arr = Object.values(obj);
        console.log(arr);

        this.$store.commit("tableDatas", arr);
      });
    },
    convertDate(date) {
      const d = new Date(date);
      return d.toLocaleDateString();
    },
    convertString(eng) {
      var result;
      this.process.forEach(element => {
        if (element.mpNo == eng) {
          result = element.mpName;
          console.log(result);
        }
      });
      return result;
    },
    convertArray(str) {
      return str.split(";");
    },
    convertArray1(str) {
      return str.split(",");
    },
    logout() {
      this.$router.replace({ name: "Login" })
    }
  }
};
</script>
<style scoped>
.form {
  margin-top: 20px;
}
.navbar {
  text-align: right;
}
body {
  height: 100%;
}
html {
  margin: 0;
  padding: 0;
  height: 100%;
}
.header {
  text-align: left;
}
.app-container {
  text-align: left;
  margin-bottom: 10px;
}
.el-col {
  margin-bottom: 20px;
}

@media (min-width: 500px) {
}
/* @media (max-width: 375px) {
  .date > .el-form-item {
    width: 100px;
  }
} */
</style>
