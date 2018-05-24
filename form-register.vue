<template>
   <div class="con">
      <div class="con-inner">
          <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="100px" class="demo-ruleForm">
            <el-form-item label="手机号码" prop="tel">
               <el-input v-model="ruleForm2.tel" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="图片验证码" class="img-code" prop="imgCode">
               <el-input v-model="ruleForm2.imgCode" auto-complete="off"></el-input>
               <a @click='changeCodeImg()'><span>{{codeImg}}</span></a>
            </el-form-item>
            <el-form-item label="密码" prop="pass">
                <el-input type="password" v-model="ruleForm2.pass" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="确认密码" prop="checkPass">
                <el-input type="password" v-model="ruleForm2.checkPass" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="年龄" prop="age">
                <el-input v-model.number="ruleForm2.age"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
                <el-button @click="resetForm('ruleForm2')">重置</el-button>
            </el-form-item>
          </el-form>
      </div>
   </div>
</template>
<script>

export default{
	name:'form-register',
	data() {
      var validateTel = (rule,value,callback) => {
        var pattern = /^(13[0-9]|14[5|7|9]|15[0|1|2|3|5|6|7|8|9]|17[0|3|6|7|8]|18[0|1|2|3|4|5|6|7|8|9])\d{8}$/;
        if(value === ""){
           callback(new Error('手机号码不能为空'));
        }else if(!pattern.test(value)){
           callback(new Error('请输入正确的手机号码'));
        }else{
           callback();
        }
      }
      var checkAge = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('年龄不能为空'));
        }
        setTimeout(() => {
          if (!Number.isInteger(value)) {
            callback(new Error('请输入数字值'));
          } else {
            if (value < 18) {
              callback(new Error('必须年满18岁'));
            } else {
              callback();
            }
          }
        }, 1000);
      };
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else if(value.length < 6 || value.length > 16){
          callback(new Error('密码格式是6-16位之间'));
        }else {
          if (this.ruleForm2.checkPass !== '') {
            this.$refs.ruleForm2.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        }else if(value.length <6 || value.length > 16){
          callback(new Error('密码格式是6-16位之间'));
        } else if (value !== this.ruleForm2.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      var validateImgcode = (rule,value,callback) => {
         if(value === ''){
           callback(new Error('图片验证码不能为空'));
         }else if(value !== this.codeImg){
           callback(new Error('图片验证码错误'));
         }else{
            callback();
         }
      };
      return {
        codeImg:'5689',
        ruleForm2: {
          tel:'',
          pass: '',
          checkPass: '',
          age: '',
          imgCode:''
        },
        rules2: {
          tel:[
             { validator: validateTel,trigger:'change'}
          ],
          pass: [
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          age: [
            { validator: checkAge, trigger: 'blur' }
          ],
          imgCode:[
            { validator: validateImgcode,trigger:'blur' }
          ]
        }
      };
    },
    methods: {
      changeCodeImg(){
         　var code = "0123456789qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM";
           var char = '';
           var result = '';
           for(var i = 0;i < 4;i ++){
             var code_index = Math.round(Math.random()*61);
             var char = code[code_index];
             if(result.toUpperCase().indexOf(char.toUpperCase()) > -1){
               i--;
               continue;
             }
             result += char;
           }
           this.codeImg = result;
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
}

</script>
<style scoped>
.con{
  margin-top:50px;
}
.con .con-inner{
    width:500px;
    margin:0 auto;
}
.img-code{
  position:relative;
}
.img-code a{
  display:inline-block;
  position:absolute;
  top:0;
  right:0;
  height:40px;
  width:50px;
  cursor:pointer;
}
.img-code a span{
  display:inline-block;
  width:100%;
  height:100%;
  text-align:center;
  background-color:#66b1ff;
  color:#fff;
}
</style>