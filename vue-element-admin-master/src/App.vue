<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  create: function() {
    const whiteUrl = ['xxx111', 'xxx222']
    const url = '/book/ddd'
    const request = axios.create({
      baseURL: 'https://sdasd.das',
      timeout: 5000
    })

    // /请求拦截器
    request.interceptors.request.use(
      config => {
        const url = config.url.replace(config.baseURL, '')
        if (whiteUrl.some(wl => url === wl)) {
          return config
        }

        config.headers['token'] = 'token'

        return config
      },
      err => {
        console.log(err)
      }
    )

    // /返回拦截器
    request.interceptors.response.use(
      response => {
        console.log(response)

        if (response.data && response.data.error_code === 0) {
          return response.data
        } else {
          Promise.reject(response.data.msg)
        }
      },
      err => {
        console.log(err)
      }
    )

    request({
      url,
      methods: 'get',
      params: {
        openId: 123
      }
    }).then(request => {
      console.log(request)
    }).catch(err => {
      console.log(err)
    })
  }
}
</script>
