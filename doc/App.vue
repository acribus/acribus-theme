<template>
  <div id="app" v-if="initialized">
    <header class="sp-header">
      <div class="sp-logo"></div>
      <div class="user">
        <i class="el-icon-setting"></i>
        <span>Username</span>
      </div>
    </header>
    <div class="sp-left-side">
      <el-menu
        unique-opened
        :default-active="theme"
        background-color="var(--bg-color)"
        text-color="var(--text-color-light)"
        active-text-color="var(--text-color-light)">
        <el-submenu index="themes">
          <template slot="title">
            <i class="el-icon-edit"></i>
            <span>Themes</span>
          </template>
          <el-menu-item
            v-for="theme in themes"
            :key="theme"
            :index="theme"
            @click="selectTheme(theme)">
            {{ capitalize(theme) }}
          </el-menu-item>
        </el-submenu>
        <el-submenu index="fun">
          <template slot="title">
            <i class="el-icon-tickets"></i>
            <span>Fun</span>
          </template>
          <el-menu-item index="fun-item-1">That's fun!</el-menu-item>
          <el-menu-item index="fun-item-2">That's fun too!</el-menu-item>
        </el-submenu>
      </el-menu>
    </div>
    <div class="sp-right-side">
      <article>
        <div class="sp-breadcrumb-container">
          <i class="el-icon-location-outline"></i>
          <el-breadcrumb id="breadcrumb" separator-class="el-icon-arrow-right">
            <el-breadcrumb-item>Themes</el-breadcrumb-item>
            <el-breadcrumb-item>{{ capitalize(theme) }}</el-breadcrumb-item>
          </el-breadcrumb>
        </div>
        <div class="sp-card colors">
          <h1>Message</h1>
          <p>Hints like this line should be wrapped by a "p" element.</p>
          <p>Click buttons to open a message.</p>
          <div>
            <el-button type="info" @click="openMessage('info')">Info</el-button>
            <el-button type="primary" @click="openMessage('primary')">Primary</el-button>
            <el-button type="success" @click="openMessage('success')">Success</el-button>
            <el-button type="warning" @click="openMessage('warning')">Warning</el-button>
            <el-button type="danger" @click="openMessage('danger')">Danger</el-button>
          </div>
        </div>
        <div class="sp-card forms">
          <h1>Search forms</h1>
          <p>Use inline form in this area.</p>
          <p>Search button should be at loading state when loading data.</p>
          <el-form :inline="true">
            <el-form-item label="Input">
              <el-input placeholder="Input a value"></el-input>
            </el-form-item>
            <el-form-item label="Number">
              <el-input type="number" placeholder="Only accept numbers"></el-input>
            </el-form-item>
            <el-form-item label="Select">
              <el-select placeholder="Select an item" :multiple="true" :value="[1, 3]">
                <el-option label="1" :value="1"></el-option>
                <el-option label="2" :value="2"></el-option>
                <el-option label="3" :value="3"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="Date">
              <el-date-picker type="datetime" placeholder="Choose a date"></el-date-picker>
            </el-form-item>
            <el-form-item>
              <el-button native-type="reset">Reset</el-button>
              <el-button type="primary">Search</el-button>
              <el-button type="primary" :loading="true" disabled>Search</el-button>
            </el-form-item>
          </el-form>
        </div>
        <div class="sp-card tables">
          <h1>Table display</h1>
          <p>You need to set "width" for "action column" manually.</p>
          <p>Buttons in table should be grouped, and should in a smaller size.</p>
          <p>This table will switch loading status every 5 seconds.</p>
          <p>
            <el-button type="primary" @click="openModal">Open a dialog</el-button>
            <el-button>Secondary button</el-button>
          </p>
          <el-table
            v-loading="loading"
            border
            stripe
            :data="tableData"
            :header-cell-style="{ background: 'var(--bg-color-light)' }"
            style="width: 100%">
            <el-table-column prop="id" label="ID"></el-table-column>
            <el-table-column prop="name" label="Name"></el-table-column>
            <el-table-column prop="age" label="Age"></el-table-column>
            <el-table-column prop="gender" label="Gender">
              <template slot-scope="scope">
                {{ scope.row.gender ? '♀' : '♂' }}
              </template>
            </el-table-column>
            <el-table-column prop="birthday" label="Birthday">
              <template slot-scope="scope">1970-01-01 00:00:00</template>
            </el-table-column>
            <el-table-column prop="status" label="Status">
              <template slot-scope="scope">
                <span class="sp-tag" :class="`sp-bg-${statusColor[scope.row.status]}`">{{ statusText[scope.row.status] }}</span>
              </template>
            </el-table-column>
            <el-table-column label="Action" width="220">
              <template slot-scope="scope">
                <el-button-group>
                  <el-button size="mini" type="info" plain>Details</el-button>
                  <el-button size="mini" type="primary" plain>Edit</el-button>
                  <el-button size="mini" type="danger" plain>Delete</el-button>
                </el-button-group>
              </template>
            </el-table-column>
          </el-table>
          <p>
            <el-pagination
              @size-change="nullFunc"
              @current-change="nullFunc"
              :current-page="1"
              :page-size="20"
              :page-sizes="[10, 20, 30, 50]"
              layout="total, sizes, prev, pager, next"
              :total="233">
            </el-pagination>
          </p>
        </div>
        <div class="sp-card variables">
          <h1>Variables</h1>
          <p>Shopee theme uses <var>CSS Variables</var>, it's easy to change some styles by simply adding some lines to your css codes (all available variables are listed below):</p>
          <pre>{{ codes.changeStyles }}</pre>
          <p>Other variables by the way, are not meant to be modified.</p>
        </div>
      </article>
    </div>
    <el-dialog title="Add user" :visible="modalVisible" :before-close="hideModal" width="500px">
      <el-form label-width="80px" :model="ruleForm" :rules="formRules">
        <el-form-item label="Input" prop="validate">
          <el-input placeholder="Input a value" v-model="ruleForm.validate"></el-input>
        </el-form-item>
        <el-form-item label="Number">
          <el-input type="number" placeholder="Only accept numbers"></el-input>
        </el-form-item>
        <el-form-item label="Select">
          <el-select placeholder="Select an item" :multiple="true" :value="[1, 3]">
            <el-option label="1" :value="1"></el-option>
            <el-option label="2" :value="2"></el-option>
            <el-option label="3" :value="3"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Date">
          <el-date-picker type="datetime" placeholder="Choose a date"></el-date-picker>
        </el-form-item>
        <p>You need to set "label-width" for "el-form" manually.</p>
        <p>This is a fake form, data will not be submitted.</p>
      </el-form>
      <span slot="footer">
        <el-button @click="hideModal">Cancel</el-button>
        <el-button type="primary" @click="confirm">Confirm</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
