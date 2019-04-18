<template>
  <div class="searchAlert">
    <div class="inputBox">
      <el-form
        :inline="true"
        :model="searchForm"
        class="searchAlertForm"
        ref="searchForm"
      >
        <el-form-item
          v-for="item in inputArr"
          :key="item.label"
					:prop="item.dataName"
        >
          <el-tooltip
            :content="item.label"
            placement="top"
          >
            <el-input
              v-model="searchForm[item.dataName]"
              :placeholder="item.label"
              size="mini"
            ></el-input>
          </el-tooltip>
        </el-form-item>
      </el-form>
    </div>
    <div class="tableBox">
      <el-table
        :data="searchDataList"
        style="width: 100%"
        size="mini"
        highlight-current-row
        @current-change="selectRowData"
				:border="true"
      >
        <el-table-column
          v-for="item in coleumArr"
          :key="item.label"
          :prop="item.prop"
          :label="item.label"
          :width="item.width"
        >
        </el-table-column>
      </el-table>
    </div>
    <div
      class="pagination"
      v-if="searchPaging"
    >
      <el-pagination
        layout="prev, pager, next"
        :total="searchPaging.totalPage"
        :small="true"
        :page-size="searchPaging.pageSize"
        @current-change="searchData"
      >
      </el-pagination>
    </div>
    <div class="buttonBox">
      <el-button
        size="mini"
        @click="resetSearchForm"
      >重置</el-button>
      <el-button
        size="mini"
        @click="searchData"
        type="primary"
      >搜索</el-button>
    </div>
  </div>
</template>
<script>
export default {
  name: "searchAlert",
  props: {
    inputArr: Array,
    buttonArr: Array,
    coleumArr: Array,
    searchPaging: {
      type: Object,
      default: null
    },
    searchFn: Function,
    searchDataList: Array,
    searchCbFn: Function
  },
  data() {
    return {
      searchForm: {}
    };
  },
  methods: {
    searchData(pageNum) {
      this.searchFn(this.searchForm, pageNum);
    },
    selectRowData(row) {
      this.searchCbFn(row);
    },
    resetSearchForm() {
			console.log(this.searchForm)
      this.$refs.searchForm.resetFields();
    }
  }
};
</script>
<style lang="scss" scoped>
.searchAlertForm {
  display: flex;
  .el-form-item {
    margin-bottom: 0px;
  }
}
.buttonBox {
  display: flex;
  justify-content: flex-end;
}
</style>
<!--
/**
 * 弹框搜索组件可嵌套弹框, 可直接页面使用
 * 可简单定制搜索form, 表格, 分页
 * 实现查询, 单选, 回显, 分页查询
 * 待扩展: 无表格小型增改弹框
 */

/*
完整使用方法
<searchAlert :inputArr="inputArr" :coleumArr="coleumArr" :searchDataList="searchDataList" :searchPaging="searchPaging"  :searchCbFn="searchCbFn" :searchFn="searchFn"></searchAlert>
----------------------------------------
创建form以及表格基本结构data控制
	//form结构
	inputArr: [
		{ label: "编号", dataName: "EMPID" },
		{ label: "姓名", dataName: "EMPNAME" },
		{ label: "手机号", dataName: "MOBILENO" },
	],
	//表格结构
	coleumArr: [
		{ label: "人员编号", width: "80", prop: "EMPID" },
		{ label: "人员姓名", width: "100", prop: "EMPNAME" },
		{ label: "手机号", width: "100", prop: "MOBILENO" },
		{ label: "邮箱", width: "100", prop: "OEMAIL" },
		{ label: "部门组织", width: "200", prop: "ORGNAME" },
	],
	//查询到的数据
	searchDataList: [],
	//分页控制, null或无该属性时为不显示
	searchPaging: {}
----------------------------------------
在使用该组件的地方实现这两个方法 methods
//回显功能, 可以拿到单选数据
searchCbFn(rowData) {
	console.log(rowData)
},
//查询功能, 分页等
searchFn(formData, pageNum = 1) {
	api({
		bizContent: {
			...formData
		},
		PageCond: {
			//接口分页大小控制
			length: 5,
			begin: (pageNum - 1) * 5
		}
	}).then(res => {
		console.log(res.data)
		this.searchDataList = res.data.datas
		//页面展示 分页大小控制
		this.searchPaging = { ...res.data.page, pageSize: 5 }
	}).catch(err => {
		throw err;
	})
}
*/
-->
