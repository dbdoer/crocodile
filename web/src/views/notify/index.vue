<template>
  <div class="app-container">
    <el-container>
      <el-aside width="250px">
        <el-table
          highlight-current-row
          size="mini"
          empty-text="暂无通知"
          :show-header="false"
          :data="data"
          @row-click="handleclick"
        >
          <el-table-column align="center" label="消息标题">
            <template slot-scope="scope">
              <span
                style="color: gray;font-size: 15px;font-weight: bold"
              >{{ scope.row.notify_typedesc }}: {{ scope.row.title }}</span>
              <br />
              <span style="color: #909399;font-size: 10px">{{ scope.row.notify_timedesc }}</span>
            </template>
          </el-table-column>
        </el-table>
      </el-aside>
      <el-divider direction="vertical"></el-divider>
      <el-main>
        <el-card v-show="notifycontent !== '' && data.length > 0 " class="box-card">
          <span style="white-space: pre-line;" v-html="notifycontent"></span>
        </el-card>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import { getnotify, readnotify } from "@/api/notify";

export default {
  data() {
    return {
      data: [],
      notifycontent: "",
      lastreadindex: null
    };
  },
  created() {
    this.startgetnotifys();
  },
  methods: {
    handleclick(row, column, event) {
      if (this.lastreadindex != null) {
        this.data.splice(this.lastreadindex, 1);
      }
      this.notifycontent = row.content;
      readnotify({ id: row.id });
      this.lastreadindex = this.data.indexOf(row);
    },
    startgetnotifys() {
      getnotify().then(resp => {
        this.data = resp.data;
      });
    }
  }
};
</script>

<style>
.el-divider--vertical {
  display: inline-block;
  width: 1px;
  height: 40em;
  margin: 0 8px;
  vertical-align: middle;
  position: relative;
  color: gray;
}
</style>