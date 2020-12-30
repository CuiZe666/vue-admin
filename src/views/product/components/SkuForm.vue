<template>
  <el-form label-width="120px">
    <!--SPU名称-->
    <el-form-item label="SPU 名称">
      <span>{{spuInfo.spuName}}</span>
    </el-form-item>
    <!--SKU名称-->
    <el-form-item label="SKU 名称">
      <el-input type="text" placeholder="请输入SKU的名称" v-model="skuInfo.skuName"></el-input>
    </el-form-item>
    <!--价格-->
    <el-form-item label="价格(元)">
      <el-input type="number" placeholder="请输入SKU的价格" v-model="skuInfo.price"></el-input>
    </el-form-item>
    <!--重量-->
    <el-form-item label="重量(千克)">
      <el-input type="number" placeholder="请输入SKU的重量" v-model="skuInfo.weight"></el-input>
    </el-form-item>
    <!--规格描述-->
    <el-form-item label="规格描述">
      <el-input type="textarea" placeholder="请输入SKU的规格描述" rows="4" v-model="skuInfo.skuDesc"></el-input>
    </el-form-item>
    <!--平台属性-->
    <el-form-item label="平台属性">
      <el-form label-width="80px" inline>
        <el-form-item :label="attr.attrName" style="margin-bottom: 5px" v-for="(attr, index) in attrList" :key="attr.id">
          <el-select v-model="attr.attrIdValueId" placeholder="请选择">
            <el-option :value="attr.id + '_' + value.id" :label="value.valueName" v-for="(value, index) in attr.attrValueList" :key="value.id"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
    </el-form-item>
    <!--销售属性-->
    <el-form-item label="销售属性">
      <el-form label-width="80px" inline>
        <el-form-item :label="attr.saleAttrName" style="margin-bottom: 5px" v-for="(attr, index) in saleAttrList" :key="attr.id">
          <el-select v-model="attr.valueId" placeholder="请选择">
            <el-option :value="value.id" :label="value.saleAttrValueName" v-for="(value, index) in attr.spuSaleAttrValueList" :key="value.id"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
    </el-form-item>
    <!--图片列表-->
    <el-form-item label="图片列表">
      <el-table :data="imageList" border @selection-change="handleSelectionChange">
        <el-table-column type="selection" width="55"> </el-table-column>
        <el-table-column label="图片">
          <template slot-scope="{ row, $index }">
            <img :src="row.imgUrl" alt="" width="150" height="150" />
          </template>
        </el-table-column>
        <el-table-column label="名称" prop="imgName"></el-table-column>
        <el-table-column label="操作">
          <template slot-scope="{ row, $index }">
            <el-tag type="success" v-if="row.isDefault === '1'">默认</el-tag>
            <el-button type="primary" v-else @click="setDefault(row)">设为默认</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="saveSku">保存</el-button>
      <el-button @click="$emit('cancel')">取消</el-button>
    </el-form-item>
  </el-form>
