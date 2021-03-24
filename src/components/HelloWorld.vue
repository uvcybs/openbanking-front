<template>
  <div>
    <div>
      <h1>사용자 인증</h1>
      <button @click="requestAuth">REQUEST</button>
      <button @click="saveAuth">SAVE</button>
      <h4>code: {{ auth.code }}</h4>
      <h4>scope: {{ auth.scope }}</h4>
      <h4>client_info: {{ auth.client_info }}</h4>
      <h4>state: {{ auth.state }}</h4>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  mounted: function () {
    this.init()
  },
  data () {
    return {
      auth: {
        code: '',
        scope: '',
        client_info: '',
        state: ''
      }
    }
  },
  methods: {
    init () {
      if (sessionStorage.auth) {
        this.auth = sessionStorage.auth
      }
    },
    requestAuth () {
      axios.get('https://testapi.openbanking.or.kr/oauth/2.0/authorize',
        {
          params: {
            response_type: 'code',
            client_id: '35081a8f-6c53-40af-b8d4-611f1ee3ba3f',
            redirect_uri: 'http://localhost:8080/#/',
            scope: 'login inquiry transfer',
            state: 'b80BLsfigm9OokPTjy03elbJqRHOfGSY',
            auth_type: '0'
          }
        })
        .then(function (response) {
          window.location.href = response.request.responseURL
          console.log(response)
        })
        .catch(function (error) {
          console.log(error.response)
        })
    },
    saveAuth () {
      sessionStorage.auth = {
        code: this.$route.query.code,
        scope: this.$route.query.scope,
        client_info: this.$route.query.client_info,
        state: this.$route.query.state
      }
    }
  }
}
</script>
