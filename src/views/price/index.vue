<template>
  <div>
    <el-row :gutter="30" style="margin:10px">
      <el-col :span="6">
        <el-form ref="searchData" :model="searchData" label-width="80px">
          <el-form-item label="名称：">
            <el-input v-model="searchData.name" placeholder="请输入名称" />
          </el-form-item>
        </el-form>
      </el-col>
      <el-col :span="3">
        <el-button type="primary" @click="search">搜索</el-button>
      </el-col>
      <el-col :span="4">
        <el-button type="success" @click="addVisible = true">新增</el-button>
      </el-col>
    </el-row>
    <el-table :data="tableData" :cell-class-name="cell" border style="width: 100%">
      <el-table-column label="Id" align="center">
        <template slot-scope="scope">
          <el-input v-model="scope.row.Id" placeholder="请输入内容" />
        </template>
      </el-table-column>
      <el-table-column prop="Name" label="名称" align="center" />
      <el-table-column prop="Price" label="价格" align="center" />
      <el-table-column prop="PriceType" label="计算单位" align="center" />
      <el-table-column prop="ShippingType" label="ShippingType" align="center" />
      <el-table-column prop="UseFlag" label="是否生效" align="center" />
      <el-table-column prop="LastModifyOn" label="最后更新时间" align="center" />
      <el-table-column prop="LastModifyBy" label="最后更新人" align="center" />
      <el-table-column label="操作" width="180" align="center">
        <template slot-scope="scope">
          <el-button size="mini" type="primary" @click="handleView(scope.$index, scope.row)">查看</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-row :gutter="0" style="margin:30px">
      <el-col :span="10" :push="psize">
        <el-pagination :current-page="pageIndex" :page-sizes="[5, 10, 20, 30]" :page-size="pageSize" :total="total" layout="total, sizes, prev, pager, next, jumper" @size-change="handleSizeChange" @current-change="handleCurrentChange" />
      </el-col>
    </el-row>
    <el-dialog :visible.sync="dialogVisible" title="查看详情" width="40%">
      <span>{{ name }}</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
    <el-dialog :visible.sync="addVisible" title="新增报价" width="50%">
      <el-form ref="addFormData" :model="addFormData" :rules="rules" label-width="100px" class="demo-ruleForm" style="padding:0px 50px">
        <el-form-item label="活动名称" prop="name">
          <el-input v-model="addFormData.name" />
        </el-form-item>
        <el-form-item label="价格" prop="price">
          <el-input v-model="addFormData.price" />
        </el-form-item>
        <el-form-item label="计算单位" prop="PriceType">
          <el-input v-model="addFormData.PriceType" />
        </el-form-item>
        <el-form-item label="ShippingType" prop="ShippingType">
          <el-input v-model="addFormData.ShippingType" />
        </el-form-item>
        <el-form-item label="是否生效" prop="ShippingType">
          <el-radio-group v-model="addFormData.UseFlag">
            <el-radio :label="1">是</el-radio>
            <el-radio :label="0">否</el-radio>
          </el-radio-group>
        </el-form-item>

      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addVisible = false">取 消</el-button>
        <el-button type="primary" @click="addData">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { getList } from '@/api/table'
export default {
  data() {
    return {
      text: '名称',
      psize: 14,
      tableData: [
        {
          id: ''

        }
      ],
      searchData: {
        name: ''
      },
      dialogVisible: false,
      addVisible: false,
      loading: true,
      name: '',
      pageIndex: 1,
      pageSize: 10,
      total: 30,
      addFormData: {
        name: '',
        price: '',
        PriceType: '',
        ShippingType: '',
        UseFlag: 1
      },
      rules: {
        name: [
          { required: true, message: '请输入名称', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  mounted() {
    this.getDataList()
  },
  methods: {
    cell({ row, column, rowIndex, columnIndex }) {
      if (row) {
        return 'center'
      }
    },
    cen({ row, column, rowIndex, columnIndex }) {
      if (row) {
        return 'center'
      }
    },
    getDataList() {
      const params = {
        name: this.searchData.name,
        page: this.pageIndex,
        page_size: this.pageSize
      }
      getList(params).then(res => {
        if (res.status === 200) {
          this.loading = false
          this.tableData = res.data
        } else {
          this.loading = false
        }
      })
    },
    addData() {
      this.addVisible = false
      console.log(this.addFormData)
    },
    handleView(a, b) {
      console.log(b)
      this.dialogVisible = true
      this.name = b.Name
    },
    handleDelete(index) {
      this.$confirm('此操作将永久删除该项, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.tableData.splice(index, 1)
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    search() {
      this.getDataList()
    },
    handleSizeChange(pagesize) {
      this.pageSize = pagesize
      this.getDataList()
    },
    handleCurrentChange(current) {
      this.pageIndex = current
      this.getDataList()
    }
  }
}
</script>
<style>
.center {
  text-align: center;
}
</style>

