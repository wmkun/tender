<template>
  <div class="home-wrap">

    <!-- *******************************************************************    
                               页面 1   登录   pagesstaues===0
    ************************************************************************ -->
    <div v-if="this.pagesstaues===0" class="login">

      <div class="myheader">
        <mt-header fixed title="普天设计招标代理"></mt-header>
      </div>
      <div class="input-wrap">
        <mt-field label="用户名" placeholder="请输入用户名" v-model="username"></mt-field>
        <mt-field label="公司名称" placeholder="请输入公司名称" v-model="tenerCompanyName"></mt-field>
        <mt-field label="密码" placeholder="请输入密码" type="password" v-model="password"></mt-field>
        <div class="login-btn">
          <mt-button type="primary" size="small" @click="login">登&nbsp;录</mt-button>
          <mt-button type="default" size="small" @click="regist">注&nbsp;册</mt-button>
        </div>
      </div>

    </div>

    <!-- *******************************************************************    
                               页面 2   项目列表  pagesstaues===1
    ************************************************************************ -->

    <div v-if="this.pagesstaues===1" class="project-list">
      <div class="myheader">
        <mt-header fixed title="普天设计招标代理">
          <router-link to="/login/" slot="right">
            <Button type="primary" @click="backToLogin">注销<i class="el-icon-arrow-right el-icon--right"></i></Button>

            <!-- <mt-button @click="backToList">close</mt-button> -->
          </router-link>

        </mt-header>
      </div>
      <div>{{this.companyName}}{{this.userName}}</div>
      <ul class="mylist1">
        <div><span>项目列表：</span></div>
        <li class="list" v-for="(item, index) in list" :key="index">
          <div class="namalisttime">
            <p class="listprojectname">{{item.projectName}}</p>
            <div class="listprojectnum">
              <div>
                <span>{{item.projectNum}}</span>
              </div>
              <div>
                <span class="listprojectnumdata">截止时间:{{item.deadlineTime}}</span>
              </div>
            </div>
          </div>
          <div class="btn-group">
            <div class="btngroupstatus">{{item.status==2?'发售截止':(item.status==1?'正在发售':'未发售')}}</div>
            <div class="btngroupbaoming" v-if="item.status === 1">
              <mt-button type="primary" size="small" @click.stop='baoming(item)'>报名入口</mt-button>
              <!-- <mt-button type="primary" size="small" @click.stop='baoming(item)'>报名入口</mt-button> -->
              <!-- <button class="btn-active" @click.stop='baoming(item)'>报名入口</button> -->
            </div>
          </div>
        </li>
      </ul>
    </div>

    <!-- *******************************************************************    
                               页面 3   报名页面  pagesstaues===2
    ************************************************************************ -->
    <div>
      <div v-if="this.pagesstaues===2" class="home-wrap1">

        <div class="myheader">
          <!-- <mt-header fixed title="普天设计招标代理"></mt-header> -->
          <mt-header title="普天设计招标代理">
            <router-link to="/" slot="left">
              <mt-button icon="back" @click="backToList">返回项目列表</mt-button>
              <!-- <Button type="primary" icon="el-icon-arrow-left" @click="backToList">上一步</Button> -->
              <!-- <Button type="primary" @click="backToList" icon="el-icon-arrow-left">返回列表</Button> -->
              <!-- <Button type="primary" @click="backToList">返回列表<i class="el-icon-arrow-left el-icon--left"></i></Button> -->
              <!-- <mt-button @click="backToList">close</mt-button> -->
            </router-link>
            <router-link to="/login/" slot="right">
              <Button type="primary" @click="backToLogin">注销<i class="el-icon-arrow-right el-icon--right"></i></Button>

              <!-- <mt-button @click="backToList">close</mt-button> -->
            </router-link>
          </mt-header>
        </div>
        <!-- *******************************************************************    
                                 按步骤 报名信息填报
          ****************************************************************** -->
        <div style="padding:15px">

          <!-- *******************************************************************    
                                  第一步，报名信息填报
          ****************************************************************** -->
          <div v-if="active === 0" id="btn-baominginput" class="baomingwrap">
            <div class="stepshead">
              <Steps :space="200" :active="active" finish-status="success">
                <Step title="填报报名信息" class="stemtititle1"></Step>
                <Step title="信息确认" class="stemtititle2"></Step>
                <Step title="缴费转账" class="stemtititle1"></Step>
              </Steps>
            </div>

            <mt-field label="公司名称：" placeholder=" " v-model="this.tenerCompanyName" :disabled="true"></mt-field>
            <mt-field label="投标联系人：" placeholder="请输入投标联系人姓名" v-model="ruleForm.tenerPeopleName"></mt-field>
            <!-- <mt-field label="身份证号：" placeholder="请输入投标联系人身份证号" v-model="ruleForm.tenerPeopleIdCard"></mt-field> -->
            <mt-field label="联系电话：" placeholder="请输入投标联系人联系电话" v-model="ruleForm.tenerPeopleNamePhone"></mt-field>
            <mt-field label="电子邮箱：" placeholder="请输入投标联系人电子邮箱" v-model="ruleForm.tenerPeopleNameEmail"></mt-field>

            <span class="name-befoer">
              营业执照：
            </span>
            <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
              :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
              :file-list="fileList">
              <Button size="small" type="primary">点击上传</Button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </Upload>
            <div slot="tip" class="el-upload__tip"></div>
            <span class="name-befoer">
              法人授权：
            </span>
            <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
              :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
              :file-list="fileList">
              <Button size="small" type="primary">点击上传</Button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </Upload>

            <div slot="tip" class="el-upload__tip"></div>
            <span class="name-befoer">
              被授权人身份证电子版：
            </span>

            <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
              :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
              :file-list="fileList">
              <Button size="small" type="primary">点击上传</Button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </Upload>

            <div v-if="projectNameItme.esStatus === true">
              <div style="margin-top: 15px;color: red;">本招标项目需上传发售方平台电子报名截图</div>

              <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
                :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
                :file-list="fileList">
                <Button size="small" type="primary">点击上传</Button>
                <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </Upload>
            </div>

            <span style="margin-top:30px;font-size:12px;color:red" class="name-befoer">
              是否需要电子发票发票
            </span>

            <!-- <div style="margin-top: 10px;font-size:12px;color:red" label="是否需要电子发票发票" prop="isNeedInvoice"> </div> -->
            <mt-switch v-model="ruleForm.isNeedInvoice">{{ruleForm.isNeedInvoice}}</mt-switch>

            <div v-if="ruleForm.isNeedInvoice === true" label-position="left" label-width="120px" class="invoiceinputmsg">
              <mt-field label="纳税人识别号：" placeholder=" 请输入公司纳税人识别号" v-model="ruleForm.tenderCompanyLicenseNum" class="tax-num"></mt-field>
              <mt-field label="公司地址：" placeholder=" 请输入公司地址" v-model="ruleForm.tenderCompanyAdress"></mt-field>
              <mt-field label="公司电话：" placeholder=" 请输入公司电话" v-model="ruleForm.tenderCompanyCall"></mt-field>
              <mt-field label="开户银行：" placeholder=" 请输入公司开户银行" v-model="ruleForm.tenderCompanyBank"></mt-field>
              <mt-field label="开户账号：" placeholder=" 请输入公司开户账号" v-model="ruleForm.tenderCompanyCard"></mt-field>
              <mt-field label="发票接收人：" placeholder=" 请输入发票收件人姓名" v-model="ruleForm.sendPeopleName"></mt-field>
              <mt-field label="接收人电话：" placeholder=" 请输入发票接收人电话" v-model="ruleForm.sendPeoplePhone"></mt-field>
              <mt-field label="接收人邮箱：" placeholder=" 请输入发票接收人邮箱" v-model="ruleForm.sendPeopleAdress"></mt-field>
              <!-- <mt-field label="发票收件人：" placeholder=" 发票收件人：" v-model="ruleForm.sendPeopleName"></mt-field> -->
            </div>

            <div style="margin-top: 20px">
              <Button @click="resetForm()">重置</Button>
              <Button style="margin-left:20px" type="primary" @click="submitForm('ruleForm')">下一步<i class="el-icon-arrow-right el-icon--right"></i></Button>

            </div>

            <!-- <div style="margin-top: 20px">
            <Button type="success" @click="backToList">返回项目列表</Button>
            </div> -->

          </div>
          <!--</el-container>-->
          <!-- *******************************************************************    
                                  第二步，信息确认
          ************************************************************************ -->
          <div v-if="active === 1" class="baomingwrap">
            <div class="stepshead">
              <Steps :space="200" :active="active" finish-status="success">
                <Step title="填报报名信息" class="stemtititle1"></Step>
                <Step title="信息确认" class="stemtititle2"></Step>
                <Step title="缴费转账" class="stemtititle1"></Step>
              </Steps>
            </div>

            <!-- <Row>
            <Col :span="12">
            <div class="grid-content bg-purple"></div></Col>
            <Col :span="12">
            <div class="grid-content bg-purple-light"></div>
            </Col>
            </Row> -->

            <mt-field label="公司名称：" placeholder="未填写" v-model="this.tenerCompanyName" :disabled="true"></mt-field>
            <mt-field label="投标联系人：" placeholder="未填写" v-model="ruleForm.tenerPeopleName" :disabled="true"></mt-field>
            <!-- <mt-field label="身份证号：" placeholder="请输入投标联系人身份证号" v-model="ruleForm.tenerPeopleIdCard"></mt-field> -->
            <mt-field label="联系电话：" placeholder="未填写" v-model="ruleForm.tenerPeopleNamePhone" :disabled="true"></mt-field>
            <mt-field label="电子邮箱：" placeholder="未填写" v-model="ruleForm.tenerPeopleNameEmail" :disabled="true"></mt-field>

            <span class="name-befoer">
              营业执照：
            </span>
            <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
              :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
              :file-list="fileList">
              <Button size="small" type="primary">点击上传</Button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </Upload>
            <div slot="tip" class="el-upload__tip"></div>
            <span class="name-befoer">
              法人授权：
            </span>
            <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
              :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
              :file-list="fileList">
              <Button size="small" type="primary">点击上传</Button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </Upload>

            <div slot="tip" class="el-upload__tip"></div>
            <span class="name-befoer">
              被授权人身份证电子版：
            </span>

            <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
              :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
              :file-list="fileList">
              <Button size="small" type="primary">点击上传</Button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </Upload>

            <div v-if="projectNameItme.esStatus === true">
              <div style="margin-top: 15px;color: red;">本招标项目需上传发售方平台电子报名截图</div>

              <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
                :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
                :file-list="fileList">
                <Button size="small" type="primary">点击上传</Button>
                <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </Upload>
            </div>

            <!-- <mt-field style="margin-top: 10px" label="是否需要电子发票发票" prop="isNeedInvoice">
            <mt-switch v-model="ruleForm.isNeedInvoice"></mt-switch>
            </mt-field> -->
            <div v-if="ruleForm.isNeedInvoice === false" label-position="left" label-width="120px" class="invoiceinputmsg">
              <div style="margin-top: 15px;color: red;margin-bottom: 15px;">已选取不需要电子发票</div>
            </div>

            <div v-if="ruleForm.isNeedInvoice === true" label-position="left" label-width="120px" class="invoiceinputmsg">

              <div style="margin-top: 15px;color: red;">已选取需要电子发票，发票信息：</div>
              <mt-field label="纳税人识别号：" placeholder="未填写" v-model="ruleForm.tenderCompanyLicenseNum" class="tax-num"
                :disabled="true"></mt-field>
              <mt-field label="公司地址：" placeholder=" 未填写" v-model="ruleForm.tenderCompanyAdress" :disabled="true"></mt-field>
              <mt-field label="公司电话：" placeholder=" 未填写" v-model="ruleForm.tenderCompanyCall" :disabled="true"></mt-field>
              <mt-field label="开户银行：" placeholder=" 未填写" v-model="ruleForm.tenderCompanyBank" :disabled="true"></mt-field>
              <mt-field label="开户账号：" placeholder=" 未填写" v-model="ruleForm.tenderCompanyCard" :disabled="true"></mt-field>
              <mt-field label="发票接收人：" placeholder=" 未填写" v-model="ruleForm.sendPeopleName" :disabled="true"></mt-field>
              <mt-field label="接收人电话：" placeholder=" 未填写" v-model="ruleForm.sendPeoplePhone" :disabled="true"></mt-field>
              <mt-field label="接收人邮箱：" placeholder=" 未填写" v-model="ruleForm.sendPeopleAdress" :disabled="true"></mt-field>
              <!-- <mt-field label="发票收件人：" placeholder=" 发票收件人：" v-model="ruleForm.sendPeopleName"></mt-field> -->
            </div>

            <!-- <Button type="primary" @click="backTobaoming">上一步</Button>
           <Button type="primary" @click="baomingqueren">下一步</Button> -->
            <ButtonGroup>
              <Button type="primary" icon="el-icon-arrow-left" @click="backTobaoming">上一步</Button>
              <Button style="margin-left:10px" type="primary" @click="baomingqueren">下一步<i class="el-icon-arrow-right el-icon--right"></i></Button>
            </ButtonGroup>

            <!-- <div style="margin-top: 20px">
              <Button type="success" @click="backToList">返回项目列表</Button>
             </div> -->

          </div>

          <!-- *******************************************************************    
                                  第三步，缴费审核
          ************************************************************************ -->

          <div v-if="active === 2" class="baomingwrap">
            <div class="stepshead">
              <Steps :space="200" :active="active" finish-status="success">
                <Step title="填报报名信息" class="stemtititle1"></Step>
                <Step title="信息确认" class="stemtititle2"></Step>
                <Step title="缴费转账" class="stemtititle1"></Step>
              </Steps>
            </div>
            <div class="paynote1">
              招标项目：{{this.projectNameItme.projectName}}
            </div>
            <div class="paynote2">
              标书价格：{{this.projectNameItme.price}}元
            </div>
            <div class="paynote3">
              请选择付款方式：
            </div>
            <!-- <el-radio-group v-model="payweixin" class="paychoicess">
                <el-radio-button v-model="payweixin" label="1" border size="medium" text-color="red">微信付款</el-radio-button>
                <el-radio-button v-model="payweixin" label="0" border size="medium" text-color="red" class="bank">银行转账</el-radio-button>
              </el-radio-group> -->
            <!-- <mt-radio title="手机系统" v-model="userPhone" :options="options">
              </mt-radio>
              <div v-if="userPhone===0">
                <canvas ref='sign'></canvas>
                <Button type="danger" @click="clearsign">清除</Button>
                <Button type="danger" @click="savesign">保存</Button>

              </div>
              <div v-if="userPhone===1">
                <div style="margin-top: 15px;color: red;">苹果系统手机需前往</div>

                
              </div> -->

            <div><canvas ref='sign'></canvas></div>
            <Button type="danger" @click="clearsign">清除</Button>
            <Button type="danger" @click="savesign">保存</Button>
            <div style="margin-top: 15px;color: red;">请先前往签名确认系统进行声明签名，上传截图至本平台</div>
            <!-- <Button type="success" @click="testnext">前往声明签名系统</Button> -->
            <mt-cell title="" to="http://101.254.243.216" is-link value="跳转至声明签名">
            </mt-cell>
            <div style="margin-top: 20px">
              <Upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
                :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed"
                :file-list="fileList">
                <Button size="small" type="primary">点击上传</Button>
                <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </Upload>
            </div>
            <div style="margin-top: 60px">

              <Button type="primary" @click="backTobaoming">上一步</Button>
              <Button type="danger" @click="upToCheck">提交审核</Button>

            </div>

            <!-- <div style="margin-top: 20px">
            <Button type="success" @click="testnext">返回项目列表</Button>
            </div> -->
          </div>
        </div>
        <!--</el-main>-->

        <!--</el-container>-->
      </div>
    </div>
    <!-- *******************************************************************    
                               页面 4   审核状态查询页面  pagesstaues===3
    ************************************************************************ -->
    <div v-if="this.pagesstaues===3" class="home-wrap1">

      <div class="myheader">
        <!-- <mt-header fixed title="普天设计招标代理"></mt-header> -->
        <mt-header title="普天设计招标代理">
          <router-link to="/" slot="left">
            <mt-button icon="back" @click="backToList">返回项目列表</mt-button>
            <!-- <Button type="primary" icon="el-icon-arrow-left" @click="backToList">上一步</Button> -->
            <!-- <Button type="primary" @click="backToList" icon="el-icon-arrow-left">返回列表</Button> -->
            <!-- <Button type="primary" @click="backToList">返回列表<i class="el-icon-arrow-left el-icon--left"></i></Button> -->
            <!-- <mt-button @click="backToList">close</mt-button> -->
          </router-link>
          <router-link to="/login/" slot="right">
              <Button type="primary" @click="backToLogin">注销<i class="el-icon-arrow-right el-icon--right"></i></Button>

              <!-- <mt-button @click="backToList">close</mt-button> -->
            </router-link>
        </mt-header>
      </div>

      <div style="padding:15px">
        <div class="paynote1">
          招标项目：{{this.projectNameItme.projectName}}
        </div>
        <div class="paynote2">
          审核状态：{{this.msgIsSignUp}}
        </div>

        <div style="padding:25px">
          <div v-if="this.isSignUp===1">
            <div v-if="this.isSendDoc===0"><Button type="primary" @click="sendDocButton">获取标书</Button></div>
            <div v-if="this.isSendDoc===1">已获取过标书，请勿重复获取</div>

          </div>

        </div>

      </div>
    </div>

  </div>

