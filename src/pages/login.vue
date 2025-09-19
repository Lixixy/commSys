<template>
  <el-row class="login">
    <el-col :lg="16" :md="12" class="login_left">
      <div>
        <div class="text_A_left">欢迎光临</div>
        <div class="text_B_left">CommSys是一款一站式校园社团活动管理平台</div>
      </div>
    </el-col>
    <el-col :lg="8" :md="12" class="login_right">
      <h2>欢迎回来</h2>
      <div class="text_A_right">
        <span class="text_B_right"></span>
        <span>账号密码登录</span>
        <span class="text_B_right"></span>
      </div>
      <el-form :model="form" :rules="rules" ref="uerFormInst" class="text fields" @keydown.enter="onSubmit">
        <el-form-item prop="username">
          <el-input v-model="form.username" :prefix-icon="User" placeholder="请输入用户名"/>
        </el-form-item>
        <el-form-item prop="password">
          <el-input type="password" show-password v-model="form.password" :prefix-icon="Lock" placeholder="请输入密码"/>
        </el-form-item>
        <el-form-item prop="region">
          <el-select v-model="form.region" placeholder="请输入你的职位">
            <el-option label="学生" value="Student" />
            <el-option label="社团管理员" value="Cluber" />
            <el-option label="系统管理员" value="Admin" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button round color="rgba(11,68,66,0.84)" class="buttonA" type="primary" @click="onSubmit">登 录</el-button>
        </el-form-item>
        <div class="register_A">
          <a @click="router.push('./register')" class="register_B">注册</a>
        </div>
      </el-form>
    </el-col>
  </el-row>
</template>
<script setup>
  import { ref,reactive } from 'vue'
  import {User,Lock} from '@element-plus/icons-vue'
  import {ElMessage} from 'element-plus'
  import router from '@/router'

  // do not use same name with ref
  const form = reactive({
    username: '',
    password:'',
    region: 'Student',
  })

  const uerFormInst = ref(null)

  const onSubmit = () => {
    uerFormInst.value.validate((valid)=>{
      if(valid){
        request.post('@/pages/login', account).then((res) => {
          if (res.code === '200') {
            localStorage.setItem('account',JSON.stringify(res.data))
            if (res.data === 'Admin'){
              router.push('@/pages/back/index')
            }else if (res.data === 'Cluber'){
              router.push('@/pages/front/index')
            }else {
              router.push('@/pages/index')
            }
            ElMessage.success('登录成功')
          }else{
            ElMessage.error(res.msg || '出错啦')
          }
        })
      }
      return false
    })
  }

  const rules = {
    username:[
      {required: true, message:'请输入用户名', trigger:'blur'},
      {min: 3, max: 10,message:'长度在3到10个字符',trigger:'blur'},
    ],
    password:[
      {required: true, message:'请输入密码', trigger:'blur'},
      {min: 1, max: 20,message:'长度在1到20个字符',trigger:'blur'},
    ],
    region:[
      {required: true,message:'请输入你的职位', trigger:'change'}
    ]
  }
</script>
<style>
.login{
  min-height: 100vh;
  background-color: rgba(var(--color-background-primary), 0.81);
}
.login_left{
  display: flex;
  justify-content: center;
  align-items: center;
}
.login_right{
  background-color: rgb(var(--color-background-secondary));
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.text_A_left{
  font-weight: bold;
  font-size: 300%;
  margin-bottom: 4px;
  color: rgb(var(--color-background-secondary));
}
.text_B_left{
  color: rgb(var(--color-background-secondary));
}
h2{
  font-weight: bold;
  font-size: 220%;
  color: rgba(3,23,22,0.87);
}
.text_A_right{
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 10px;
  color:rgba(var(--color-background-primary),0.5);
}
.text_B_right{
  height: 1px;
  width: 30px;
  background-color: rgba(var(--color-background-primary),0.5);
  margin: 3px;
}
.text fields{
  width: 250px;
}
.buttonA{
  width: 250px;
}
.register_A{
  float: right;

}
.register_B{
  font-size: 75%;
  color: rgba(var(--color-background-primary),0.84);
}
</style>