const theme = location.search.substr(1)
// TODO: more color palette
const themes = ['shopee', 'element', 'flat-ui'/*, 'material', 'metro', 'fluent'*/]

export default {
  data() {
    return {
      initialized: false,
      theme,
      themes,
      tableData: [
        { id: 1, name: 'Rex', age: 22, gender: 0, status: 0 },
        { id: 2, name: 'Carina', age: 21, gender: 1, status: 0 },
        { id: 3, name: 'Summer', age: 22, gender: 0, status: 1 },
        { id: 4, name: 'Usual', age: 23, gender: 0, status: 2 },
        { id: 5, name: 'Monkey', age: 19, gender: 0, status: 0 }
      ],
      statusText: ['Normal', 'Leave', 'Offline'],
      statusColor: ['success', 'warning', 'info'],
      loading: false,
      modalVisible: false,
      ruleForm: { validate: '' },
      formRules: {
        validate: [{
          required: true,
          min: 10,
          message: 'Should more than 10 characters!',
          trigger: 'change'
        }]
      },
      codes: {
        changeStyles: `#app {
  --font-size: 14px;
  --header-height: 60px;
  --logo-height: 60px;
  --left-side-width: 250px;
  --breadcrumb-height: 20px;
  --card-border-radius: 5px;
  --card-spacing: 16px;
  --tag-border-radius: 5px;
  --tag-font-size: 12px;
  --uploader-size: 178px;
  --uploader-border-radius: 5px;
}`
      }
    }
  },
  methods: {
    capitalize(str) {
      return str[0].toUpperCase() + str.substr(1)
    },
    selectTheme(theme) {
      location.href = `?${theme}`
    },
    openNotify() {
      this.$notify({
        title: 'It works!',
        type: 'success',
        message: 'We\'ve laid the ground work for you. It\'s time for you to build something epic!',
        duration: 5000
      })
    },
    openMessage(type) {
      if (type === 'primary') {
        type = 'info'
      } else if (type === 'danger') {
        type = 'error'
      }
      this.$message({
        type,
        message: `You've got a piece of ${type} message.`
      })
    },
    setLoading(loading) {
      this.loading = loading
      if (this.loading) {
        setTimeout(() => this.setLoading(false), 1000)
      } else {
        setTimeout(() => this.setLoading(true), 5000)
      }
    },
    openModal() {
      this.modalVisible = true
      this.$nextTick(() => this.ruleForm.validate = 'some text')
    },
    hideModal() {
      this.modalVisible = false
    },
    confirm() {
      this.$confirm('Are you sure?', 'Confirm').then(() => {
        this.hideModal()
        this.$message({
          type: 'success',
          message: 'Successful.'
        })
      }).catch(() => {})
    },
    nullFunc() {}
  },
  created() {
    if (!theme) {
      location.href = `?${themes[0]}`
      return
    }
    if (process.env.NODE_ENV === 'production') {
      const link = document.createElement('link')
      link.rel = 'stylesheet'
      link.href = `dist/${theme}/index.css`
      document.head.appendChild(link)
      link.onload = () => {
        this.initialized = true
        this.openNotify()
      }
    } else {
      require(`../dist/${theme}/index.css`)
      this.initialized = true
      this.openNotify()
    }
  },
  mounted() {
    this.setLoading(false)
  }
}
</script>

<style>
.sp-logo {
  background-image: url(./assets/shopee.svg);
}
header .user {
  float: right;
}
h1 {
  font-weight: normal;
  font-size: 20px;
  margin: 0 0 1rem;
}
</style>