</template>
<script type="text/javascript">
import { Upload, Button, Steps, Step, ButtonGroup, Row, Col } from 'element-ui'
import { mapGetters } from 'vuex'
import SignaturePad from 'signature_pad'
// https://cn.vuejs.org/v2/style-guide/#组件-实例的选项的顺序-推荐
export default {
  name: 'Login',
  // name: 'Baoming',
  components: {
    Upload,
    Button,
    Steps,
    Step,
    ButtonGroup,
    Row,
    Col
  },

  model: {},
  props: {},
  data() {
    return {
      username: '',
      pagesstaues: 0,
      tenerCompanyName: '',
      userloginintem: {
        username: '',
        tenerCompanyName: '',
        password: ''
      },
      password: '',
      projectNow: '',
      stemStauts: {
        title1: '填报报名信息',
        title2: '信息确认',
        title3: '缴费转账'
      },
      list: [
        {
          projectName: '项目111111111111',
          projectNum: '8864161313-dgdh-5422',
          deadlineTime: '2018-11-11',
          status: 0,
          esStatus: false,
          price: '230.35',
          linkProject: 'https://www.baidu.com/',
          linkDoc: 'https://www.baidu.com/'
        },
        {
          projectName: '项目22222222222',
          projectNum: '8864161313-dgdh-5422',
          deadlineTime: '2018-11-11',
          status: 1,
          esStatus: true,
          price: '230.35',
          linkProject: 'https://www.baidu.com/',
          linkDoc: 'https://www.baidu.com/'
        },
        {
          projectName: '项目3333333333',
          projectNum: '8864161313-dgdh-5422',
          deadlineTime: '2018-11-11',
          status: 1,
          esStatus: false,
          price: '230.35',
          linkProject: 'https://www.baidu.com/',
          linkDoc: 'https://www.baidu.com/'
        },
        {
          projectName: '项目44444444444444',
          projectNum: '8864161313-dgdh-5422',
          deadlineTime: '2018-11-11',
          status: 2,
          esStatus: false,
          price: '230.35',
          linkProject: 'https://www.baidu.com/',
          linkDoc: 'https://www.baidu.com/'
        }
      ],
      options: [
        {
          label: '安卓系统',
          value: '0',
          disabled: true
        },
        {
          label: '苹果系统',
          value: '1',
          disabled: true
        }
      ],
      active: 0,
      payweixin: 1,
      sheetVisible: false,
      userPhone: 0,
      isuploadlic: 0,
      msgIsSignUp: '正在审核',

      projectNameItme: {
        projectName: '',
        projectNum: '',
        deadlineTime: '',
        status: 0,
        esStatus: false,
        price: '',
        linkProject: '',
        linkDoc: ''
      },

      ruleForm: {
        // tenerCompanyName: '',
        tenerPeopleName: '',
        tenerPeopleIdCard: '',
        tenerPeopleNamePhone: '',
        tenerPeopleNameEmail: '',
        isNeedInvoice: false,
        tenderUploadLic: '',
        tenderUploadSouquan: '',
        tenderUploadEs: '',
        tenderCompanyLicenseNum: '',
        tenderCompanyAdress: '',
        tenderCompanyCall: '',
        tenderCompanyBank: '',
        tenderCompanyCard: '',
        sendPeopleName: '',
        sendPeoplePhone: '',
        sendPeopleAdress: '',
        isSendDoc: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        region: [{ required: true, message: '请选择活动区域', trigger: 'change' }],
        date1: [{ type: 'date', required: true, message: '请选择日期', trigger: 'change' }],
        date2: [{ type: 'date', required: true, message: '请选择时间', trigger: 'change' }],
        type: [{ type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }],
        resource: [{ required: true, message: '请选择活动资源', trigger: 'change' }],
        desc: [{ required: true, message: '请填写活动形式', trigger: 'blur' }]
      },
      actions: [
        {
          name: '从相册选择',
          method: function() {}
        }
      ],
      fileList: [],
      signaturePad: {}
    }
  },

  computed: {},
  watch: {},
  created() {},
  mounted() {},
  beforeDestroy() {},
  methods: {
    login() {
      // 在这里写login的逻辑
      this.userloginintem.username = this.username
      this.userloginintem.tenerCompanyName = this.tenerCompanyName
      this.userloginintem.password = this.password
      // Vue.http.post('http://101.254.243.216:3001/v1/tendor/login', this.userloginintem).then(function(data) {
      //   if (data.ok) {
      //     this.pagesstaues = 1
      //   }
      // })

      this.pagesstaues = 1

      // this.$xhr
      //   .post(`http://101.254.243.216:3001/v1/tendor/login`,this.userloginintem,{emulateJSON: true})
      //   .then(res => {
      //     console.log(res)
      //     alert(res.message)
      //     if (res.status === 600) {
      //     } else if (res.status === 601) {
      //     } else if (res.status === 602) {
      //     } else if (res.status === 200) {
      //       this.pagesstaues = 1
      //     }
      //   })
      //   .catch(err => {
      //     console.log(err)
      //   })

      // this.username=
      // this.$store.dispatch('setCompanyName', {
      //   name: this.username,
      //   com:this.tenerCompanyName
      // })

      // this.$router.push({ path: `/index`, query: { name:this.username,com:this.tenerCompanyName} })
    },
    regist() {
      this.$router.push({ path: '/register' })
    },
    getProjectList() {
      this.$xhr.get(`url`).then(res => {
        console.log(res)
      })
    },

    getListData() {
      this.$xhr.get('http://101.254.243.216:3001/test', {}).then(res => {
        console.log(JSON.stringify(res))
      })
    },

    uploadlic() {
      this.sheetVisible = true
    },

    baomingqueren() {
      this.active = 2
      this.initSign()

      //				console.log(params)
      //				this.getListData()
    },

    submitForm(formName) {
      //		        this.$refs[formName].validate((valid) => {
      //		          if (valid) {
      //		            alert('submit!');
      //		          } else {
      //		            console.log('error submit!!');
      //		            return false;
      //		          }
      //		        });
      this.active = 1
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    },
    baoming(item) {
      // this.$router.push({ path: `/baoming`, query: { id: item.projectName } })
      // console.log(item)
      this.pagesstaues = 2
      active = 0
    },
    backTobaoming() {
      this.active = 0
    },
    backToList() {
      // this.$router.push({ name: 'index' })
      this.pagesstaues = 1
      this.active = 0
    },
    backToLogin() {
      this.pagesstaues = 0
      this.active = 0
    },
    baoming(params) {
      this.projectNameItme.projectName = params.projectName
      this.projectNameItme.projectNum = params.projectNum
      this.projectNameItme.deadlineTime = params.deadlineTime
      this.projectNameItme.status = params.status
      this.projectNameItme.esStatus = params.esStatus
      this.projectNameItme.price = params.price
      this.projectNameItme.linkProject = params.linkProject
      this.projectNameItme.linkDoc = params.linkDoc
      this.projectNameItme.isSendDoc = params.isSendDoc
      this.active = 0
      this.pagesstaues = 2
      this.getListData()
      step.currentStepNumber = 1
    },
    upToCheck() {
      this.pagesstaues = 3
      if (this.initSign === 0) {
        this.msgIsSignUp = '正在审核'
      } else if (this.initSign === 1) {
        this.msgIsSignUp = '审核通过'
      } else {
        this.msgIsSignUp = '报名信息有误，审核不通过'
      }
    },
    resetForm() {
      ;(this.ruleForm.tenerPeopleName = ''),
        (this.ruleForm.tenerPeopleIdCard = ''),
        (this.ruleForm.tenerPeopleNamePhone = ''),
        (this.ruleForm.tenerPeopleNameEmail = ''),
        (this.ruleForm.isNeedInvoice = false),
        (this.ruleForm.tenderUploadLic = ''),
        (this.ruleForm.tenderUploadSouquan = ''),
        (this.ruleForm.tenderUploadEs = ''),
        (this.ruleForm.tenderCompanyLicenseNum = ''),
        (this.ruleForm.tenderCompanyAdress = ''),
        (this.ruleForm.tenderCompanyCall = ''),
        (this.ruleForm.tenderCompanyBank = ''),
        (this.ruleForm.tenderCompanyCard = ''),
        (this.ruleForm.sendPeopleName = ''),
        (this.ruleForm.sendPeoplePhone = ''),
        (this.ruleForm.sendPeopleAdress = '')
    },

    handlePreview(file) {},
    handleRemove(file, fileList) {},
    beforeRemove(file, fileList) {},
    handleExceed(file, fileList) {},
    initSign() {
      // console.log(SignaturePad)
      setTimeout(() => {
        this.signaturePad = new SignaturePad(document.querySelector('canvas'), {
          minWidth: 1,
          maxWidth: 3,
          penColor: 'rgb(66, 133, 244)'
        })
      })
    },
    savesign() {
      let pic = this.signaturePad.toDataURL('image/jpeg')
      this.signaturePad.toDataURL()
      // this.signaturePad.clear();
      console.log(pic)
    },
    clearsign() {
      this.signaturePad.clear()
    },
    testnext() {},
    sendDocButton() {
      this.isSendDoc = 1
    }
  }
}
</script>
<style lang="scss" scoped>
.login {
  height: 100%;
  box-sizing: border-box;
  margin-top: 40%;
  padding: 20px;
  display: flex;
  align-items: center;
  .input-wrap {
    width: 100%;
    .login-btn {
      font-size: 0;
      padding-top: 20px;
      text-align: center;
      button {
        &:nth-child(1) {
          margin-right: 20px;
        }
      }
    }
  }
}
.project-list {
  padding: 20px;
  width: 100%;
  box-sizing: border-box;
  .mylist1 {
    width: 100%;
    margin-top: 40px;
    .list {
      padding: 5px;
      border: 1px solid #ddd;
      margin-bottom: 5px;
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      .btn-group {
        width: 28%;
        height: 100%;
        .btngroupstatus {
          // margin-top: 5px;
        }
        .btngroupbaoming {
          margin-top: 5px;
        }
      }
      .namalisttime {
        .listprojectname {
          font-weight: bold;
        }
        .listprojectnum {
          margin-top: 3px;
          margin-left: 10px;
          // height: 30px;
          .listprojectnumdata {
            //margin-left: 3px;
          }
        }
      }
    }
  }
}
.btn-active {
  font-size: 15px;
}


