# Vue-Cli创建项目报错

创建项目时报错

```
npm ERR! code EPERM
npm ERR! syscall open
npm ERR! path D:\Node\node_cache\_cacache\tmp\ed56322d
npm ERR! errno -4048
npm ERR! Error: EPERM: operation not permitted, open 'D:\Node\node_cache\_cacache\tmp\ed56322d'
npm ERR!  [Error: EPERM: operation not permitted, open 'D:\Node\node_cache\_cacache\tmp\ed56322d'] {
npm ERR!   errno: -4048,
npm ERR!   code: 'EPERM',
npm ERR!   syscall: 'open',
npm ERR!   path: 'D:\\Node\\node_cache\\_cacache\\tmp\\ed56322d'
npm ERR! }
npm ERR!
npm ERR! The operation was rejected by your operating system.
npm ERR! It's possible that the file was already in use (by a text editor or antivirus),
npm ERR! or that you lack permissions to access it.
npm ERR!
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator.

npm ERR! Log files were not written due to an error writing to the directory: D:\Node\node_cache\_logs
npm ERR! You can rerun the command with `--loglevel=verbose` to see the logs in your terminal
 ERROR  Error: command failed: npm install --loglevel error --legacy-peer-deps
Error: command failed: npm install --loglevel error --legacy-peer-deps
    at ChildProcess.<anonymous> (D:\Node\node_global\node_modules\@vue\cli\lib\util\executeCommand.js:138:16)        
    at ChildProcess.emit (node:events:513:28)
    at cp.emit (D:\Node\node_global\node_modules\@vue\cli\node_modules\cross-spawn\lib\enoent.js:34:29)
    at maybeClose (node:internal/child_process:1091:16)
    at ChildProcess._handle.onexit (node:internal/child_process:302:5)
```

## 解决方法

去node.js下载文件夹，更改node_cache和node_global文件夹权限，将User权限更改为完全控制



# vite项目打包后二级页面刷新报错

## 问题描述

使用vite开发项目后，运行`vue run build`打包部署上线后，在二级路由的页面时进行刷新，会有报错：

```
Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
```

## 解决方法

原因是vite关于静态资源路径配置不对，要修改`vite.config.js`文件中的`base`的值

## 最终代码

```js
export default defineConfig({
  base: "/",
  plugins: [
    vue(),
  ],
  server: {
    hmr: true,
    port: 3001,
    proxy: {
      '/api': {
        target: "http://kbws.xyz/", //目标代理接口
        secure: false,
        changeOrigin: true, //开启代理，在本地创建一个虚拟服务器
        pathRewrite: {
          '^/api': '/api',
        }
      }
    }
  },
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url)),
    }
  }
})
```

