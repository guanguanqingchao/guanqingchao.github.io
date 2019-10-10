### webpack 基础

#### 基本概念

    缓存原理：见

    bundle
    chunk
    
    
    hash：跟整个项目的构建相关，只要项目里有文件更改，整个项目构建的hash值都会更改，并且全部文件都共用相同的hash值
    chunkhash：根据不同的入口文件(Entry)进行依赖文件解析、构建对应的chunk，生成对应的哈希值。不同入口及其依赖hash值一样。公用的库hash单独。
              如果入口main.js中引用了style.css[如果使用MiniCssExtractPlugin提取css，[chunkhash].css，那么二者的包hash一致.如果main文件改变了 ，那么css的hash也会变
    contenthash：设置为contenthash，保证只要css内容不变，就不会重复构建
    
    runtime:在浏览器运行过程中，webpack 用来连接模块化应用程序所需的所有代码。它包含：在模块交互时，连接模块所需的加载和解析逻辑。
    manifest:管理模块之间的交互，可以追踪所有模块到输出 bundle 之间的映射
    当 compiler 开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为 "manifest"，当完成打包并发送到浏览器时，runtime 会通过 manifest 来解析和加载模块。
    
    
  
    
    
#### 示例文件


#### 原理

结合pdf 和https://juejin.im/user/59dc483e6fb9a0450e7511b4/posts



#### 利用webpack进行性能优化

一. 减少前端资源大小

        如果使用 webpack 4，启用生产模式，webpack会默认压缩代码
        最小化你的代码，通过 bundle-level 最小化和 loader 选项
        使用 ES 模块以启用 tree shaking，
        压缩图片
        启用特定依赖优化
        启用模块连接
        使用 externals，如果这对你有效果
    
    
二. 持久化缓存

        1. 代码懒加载: import() 在代码中所有被import()的模块，都将打成一个单独的包，放在chunk存储的目录下。在浏览器运行到这一行代码时，就会自动请求这个资源，实现异步加载。这样可以让 入口 bundle 变得更小，从而减少首次加载时间      
        例如：
        //main.js
        import('lodash').then(_ => {
            // Do something with lodash (a.k.a '_')...
            console.log(
                _.join(['Another', 'module', 'loaded!'], ' ')
            );
            
        })
        正常webpack会将lodash打包到vendor中，懒加载之后会单独打成一个包[2.b29baa1ffc307ac4709c.`bundle.js]，代码执行到import()的时候才会加载lodash这个包
        
        
        2. 将代码拆分为路由和页面: 
        单页面应用：配合路由来懒加载
        传统多页面：通过entry来拆分首页、其他功能页等。通过splitchunks来提取公用代码
        
        
        3. 保证id稳定 HashedModuleIdsPlugin只有当重命名或移动该模块时，模块的 ID 才会更改。新的模块也不会影响到其他模块的 ID。
        
        4. 缓存 bundle 并通过更改 bundle 名称来进行版本控制
        
        5. 将 bundle 拆分成 app（应用）代码、vendor（第三方库） 代码和 runtime
           runtime单独打包出来，每次代码变化的时候，vendor的hash也会变化 // runtimeChunk: true,
        
        6. 内联 runtime 可以节省 HTTP 请求  html-webpack-inline-source-plugin
        
            new HtmlWebpackPlugin({
                title: '管理',
                inlineSource: 'runtime~.+\\.js',  
            }),
            //内联嵌入runtime文件
            new InlineSourcePlugin(),
          
            <!-- index.html -->
            <script src="./runtime.79f17c27b335abc7aaf4.js"></script>
            
            <!-- index.html -->
            <script>
            !function(e){function n(r){if(t[r])return t[r].exports;…}} ([]);
            </script>
            
            


            

    

