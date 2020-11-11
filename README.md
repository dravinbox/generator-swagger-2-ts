# generator-swagger-2-ts [![NPM version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url] [![Dependency Status][daviddm-image]][daviddm-url]
> 

A Swagger Codegen for typescript and javascript, A [Yeoman](http://yeoman.io) generator 

This package generates a js/typescript class from a swagger url. The code is generated using Yeoman and you can modify template to diy the generated code

[中文文档](./README.cn.md)

## Installation

First, install [Yeoman](http://yeoman.io) and generator-swagger-2-ts using [npm](https://www.npmjs.com/) (we assume you have pre-installed [node.js](https://nodejs.org/)).

```bash
npm install -g yo
npm install -g generator-swagger-2-ts
```

Then generate your new project:

```bash
yo swagger-2-ts
```


According to the prompt:
- input swagger - UI address, for example: `http://192.168.86.8:8051/swagger-ui.html`, the package will automatically replace the url address 'swagger-ui.html' to 'v2/api-docs' to get really swagger API docs url,  then download and parse it to generate the API class
- choose generate js or typescript
- you can customize the generated API class name, API file name

or you can pass arguments by command:

```bash
 yo swagger-2-ts --swaggerUrl=http://localhost:8080/swagger-ui.html --className=API --type=typescript --outputFile=api.ts
```

- swaggerUrl: swagger ui url
- className： API class name
- type： typescript or javascipt
- outputFile: api file save to

## generated CODE：

```javascript

export type AccountUserInfo = {
  disableTime?: string
  isDisable?: number
  lastLoginIp?: string
  lastLoginPlace?: string
  lastLoginTime?: string
  openId?: string
}


export type BasePayloadResponse = {
  data?: object
  desc?: string
  retcode?: string

}

/**
 * User Account Controller
 * @class UserAccountAPI
 */
export class UserAccountAPI {
/**
  * changeUserState
  * @method
  * @name UserAccountAPI#changeUserState
  
  * @param  accountUserInfo - accountUserInfo 
  
  * @param $domain API域名,没有指定则使用构造函数指定的
  */
  changeUserState(parameters: {
    'accountUserInfo': AccountUserInfo,
    $queryParameters?: any,
    $domain?: string
  }): Promise<AxiosResponse<BasePayloadResponse>> {

    let config: AxiosRequestConfig = {
      baseURL: parameters.$domain || this.$defaultDomain,
      url: '/userAccount/changeUserState',
      method: 'PUT'
    }

    config.headers = {}
    config.params = {}

    config.headers[ 'Accept' ] = '*/*'
    config.headers[ 'Content-Type' ] = 'application/json'

    config.data = parameters.accountUserInfo
    return axios.request(config)
  }

/**
  * changeUserState
  * @method
  * @name UserAccountAPI#changeUserState
  
  * @param  accountUserInfo - accountUserInfo 
  
  * @param $domain API域名,没有指定则使用构造函数指定的
  */
  async changeUserStateAsync(parameters: {
    'accountUserInfo': AccountUserInfo,
    $queryParameters?: any,
    $domain?: string
  }): Promise<BasePayloadResponse> {

    let resp = await this.changeUserStateAsync(parameters);
    return Promise.resolve(resp.data);
  }

}


/**
 * 管理系统接口描述
 * @class API
 */
export class API {
  /**
   *  API构造函数
   * @param domain API域名
   */
  constructor(domain?: string) {
    this.$defaultDomain = domain || 'http://localhost:8080'
  }

  _UserAccountAPI: UserAccountAPI = null;

  /**
  * 获取 User Account Controller API
  * return @class UserAccountAPI
  */
  getUserAccountAPI(): UserAccountAPI {
    if (!this._UserAccountAPI) {
      this._UserAccountAPI = new UserAccountAPI(this.$defaultDomain)
    }
    return this._UserAccountAPI
  }
}

```

## Use the API Class

```javascript
import { API } from './api'
// in main.ts
let api = new API('YOUR_API_HOST')
api.withAuthorization("YOUR_TOKEN")
// add Interceptors
api.withInterceptors(
    response => {
        if (response.status === 200) {
            return Promise.resolve(response)
        } else {
            return Promise.reject(response)
        }
    },
    error => {
        if (error.response.status) {
            switch (error.response.status) {
                case 401:
                    console.error("401 Unauthorized")
                    break
                case 403:
                    console.error("403 Forbidden")
                    break
                case 404:
                    console.error("404 Not Found")
                    break
                default:
                    console.error(error.response.data.message)
            }
            return Promise.reject(error.response)
        }
    })

api.getUserAccountAPI().changeUserState({
  isDisable: 1
  openId: 'open id'
}).then(resp=>{
  console.log(resp)
})


```

 ## Reference

 - [swagger-js-codegen](https://github.com/wcandillon/swagger-js-codegen)

## License

Apache-2.0 © [jadepeng]()


[npm-image]: https://badge.fury.io/js/generator-swagger-2-ts.svg
[npm-url]: https://npmjs.org/package/generator-swagger-2-ts
[travis-image]: https://travis-ci.org/jadepeng/generator-swagger-2-ts.svg?branch=master
[travis-url]: https://travis-ci.org/jadepeng/generator-swagger-2-ts
[daviddm-image]: https://david-dm.org/jadepeng/generator-swagger-2-ts.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/jadepeng/generator-swagger-2-ts
