---
title: AdminTemplate
katex: true
date: 2024-12-17 22:10:05
excerpt: 基于 vue3 + vite5 + typescript + element-plus 构建的后台管理前端模板（配套后端源码），vue-element-admin 的 vue3 版本。
tag: java
---

对[vue3-element-admin](https://github.com/youlaitech/vue3-element-admin)的源码分析，为了配合写后端代码

# main.ts

```typescript
const app = createApp(App);
// 注册插件
app.use(setupPlugins);
app.mount("#app");
```
- setupPlugins 将插件封装进插件初始化函数

```typescript
export default {
  install(app: App<Element>) {
    // 自定义指令(directive)
    setupDirective(app);
    // 路由(router)
    setupRouter(app);
    // 状态管理(store)
    setupStore(app);
    // 国际化
    setupI18n(app);
    // Element-plus图标
    setupElIcons(app);
    // 路由守卫
    setupPermission();
    // 初始化 WebSocket
    webSocketManager.setupWebSocket();
    // 注册 CodeMirror
    app.use(InstallCodeMirror);
  },
};

```
- 每个setupXXX 函数实际都是一个 app.use(router);
- 其中setupRouter 会把根目录重定向自动跳转/dashboard，但是跳转之前会经过 setupPermission 中定义的路径守卫
- 路径守卫判断当前的登录状态，如果没登录就会触发redirectToLogin，最后定位到next(`/login?redirect=${encodeURIComponent(redirect)}`);

# login

在定位的/login组件中提交表单触发userStore.login(loginData.value)
```typescript
function login(loginData: LoginData) {
return new Promise<void>((resolve, reject) => {
  AuthAPI.login(loginData)
    .then((data) => {
      const { tokenType, accessToken, refreshToken } = data;
      setToken(tokenType + " " + accessToken); // Bearer eyJhbGciOiJIUzI1NiJ9.xxx.xxx
      setRefreshToken(refreshToken);
      resolve();
    })
    .catch((error) => {
      reject(error);
    });
});
}
```
- 注意修改AuthAPI.login函数中的header,默认是"Content-Type": "multipart/form-data",如果用对象接收参数需要改成"Content-Type": "application/json"
- 最后调用的login就是发送axios请求
- 两个set都是localStorage.setItem
## mock
- 发送的登录请求是url: `${AUTH_BASE_URL}/login`，const AUTH_BASE_URL = "/api/v1/auth";
- 应用注册了mock插件
```typescript
# 应用端口
VITE_APP_PORT=3000

# 代理前缀
VITE_APP_BASE_API=/dev-api

# 接口地址
VITE_APP_API_URL=https://api.youlai.tech # 线上
# VITE_APP_API_URL=http://localhost:8989    # 本地

# WebSocket 端点（不配置则关闭），线上 ws://api.youlai.tech/ws ，本地 ws://localhost:8989/ws
VITE_APP_WS_ENDPOINT=

# 启用 Mock 服务
VITE_MOCK_DEV_SERVER=true

```
- 并且配置了proxy
```typescript
server: {
  host: "0.0.0.0",
  port: +env.VITE_APP_PORT,
  open: true,
  proxy: {
    // 代理 /dev-api 的请求
    [env.VITE_APP_BASE_API]: {
      changeOrigin: true,
      // 代理目标地址：https://api.youlai.tech
      target: env.VITE_APP_API_URL,
      rewrite: (path) => path.replace(new RegExp("^" + env.VITE_APP_BASE_API), ""),
    },
  },
},

```
- 在base.ts中配置了拼接路径path.join(import.meta.env.VITE_APP_BASE_API + "/api/v1/", mock.url);并且在登录的mock中有
```json
{
url: "auth/login",
method: ["POST"],
body: {
  code: "00000",
  data: {
    accessToken:
      "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImRlcHRJZCI6MSwiZGF0YVNjb3BlIjoxLCJ1c2VySWQiOjIsImlhdCI6MTcyODE5MzA1MiwiYXV0aG9yaXRpZXMiOlsiUk9MRV9BRE1JTiJdLCJqdGkiOiJhZDg3NzlhZDZlYWY0OWY3OTE4M2ZmYmI5OWM4MjExMSJ9.58YHwL3sNNC22jyAmOZeSm-7MITzfHb_epBIz7LvWeA",
    tokenType: "Bearer",
    refreshToken: null,
    expires: null,
  },
  msg: "一切ok",
},
}
```
- 因此最后模拟的请求路径就是/dev-api/api/v1/auth/login，和上面发送的登录请求基本对应，因此会返回上面定义的body数据
## axios

在接受到的请求结果中配置了拦截器，当返回的是成功的时候直接把data返回，而不要code和msg
```typescript

// 响应拦截器
service.interceptors.response.use(
  (response: AxiosResponse) => {
    // 如果响应是二进制流，则直接返回，用于下载文件、Excel 导出等
    if (response.config.responseType === "blob") {
      return response;
    }

    const { code, data, msg } = response.data;
    if (code === ResultEnum.SUCCESS) {
      return data;
    }

    ElMessage.error(msg || "系统出错");
    return Promise.reject(new Error(msg || "Error"));
  },
  async (error: any) => {
    // 非 2xx 状态码处理 401、403、500 等
    const { config, response } = error;
    if (response) {
      const { code, msg } = response.data;
      if (code === ResultEnum.ACCESS_TOKEN_INVALID) {
        // Token 过期，刷新 Token
        return handleTokenRefresh(config);
      } else if (code === ResultEnum.REFRESH_TOKEN_INVALID) {
        return Promise.reject(new Error(msg || "Error"));
      } else {
        ElMessage.error(msg || "系统出错");
      }
    }
    return Promise.reject(error.message);
  }
);

``` 

## getCaptcha

后端写好接口之后，把枚举类的成功code改统一，然后把response的属性名也统一一下就能成功拿到数据了


# role

```typescript
function handleQuery() {
  loading.value = true;
  RoleAPI.getPage(queryParams)
    .then((data) => {
      roleList.value = data.list;
      total.value = data.total;
    })
    .finally(() => {
      loading.value = false;
    });
}

```
- 角色管理页面一加载就会调用这个函数，往表单里面填充数据
```typescript
const queryParams = reactive<RolePageQuery>({
  pageNum: 1,
  pageSize: 10,
});

export interface RolePageQuery extends PageQuery {
  /** 搜索关键字 */
  keywords?: string;
}

/**
* 分页查询参数
*/
interface PageQuery {
  pageNum: number;
  pageSize: number;
}

```
- 获取角色分页数据
