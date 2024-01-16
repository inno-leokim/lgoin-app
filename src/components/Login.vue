<template>
  <div class="protected" v-if="loginSuccess">
      <h1>
          <h3 variant="success">보안 사이트에 대한 액세스가 허용되었습니다</h3>
      </h1>
      <h5>로그인 성공!</h5>
  </div>
  <div class="unprotected" v-else-if="loginError">
      <h1>
          <h3 variant="danger">이 페이지에 대한 접근 권한이 없습니다.</h3>
      </h1>
      <h5>로그인 실패!</h5>
  </div>
  <div class="unprotected" v-else>
      <h1>
          <h3 variant="info">로그인해주세요</h3>
      </h1>
      <h5>로그인 하지 않았습니다. 로그인을 해주세요</h5>

      <form @submit.prevent="login()">
          <label>
              <input type="text" placeholder="username" v-model="username">
          </label>
          <label>
              <input type="password" placeholder="password" v-model="password">
          </label>
          <button variant="success" type="submit">Login</button>
          <p v-if="error" class="error">Bad login information</p>
      </form>
  </div>
</template>

<script>
  // import axios from 'axios'

  export default {
      name: 'login',
      data() {
          return {
              loginSuccess: false,
              loginError: false,
              username: '',
              password: '',
              error: false
          }
      },
      methods: {
          async login() {
              
              try {
                const result = await fetch("http://localhost:8080/login", {
                  method: "POST",
                  headers: {
                      'Content-Type': 'application/x-www-form-urlencoded',
                      'credentials': 'same-origin'
                  },
                  body: new URLSearchParams({
                    username: this.username,
                    password: this.password
                  })
                });

                const data = await result.json();
                
                console.log(data);

                if(data.statusCode === 200){
                  this.loginSuccess = true;
                  this.loginError = false;
                }else{
                  this.loginError = true;
                  this.loginSuccess = false;
                }
                
              } catch (error) {
                // this.loginError = true;
                throw new Error(error);
              }
              
          }
      }
  }

</script>