<template>
  
  <div>
    <!-- 父階 -->
    <div v-if="fatherChecked">
      <el-table
      
        :data="pagedTableDataFather"
        stripe
        style="width: 100% ;"
        border
        :row-style="{ height: '80px' }"
        :header-cell-style="{
        'background-color': '#4c5870',
        'color': 'white',
        'font-weight': '200',
        'border-radius':'4px' }

        "
        
      >
        <el-table-column type="index" :index="indexMethod" label="序">
        </el-table-column>

        <el-table-column
          v-if="c_sname"
          align="center"
          prop="c_sname"
          label="客戶簡稱"
          width="80"
        >
        </el-table-column>
        <el-table-column
          v-if="o_custbillcode"
          align="center"
          prop="o_custbillcode"
          label="客戶單號"
          width="110"
        >
        </el-table-column>
        <el-table-column
          v-if="o_date"
          align="center"
          prop="o_date"
          label="訂單日期"
          width="100"
        >
        </el-table-column>
        <el-table-column
          v-if="o_targetdate"
          align="center"
          prop="o_targetdate"
          label="預交日期"
          width="100"
        >
        </el-table-column>
        <el-table-column
          v-if="o_prodno"
          align="center"
          prop="o_prodno"
          label="產品編號"
          width="200"
        >
        </el-table-column>
        <el-table-column
          v-if="p_name"
          align="center"
          prop="p_name"
          label="產品名稱"
          width="200"
        >
        </el-table-column>
        <el-table-column align="center" prop="o_qty" label="訂單數" width="80">
        </el-table-column>
        <!-- <el-table-column
          align="center"
          prop="o_billqty"
          label="出貨數"
          width="80"
        >
        </el-table-column> -->
        <el-table-column prop="b_process" label="製程進度" fixed="right">
          <template slot-scope="scope">
            <!-- {{scope.row}} -->
            <el-badge
              v-for="item in scope.row.b_process"
              :key="item"
              :value="scope.row.o_qty"
              class="item"
            >
              <el-tag type="info" effect="dark">{{ convertString(item) }}{{ item }}</el-tag>
            </el-badge>
          </template>
        </el-table-column>
      </el-table>
      <div v-if="fatherChecked">
        <el-pagination
          layout="sizes,prev, pager, next"
          :total="$store.state.single.length"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :page-sizes="pageSizes"
          :page-size="pageSize"
        >
        </el-pagination>
      </div>
    </div>

    <!-- 子階 -->
    <div v-else>
      <el-table
        :data="pagedTableDataChild"
        stripe
        style="width: 100% "
        border
        
        :row-style="{ height: '80px' }"
        :header-cell-style="{
        'background-color': '#4c5870',
        'color': 'white',
        'font-weight': '200',
        'border-radius':'4px' }

        "
      >
        <el-table-column type="index" :index="indexMethod" label="序">
        </el-table-column>

        <el-table-column
          v-if="c_sname"
          align="center"
          prop="c_sname"
          label="客戶簡稱"
          width="80"
        >
        </el-table-column>
        <el-table-column
          v-if="o_custbillcode"
          align="center"
          prop="o_custbillcode"
          label="客戶單號"
          width="110"
        >
        </el-table-column>
        <el-table-column
          v-if="o_date"
          align="center"
          prop="o_date"
          label="訂單日期"
          width="100"
        >
        </el-table-column>
        <el-table-column
          v-if="o_targetdate"
          align="center"
          prop="o_targetdate"
          label="預交日期"
          width="100"
        >
        </el-table-column>
        <el-table-column
          v-if="o_prodno"
          align="center"
          prop="o_prodno"
          label="產品編號"
          width="200"
        >
        </el-table-column>
        <el-table-column
          v-if="p_name"
          align="center"
          prop="p_name"
          label="產品名稱"
          width="200"
        >
        </el-table-column>
        <el-table-column align="center" prop="o_qty" label="訂單數" width="80">
        </el-table-column>
        <!-- <el-table-column
          align="center"
          prop="o_billqty"
          label="出貨數"
          width="80"
        >
        </el-table-column> -->
        <el-table-column prop="b_process" label="製成進度"  >
          <template slot-scope="scope">
            <!-- {{ scope.row }} -->
            <el-badge
              v-for="(item, index) in scope.row.b_process_mp_over"
              :key="index"
              :value="scope.row.o_qty"
              class="item"
            >
              <el-tag v-if="item.mp_over" type="success"
              effect="dark"
                >{{ convertString(item.mp_process) }}
                {{ item.mp_process }}</el-tag
              >
              <el-tag v-else type="info"
              effect="dark"
                >{{ convertString(item.mp_process) }}
                {{ item.mp_process }}</el-tag
              >
            </el-badge>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 分頁 -->
    <div v-if="!fatherChecked">
      <el-pagination
        layout="sizes,prev, pager, next"
        :total="$store.state.lists.length"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-sizes="pageSizes"
        :page-size="pageSize"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    "fatherChecked",
    "c_sname",
    "o_custbillcode",
    "o_date",
    "o_targetdate",
    "o_prodno",
    "p_name"
  ],
  created() {
    this.getProcess();
  },
  data() {
    return {
      tableData: [],
      tableData1: [],
      process: [
        { mpNo: "false", mpName: "未完工" },
        { mpNo: "true", mpName: "完  工" },
        { mpNo: null, mpName: "無" },
        { mpNo: "", mpName: "無" },
      ],
      afterprocess: [],
      active: 0,

      page: 1,
      pageSize: 5,
      pageSizes: [1, 3, 10]
    };
  },
  methods: {
    setPage(val) {
      this.page = val;
    },
    handleSizeChange(val) {
      console.log(`每頁 ${val} 條`);
      this.pageSize = val;
    },
    handleCurrentChange(val) {
      console.log(`當前頁: ${val}`);
      this.page = val;
    },
    getProcess() {
      this.$axios({
        url: "/makeprocess",
        method: "get"
        // headers: {
        //   Authorization: "Cadtech " + localStorage.getItem("jwt")
        // }
      })
        .then(res => {
          res.data.forEach(element => {
            this.process.push({
              mpNo: element.mpNo,
              mpName: element.mpName
            });
          });
          var test = this.process.map(function(obj) {
            var rObj = {};
            rObj[obj.mpNo] = obj.mpName;
            return rObj;
          });
          this.afterprocess = test;
        })
        .catch(error => {
          error;
          // console.log(error);
        });
    },

    convertDate(date) {
      const d = new Date(date);
      return d.toLocaleDateString();
    },
    indexMethod(index) {
      return index + 1 * 1;
    },

    convertString(eng) {
      var result;
      this.process.forEach(element => {
        if (element.mpNo == eng) {
          result = element.mpName;
          // console.log(result);
        }
      });
      return result;
    },

    convertArray(str) {
      return str.toString().split(",");
    },
    convertTrueOrFalse(str) {
      // console.log(str);
      if (str == true.toString()) {
        return true;
      } else {
        return false;
      }
    }
  },
  computed: {
    pagedTableDataFather() {
      return this.$store.state.single.slice(
        this.pageSize * this.page - this.pageSize,
        this.pageSize * this.page
      );
    },
    pagedTableDataChild() {
      return this.$store.state.lists.slice(
        this.pageSize * this.page - this.pageSize,
        this.pageSize * this.page
      );
    }
  }
};
</script>
<style scoped>

.el-badge {
  margin: 20px 10px;
}
.el-tag {
  margin: 0 0px;
}
</style>
