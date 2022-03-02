<template>
<div class="container">
    <div class="formParent">
    <el-alert v-if="notSubmitted"
        title="You must log in to see the tables"
        type="warning"
        class="elAlert">
    </el-alert>
      <el-alert v-if="!notSubmitted"
        title="You have logged in successfully"
        type="success"
        class="elAlert">
    </el-alert>
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
      <el-form-item label="Activity zone">
        <el-select v-model="ruleForm.region" placeholder="please select your town">
          <el-option label="Craiova" value="Craiova"></el-option>
          <el-option label="Bucuresti" value="Bucuresti"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="Age" prop="age">
        <el-input v-model.number="ruleForm.age"></el-input>
      </el-form-item>
      <el-form-item label="Select a day">
        <el-date-picker
          v-model="value1"
          type="date"
          placeholder="Pick a day">
        </el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" plain @click="submitForm('ruleForm')">Submit</el-button>
        <el-button type="danger" plain @click="resetForm('ruleForm')">Reset</el-button>
      </el-form-item>
    </el-form>
    <div v-if="!notSubmitted">
      <table-test></table-test>
    </div>
  </div>
 
</div>
</template>

<script>
import TableTest from './TableTest.vue'
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
          region: '',
        },
        rules: {
           region: [
            { required: true, message: 'Please select Activity zone', trigger: 'change' }
          ],
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
        notSubmitted: true, 
        pickerOptions: {
          disabledDate(time) {
            return time.getTime() > Date.now();
          },
          shortcuts: [{
            text: 'Today',
            onClick(picker) {
              picker.$emit('pick', new Date());
            }
          }, {
            text: 'Yesterday',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          }, {
            text: 'A week ago',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', date);
            }
          }]
        },
        value1: '',
        value2: '',
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.notSubmitted = false;
            console.log("submitted");
            console.log(this.notSubmitted);
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
    }
  }
</script>

<style scoped>

.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.formParent {
  width: 850px;
}

.elAlert {
  margin: 10px 0;
}

</style>
