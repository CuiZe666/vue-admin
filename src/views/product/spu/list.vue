<template>
  <div>
    <!--上面的卡片-->
    <el-card style="margin-bottom: 20px">
      <!--使用下拉框的公共组件-->
      <CategorySelector ref="cs" @changeCategory="changeCategory" />
    </el-card>
    <el-card>
      <div v-show="!isShowSpuForm && !isShowSkuForm">
        <!--按钮-->
        <el-button type="primary" icon="el-icon-plus" @click="addSpuForm" :disabled="!category3Id">添加SPU</el-button>
        <!--表格-->
        <!--data中存放的是数组-->
        <el-table :data="spuInfoList" style="width: 100%" border>
          <el-table-column type="index" label="序号" width="80px" align="center">
          </el-table-column>
          <!--prop中绑定的是每一行对象中的某个属性-->
          <el-table-column prop="spuName" label="SPU名称"> </el-table-column>
          <el-table-column prop="description" label="SPU描述">
          </el-table-column>
          <el-table-column label="操作">
            <template slot-scope="{ row, $index }">
              <HintButton title="添加SKU" type="primary" icon="el-icon-plus" size="mini" @click="showAddSku(row)" />
              <HintButton title="修改SPU" type="primary" icon="el-icon-edit" size="mini" @click="showUpdateSpuForm(row.id)" />
              <HintButton title="查看SKU" type="info" icon="el-icon-info" size="mini" @click="showSkuList(row.id)" />

              <el-popconfirm :title="`确定要删除 ${row.spuName} 吗`" @onConfirm="deleteSpu(row.id)">
                <HintButton title="删除" type="danger" icon="el-icon-delete" size="mini" slot="reference" />
              </el-popconfirm>
            </template>
          </el-table-column>
        </el-table>
        <!--分页-->
        <el-pagination @size-change="handleSizeChange" @current-change="getSpuInfoList" :current-page="page" :page-sizes="[3, 6, 9, 12]" :page-size="limit" layout=" prev, pager, next, jumper,->,sizes,total" :total="total" style="margin-top: 20px; text-align: center" background>
        </el-pagination>
      </div>
      <!--添加或者修改Spu的组件-->
      <SpuForm :visible.sync="isShowSpuForm" ref="spuForm" @saveSuccess="saveSuccess" />
      <SkuForm v-show="isShowSkuForm" @cancel="cancel" ref="skuForm" @saveSuccess="isShowSkuForm = false" />
    </el-card>
    <!--对话框代码-->
    <el-dialog title="SKU列表" :visible.sync="dialogTableVisible">
      <el-table :data="skuInfoList">
        <el-table-column property="skuName" label="名字" width="150"></el-table-column>
        <el-table-column property="price" label="价格(元)" width="200"></el-table-column>
        <el-table-column property="weight" label="重量(千克)"></el-table-column>
        <el-table-column property="skuDefaultImg" label="默认图片">
          <template slot-scope="{ row }">
            <img :src="row.skuDefaultImg" alt="" width="100" height="100" />
          </template>
        </el-table-column>
      </el-table>
    </el-dialog>
  </div>
</template>
<script>
// 引入SpuForm组件
import SpuForm from '../components/SpuForm'
import SkuForm from '../components/SkuForm'
export default {
  name: 'SpuList',
  // 注册组件
  components: {
    SpuForm,
    SkuForm,
  },
  data() {
    return {
      category1Id: '', // 一级分类的id
      category2Id: '', // 二级分类的id
      category3Id: '', // 三级分类的id
      page: 1, // 默认页码1
      limit: 3, // 默认每页的条数
      total: 0, // 总条数
      spuInfoList: [], // spu的列表数据
      isShowSpuForm: false, // 默认是不显示添加或修改Spu的界面的
      spuId: '', // spu的id
      isShowSkuForm: false, // 默认是不显示添加Sku界面的
      skuInfoList: [], // 用来存放sku列表数据
      dialogTableVisible: false, // 不显示对话框
    }
  },
  watch: {
    isShowSpuForm(val) {
      this.$refs.cs.isDisabled = val
    },
  },
  methods: {
    // 获取spu信息数据
    async getSpuInfoList(page = 1) {
      this.page = page
      const { limit, category3Id } = this
      // 异步请求
      const result = await this.$API.spu.getSpuInfoList(page, limit, category3Id)
      if (result.code === 200) {
        // 更新数据
        // console.log(result)
        const { records, total } = result.data
        // 更新数据
        this.total = total
        this.spuInfoList = records
      }
    },
    // 自定义事件
    changeCategory({ categoryId, level }) {
      // 判断级别
      if (level === 1) {
        this.category1Id = categoryId
        // 重置数据
        this.category2Id = null
        this.category3Id = null
        this.total = 0
        this.spuInfoList = []
      } else if (level === 2) {
        this.category2Id = categoryId
        // 重置数据
        this.category3Id = null
        this.total = 0
        this.spuInfoList = []
      } else if (level === 3) {
        this.category3Id = categoryId
        // 获取当前的spu信息数据
        this.getSpuInfoList()
      }
    },
    // 分页
    handleSizeChange(val) {
      // 更新每页条数的数据
      this.limit = val
      // 重新获取数据
      this.getSpuInfoList()
    },

    // 点击修改SPU按钮,显示修改SpuForm界面
    showUpdateSpuForm(spuId) {
      this.spuId = spuId
      // 调用SpuForm组件中的初始化数据方法
      this.$refs.spuForm.initUpdateData(spuId)
      // 显示修改的界面
      this.isShowSpuForm = true
    },
    // 保存成功
    saveSuccess() {
      if (this.spuId) {
        // 修改
        // 重新获取数据
        this.getSpuInfoList(this.page)
        this.spuId = ''
      } else {
        // 添加
        this.getSpuInfoList()
      }
    },

    // 删除spu操作
    async deleteSpu(spuId) {
      const result = await this.$API.spu.deleteSpuById(spuId)
      if (result.code === 200) {
        this.$message.success('删除SPU成功')
        // 重新获取数据
        this.getSpuInfoList()
      } else {
        this.$message.error('删除SPU失败')
      }
    },

    // 点击添加SPU按钮,显示SpuForm界面
    addSpuForm() {
      // 获取当前的三级分类的id
      const category3Id = this.category3Id
      // 三级分类的id需要在SpuForm组件中使用,并且需要传递过去
      this.$refs.spuForm.initAddData(category3Id)
      this.isShowSpuForm = true
    },

    // 点击添加Sku按钮,实现添加Sku操作的
    showAddSku(spuInfo) {
      // 把spuInfo对象解构出来,同时把一级/二级分类的id存放在新的spuInfo对象中
      spuInfo = {
        ...spuInfo,
        category1Id: this.category1Id,
        category2Id: this.category2Id,
      }
      // 显示SkuForm组件界面
      this.isShowSkuForm = true
      // 显示SkuForm组件界面的时候,的初始化数据方法
      this.$refs.skuForm.initAddData(spuInfo)
    },
    // 在SkuForm 组件中点击了取消按钮之后,要执行的回调函数
    cancel() {
      this.isShowSkuForm = false
    },

    // 点击查看SKU按钮,显示sku列表数据
    async showSkuList(spuId) {
      const result = await this.$API.sku.getSkuInfoListBySpuId(spuId)
      this.skuInfoList = result.data
      this.dialogTableVisible = true
    },
    // 哈哈哈
  },
}
</script>
<style>
</style>