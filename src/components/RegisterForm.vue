<template>
<div class="register">
  <h1> Sign up Form </h1>
  <form method="POST"  @submit.prevent="submit">
        <label for="fname">First Name: </label><br>
        <input type="text" name="fname" v-model.trim="$v.fname.$model"  placeholder="Your First Name">
         <div class="error" v-if="!$v.fname.required">Field is required</div><br>
          <div class="error" v-if="!$v.fname.alpha">Only Characters</div>
        <br>
        
        <label for="lname">Last Name: </label><br>
        <input type="text" name="lname" v-model.trim="$v.lname.$model" placeholder="Your Last name">
        <div class="error" v-if="!$v.lname.required">Field is required</div>         
        <br>
        <br>

        <label for="phone_no">Mobile Number: </label><br>
        <input type="number" name="phone_no" v-model.trim="$v.phone_no.$model" placeholder="Your mobile number">
        <div class="error" v-if="!$v.phone_no.required">Field is required</div>         
        <br>
        <br>
        
        <label for="email">Email: </label><br>
        <input type="email" name="email" v-model.trim="$v.email.$model" placeholder="Your Email">
        <div class="error" v-if="!$v.email.required">Field is required</div>
        <div class="error" v-if="!$v.email.email">Enter valid email address</div>
        <br>
        <br>

        <label for="password">Password: </label><br>
        <input type="password" name="password" v-model.trim="$v.password.$model" placeholder="Your Password">
        <div class="error" v-if="!$v.password.required">Field is required</div>
        <div class="error" v-if="!$v.password.valid ">Minimum Length of password should be 8 and contain atleast 1 Upercase, 1 lowercase and 1 special character</div>
        <br>
        <br>
        <a href="/login">
          <button class="button" type="submit" :disabled="submitStatus === 'PENDING'">Submit!</button>
        </a>
        <p class="typo__p" v-if="submitStatus === 'OK'">Thanks for your submission!</p>
        <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
        <p class="typo__p" v-if="submitStatus === 'PENDING'"> Sending... </p>
    
  </form>
  <br>
  <br>
  <a href="/login"> Click here for Login</a>
</div>
</template>

<script>
import axios from 'axios'
import { required,alpha,numeric, maxLength, minLength,email} from 'vuelidate/lib/validators';

export default {
     devServer: {
     proxy: 'http://localhost:3000'
 },
  data() {
    return {
        fname:'',
        lname:'',
        phone_no:0,
        email:'',
        password:'',
        submitStatus:''
    };
  },
  methods: {
       async submit() {
      console.log('submit!')
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        // do your submit logic here
        // axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*'
const options = {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          // 'Access-Control-Allow-Origin': 'http://localhost:3000/signup',
          // 'Access-Control-Allow-Methods': 'POST',
          // 'Access-Control-Allow-Credentials': 'true'
        },
        url: 'http://localhost:3000/signup/',
        data: { fname:this.fname,
            lname:this.lname,
            phone_no:this.phone_no,
            email:this.email,
            password:this.password },
      }
      const data = await axios.request(options)
      console.log(data)
        // fetch(' http://localhost:3000/signup',{
        //   method:'POST',
        //   headers: { 'Content-Type': 'application/json' },
        //   body:JSON.stringify({
        //     fname:this.fname,
        //     lname:this.lname,
        //     phone_no:this.phone_no,
        //     email:this.email,
        //     password:this.password
        //   }),

        // })
        // .then(response => response.json())
        // .catch(err=> console.error("There was an error "+ err));

        this.submitStatus = 'PENDING'
        setTimeout(() => {
          this.submitStatus = 'OK'
        }, 5000)
      }
    }
    
  },
  validations: {
      fname:{
          required,
          alpha
      },
      lname:{
        required,
        alpha
      },
      phone_no:{
          required,
          numeric,
          maxLength: maxLength(10)
      },

      email:{
          required,
          email
      },
      password:{
        required,
        minLength: minLength(8),
        valid:function(value){
          const containsUppercase = /[A-Z]/.test(value)
        const containsLowercase = /[a-z]/.test(value)
        const containsNumber = /[0-9]/.test(value)
        const containsSpecial = /[#?!@$%^&*-]/.test(value)
        return containsUppercase && containsLowercase && containsNumber && containsSpecial
        }
          
      }


  }
};
</script>
<style>
    .error{
        color: red;
    }
</style>