</template>
<script>
export default {
  name: 'SkuForm',
  data() {
    return {
      skuInfo: {
        // 当前界面出现的那一刻就可以得到了
        spuId: '', // spu的id
        tmId: '', // 品牌的id
        category3Id: '', // 三级分类的id
        // 在表单标签中通过v-model指令可以收集到
        skuName: '', // 名字
        skuDesc: '', // 描述信息
        price: '', // 价格
        weight: '', // 重量

        skuAttrValueList: [], // 存储选中的平台属性
        skuImageList: [], // 选中的图片数组
        skuSaleAttrValueList: [], // 销售属性数组

        skuDefaultImg: '', // 图片的地址(默认选中)---在点击设为默认按钮的时候进行数据的收集
        isSale: 0, // 上下架
      },
      spuInfo: {}, // spu对象数据
      attrList: [], // 平台属性数据数组
      saleAttrList: [], // 销售属性数据数组
      imageList: [], // 图片数据数组
      selectedImageList: [], // 存放选中的图片数组数据
    }
  },
  methods: {
    // 初始化数据方法
    initAddData(spuInfo) {
      this.spuInfo = spuInfo
      // 更新数据保存起来(品牌的id,spu的id,三级分类的id)
      this.skuInfo.tmId = spuInfo.tmId
      this.skuInfo.spuId = spuInfo.id
      this.skuInfo.category3Id = spuInfo.category3Id
      // 调用方法,方法内部调用三个接口
      this.getInitData()
    },
    // 该方法的目的是为了调用三个接口,进行数据的初始化操作
    async getInitData() {
      // 解构出来一二三级分类的id及spu的id
      const { category1Id, category2Id, category3Id, id } = this.spuInfo
      // 获取所有的平台属性数据数组
      const promise1 = await this.$API.attr.getAttrList(category1Id, category2Id, category3Id)
      // 根据spu的id获取对应的销售属性数据数组
      const promise2 = await this.$API.sku.getSpuSaleAttrList(id)
      // 根据spu的id获取对应的图片列表数据数组
      const promise3 = await this.$API.sku.getSpuImageList(id)
      // 统一处理promise
      const result = await Promise.all([promise1, promise2, promise3])
      // 更新数据
      this.attrList = result[0].data // 平台属性
      this.saleAttrList = result[1].data // 销售属性
      const imageList = result[2].data // 图片数据
      imageList.forEach((img) => {
        img.isDefault = '0'
      })
      this.imageList = imageList
    },

    // 设置当前的某个图片为默认的图片操作
    setDefault(img) {
      // 不行,isDefault属性不是响应式数据
      // img.isDefault = '1'
      // 设置当前的isDefault属性为响应式的数,以便数据变化了,界面可以重新渲染
      // this.$set(img,'isDefault','1')
      this.imageList.forEach((img) => {
        img.isDefault = '0'
      })
      img.isDefault = '1'
      // 保存当前默认的图片地址
      this.skuInfo.skuDefaultImg = img.imgUrl
    },
    // 表格中的复选框选中,选中内容发生变化就会触发的事件的回调函数
    handleSelectionChange(val) {
      // 最终存储的是用户选择了哪些图片
      this.selectedImageList = val
    },

    // 保存Sku操作
    async saveSku() {
      // 获取skuInfo对象,及平台属性数组,及销售属性数组及图片数组
      const { skuInfo, attrList, saleAttrList, selectedImageList } = this
      // 过滤 平台属性数据
      skuInfo.skuAttrValueList = attrList.reduce((pre, item) => {
        // 获取当前的某个选中的平台属性(平台属性id和选中的属性值id)
        const attrIdValueId = item.attrIdValueId
        // 判断是否存在
        if (attrIdValueId) {
          const [attrId, valueId] = attrIdValueId.split('_')
          // 存储已经选中的平台属性对象
          pre.push({
            attrId,
            valueId,
          })
        }
        return pre
      }, [])
      // 过滤 销售属性数据
      skuInfo.skuSaleAttrValueList = saleAttrList.reduce((pre, item) => {
        // 获取选中的销售属性的id值
        const valueId = item.valueId
        if (valueId) {
          pre.push({
            saleAttrValueId: valueId,
          })
        }
        return pre
      }, [])

      // 过滤 图片数据
      skuInfo.skuImageList = selectedImageList.map((img) => ({
        imgName: img.imgName,
        imgUrl: img.imgUrl,
        isDefault: img.isDefault,
        spuImgId: img.spuId,
      }))

      // 调用接口实现sku的保存操作
      const result = await this.$API.sku.addOrUpdateSkuInfo(skuInfo)
      if (result.code === 200) {
        this.$message.success('添加SKU成功')
        // 重置数据
        this.resetData()
        // 通知父级组件
        this.$emit('saveSuccess')
      } else {
        this.$message.error('添加SKU失败')
      }
      // 成功
      // 重置数据,关闭当前界面,通知父级组件成功了,
      // 失败
      // 提示信息
    },

    // 重置数据
    resetData() {
      this.skuInfo = {
        // 当前界面出现的那一刻就可以得到了
        spuId: '', // spu的id
        tmId: '', // 品牌的id
        category3Id: '', // 三级分类的id
        // 在表单标签中通过v-model指令可以收集到
        skuName: '', // 名字
        skuDesc: '', // 描述信息
        price: '', // 价格
        weight: '', // 重量

        skuAttrValueList: [], // 存储选中的平台属性
        skuImageList: [], // 选中的图片数组
        skuSaleAttrValueList: [], // 销售属性数组

        skuDefaultImg: '', // 图片的地址(默认选中)---在点击设为默认按钮的时候进行数据的收集
        isSale: 0, // 上下架
      }
      this.spuInfo = {} // spu对象数据
      this.attrList = [] // 平台属性数据数组
      this.saleAttrList = [] // 销售属性数据数组
      this.imageList = [] // 图片数据数组
      this.selectedImageList = [] // 存放选中的图片数组数据
    },
  },
}
</script>
<style scoped>
</style>