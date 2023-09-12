<template>
  <div>
    <div class="data-display">
      <el-table :data="tableData" :key="currentPage" border style="width: 100%" @selection-change="handleSelectionChange">
        <el-table-column type="selection" width="55"></el-table-column> <!-- 添加选择框列 -->
        <el-table-column prop="id" label="编号"></el-table-column>
        
        <el-table-column prop="name" label="姓名"></el-table-column>
        <el-table-column prop="gender" label="性别"></el-table-column>
        <el-table-column prop="age" label="年龄"></el-table-column>
        <el-table-column prop="salesperson" label="业务员"></el-table-column>
        <el-table-column prop="comment" label="评论"></el-table-column>
        <el-table-column label="编辑" width="100">
          <template slot-scope="scope">
            <el-button type="text" @click="editComment(scope.row)">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 添加“设置业务员”的按钮 -->
    <el-button v-if="selectdRows.length > 0" type="primary" @click="openAssignDialog">设置业务员</el-button>

    <!-- 弹出的编辑评论对话框 -->
    <el-dialog title="编辑评论" :visible="dialogVisible" @close="closeDialog">
      <el-form :model="editedCommentForm" label-width="80px">
        <el-form-item label="编号">
          <el-input v-model="editedCommentForm.id" disabled></el-input>
        </el-form-item>
        <el-form-item label="姓名">
          <el-input v-model="editedCommentForm.name" disabled></el-input>
        </el-form-item>
        <el-form-item label="性别">
          <el-input v-model="editedCommentForm.gender" disabled></el-input>
        </el-form-item>
        <!-- 添加其他字段的显示 -->
        <el-form-item label="评论">
          <el-input v-model="editedCommentForm.comment"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="saveComment">保存</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!-- 弹出的分配业务员对话框 -->
    <el-dialog title="分配业务员" :visible="assignDialogVisible" @close="closeAssignDialog">
      <el-select v-model="selectedSalesperson" placeholder="选择业务员">
        <el-option v-for="(salesperson, index) in salespersons" :key="index" :label="salesperson" :value="salesperson"></el-option>
      </el-select>
      <el-button type="primary" @click="saveAssignments">保存</el-button>
    </el-dialog>

    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[10, 20, 50, 100, 500, 1000]"
      :page-size="pageSize"
      layout="sizes, prev, pager, next, jumper"
      :total="totalItems"
    />
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      totalItems: 0,
      currentPage: 1,
      pageSize: 10,
      tableData: [],
      dialogVisible: false,
      assignDialogVisible: false,
      editedCommentForm: {
        id: null,
        name: '',
        gender: '',
        comment: '',
      },
      selectedSalesperson: '',
      selectdRows: [],
      salespersons: [],
    };
  },
  computed: {
    currentPageData() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      const endIndex = startIndex + this.pageSize;
      return this.tableData.slice(startIndex, endIndex);
    },
  },
  methods: {
    fetchData() {
      // Clear the existing data
      this.tableData = [];
      axios
        .post('http://192.168.24.133:8077/list', {
          page: this.currentPage,
          pageSize: this.pageSize,
        })
        .then(response => {
          this.tableData = response.data.data;
          this.totalItems = response.data.selectTotalCnt;
        })
        .catch(error => {
          console.error('API error:', error);
        });
    },
    handleSelectionChange(selection) {
      this.selectdRows = selection;
    },
    handleSizeChange(newSize) {
      this.pageSize = newSize;
      this.fetchData();
    },
    handleCurrentChange(newPage) {
      this.currentPage = newPage;
      this.fetchData();
    },
    editComment(row) {
      this.editedCommentForm.id = row.id;
      this.editedCommentForm.comment = row.comment;
      this.editedCommentForm.gender = row.gender;
      this.editedCommentForm.name = row.name;
      this.dialogVisible = true;
    },
    saveComment() {
      // Send edited comment to the server
      axios
        .post('http://192.168.24.133:8077/updateComment', {
          id: this.editedCommentForm.id,
          comment: this.editedCommentForm.comment,
        })
        .then(response => {
          // Update table data
          this.fetchData();
          // Close the dialog
          this.dialogVisible = false;
          // Clear edited comment form
          this.editedCommentForm.id = null;
          this.editedCommentForm.comment = '';
        })
        .catch(error => {
          console.error('API error:', error);
        });
    },
    closeDialog() {
      this.dialogVisible = false;
      this.editedCommentForm.id = null;
      this.editedCommentForm.comment = '';
    },
    openAssignDialog() {
      this.assignDialogVisible = true;
    },
    closeAssignDialog() {
      this.assignDialogVisible = false;
    },
    saveAssignments() {
      const selectedIds = this.selectdRows.map(row => row.id);
      axios
        .post('http://192.168.24.133:8077/assignSalesperson', {
          ids: selectedIds,
          user: 'admin',
          salespersonId: this.selectedSalesperson,
        })
        .then(response => {
          this.closeAssignDialog();
          this.fetchData();
        })
        .catch(error => {
          console.error('API error:', error);
        });
    },
    loadSalespersons() {
      axios
        .post('http://192.168.24.133:8077/salespersons')
        .then(response => {
          this.salespersons = response.data;
        })
        .catch(error => {
          console.error('API error:', error);
        });
    },
  },
  created() {
    this.fetchData();
    this.loadSalespersons();
  },
};
</script>

<style scoped>
.data-display {
  margin: 20px;
}
</style>