@import '~assets/css/lib-variable';
@import '~assets/css/lib-mixin';
.home-wrap {
  padding: 0px;
  width: 100%;
}
.home-wrap1
{
  // padding: 0px,10px;
  
  width: 100%;

}
.myheader {
  margin-bottom: 15px;
}
.baomingwrap{
  width: 100%;
  // margin-left: 10px;
  // padding-right: 10px;
}
.paynote1 {
  margin-top: 40px;
  margin-left: 50px;
  width: 90%;
  display: flex;
}
.paynote2 {
  margin-top: 20px;
  margin-left: 50px;
  width: 90%;
}

.paynote3 {
  margin-top: 50px;
  margin-left: 20px;
  width: 90%;
}
.paychoicess {
  margin-top: 20px;
  margin-left: 80px;
  width: 90%;
}

/*#btn-baominginput {
		height: 1800px;
	}*/

.mylist1 {
  align-items: flex-start;
  text-align: left;
}

.bank {
  margin-left: 5px;
}

.demo-input-suffix {
  display: flex;
  text-align: left;
}

.el-select .el-input {
  width: 80%;
  /*margin: 0 auto;*/
  font-size: 8px;
  margin-bottom: 4px;
}

.name-befoer {
  width: 130px;
  justify-content: space-between;
  margin: 0 auto;
  font-size: 16px;
}
/*.eel-icon-upload .el-upload__text{

		width :200;
	}*/

.input-with-select .el-input-group__prepend {
  background-color: #fff;
}

.el-header,
.el-footer {
  background-color: rgb(95, 130, 194);
  height: 40px;
  color: black;
  font-size: 20px;
  text-align: center;
  line-height: 40px;
}

.el-aside {
  background-color: #d3dce6;
  color: #333;
  text-align: center;
  line-height: 80px;
}

.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
  line-height: 160px;
}

body > .el-container {
  margin-bottom: 10px;
  height: 40;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
.invoiceinputmsg {
  /deep/ .mint-cell {
    .mint-cell-title,
    .mint-cell-value {
      font-size: 14px;
    }
  }
}
.stepshead {
  margin-bottom: 5px;

  /deep/ .el-step {
    .el-step_main {
      .stemtititle1 {
        font-size: 8px;
      }
      .stemtititle2 {
        font-size: 16px;
      }
    }
  }
}

canvas {
  border: 1px solid #666;
}
.Row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.Col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;

}
.mt-field{
  
}
</style>
