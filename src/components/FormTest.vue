<template>
  <div>
    <el-form :model="ruleForm" status-icon :rules="rules" v-if="notSubmitted" ref="ruleForm" label-width="120px" class="demo-ruleForm">
      <el-form-item label="Username" prop="username">
        <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="Password" prop="pass">
        <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="Confirm" prop="checkPass">
        <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="Age" prop="age">
        <el-input v-model.number="ruleForm.age"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">Submit</el-button>
        <el-button @click="resetForm('ruleForm')">Reset</el-button>
      </el-form-item>
    </el-form>
    <div v-else>
      <table-test></table-test>
    </div>
  </div>
</template>

<script>
import TableTest from './TableTest.vue';
  export default {
    components: {
      TableTest
    },
    data() {
      var checkAge = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('Please input the age'));
        }
        setTimeout(() => {
          if (!Number.isInteger(value)) {
            callback(new Error('Please input digits'));
          } else {
            if (value < 18) {
              callback(new Error('Age must be greater than 18'));
            } else {
              callback();
            }
          }
        }, 1000);
      };
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('Please input the password'));
        } else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('Please input the password again'));
        } else if (value !== this.ruleForm.pass) {
          callback(new Error('Two inputs don\'t match!'));
        } else {
          callback();
        }
      };
      return {
        ruleForm: {
          pass: '',
          checkPass: '',
          age: '',
          username: '',
        },
        rules: {
          pass: [
            { validator: validatePass, trigger: 'blur', required: true, }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur', required: true, }
          ],
          age: [
            { validator: checkAge, trigger: 'blur', required: true, }
          ],
          username: [
            { required: true, message: 'Please input username', trigger: 'blur' },
            { min: 3, max: 20, message: 'Length should be 3 to 20', trigger: 'blur' }
          ],
        },
        notSubmitted: true
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.notSubmitted = false;
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
