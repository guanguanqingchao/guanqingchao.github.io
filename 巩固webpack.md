### webpack 基础

#### 基本概念

    缓存原理：见

    bundle
    chunk
    
    tree shaking:
    
    hash：跟整个项目的构建相关，只要项目里有文件更改，整个项目构建的hash值都会更改，并且全部文件都共用相同的hash值
    chunkhash：根据不同的入口文件(Entry)进行依赖文件解析、构建对应的chunk，生成对应的哈希值。不同入口及其依赖hash值一样。公用的库hash单独。如果入口main.js中引用了style.css[如果使用MiniCssExtractPlugin提取css，[chunkhash].css，那么二者的包hash一致.如果main文件改变了 ，那么css的hash也会变
    contenthash：设置为contenthash，保证只要css内容不变，就不会重复构建
    
    runtime:在浏览器运行过程中，webpack 用来连接模块化应用程序所需的所有代码。它包含：在模块交互时，连接模块所需的加载和解析逻辑。
    manifest:管理模块之间的交互，可以追踪所有模块到输出 bundle 之间的映射，当 compiler 开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为 "manifest"，当完成打包并发送到浏览器时，runtime 会通过 manifest 来解析和加载模块。
    
    
  
    
    
#### 示例文件


#### 原理


    Webpack 的构建流程可以分为以下三大阶段。
    • 初始化:启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler。
    • 编译:从 Entry 发出，针对每个 Module 串行调用对应的 Loader 去翻译文件的内容， 再找到该 Module 依赖的 Module，递归地进行编译处理 。
    • 输出:将编译后的 Module 组合成 Chunk，将 Chunk 转换成文件，输出到文件系统中 。
    
    
    
    
   
  


#### 自己动手实现插件和loader

     Compiler对象包含了 Webpack环境的所有配置信息，包含 options、loaders、plugins 等信息。这个对象在 Webpack 启动时被实例化，它是全局唯一的，可以简单地将 它理解为 Webpack 实例。
            
    Compilation对象包含了当前的模块资源、编译生成资源、变化的文件等。当 Webpack 以开发模式运行时，每当检测到一个文件发生变化，便有一次新的 Compilation 被创建 。 Compilation 对象也提供了很多事件回调供插件进行扩展。通过 Compilation也能读取到 Compiler对象。

    Compiler和 Compilation的区别在于: Compiler代表了整个 Webpack从启动到关闭的生 命周期，而 Compilation 只代表一次新的编译。
    
    /**
    *广播事件
    * event-name 为事件名称，注意不要和现有的事件重名 
    * params 为附带的参数
    */
    compiler.apply ('event-name', params);
    
    /**
    *监听名称为 event-name 的事件，当 event-name 事件发生时，函数就会被执行 。 
    *同时函数中的 params 参数为广播事件时附带的参数 。
    */
    compiler.plugin ('event-name', function (params) {
    } ) 
    
    基本框架
    
    class BasicPlugin {
        constructor(options) {
            this.options = options
        }

        //Webpack 会调用 BasicPlugin 实例的 apply 方法为插件实例传入 compiler 对象
        apply(compiler) {

            console.log('############', this.options)

            //1. 读取输出资源、代码块、模块及真依赖
            compiler.plugin('emit', function (compilation, cb) {
                //compilation.chunks存放代码块,id name等

                compilation.chunks.forEach(chunk => {

                    chunk.files.forEach(filename => {
                        // compilation.assets 存放当前即将输出的所有资源 
                        //compilation.assets 是一个键值对，键为需要输出的文件名称，值为文件对应的内容。
                        //调用一个输出资源的 source ()方法能获取输出 资源的内容
                        let source = compilation.assets[filename].source();
                        // console.log('%%%%%%%%%%%%%%%%', source)


                    })

                })


                cb()

            })

            //2. 监听文件的变化
            //当依赖的文件发生变化时会触发 watch-run 事件
            compiler.plugin('watch-run', (watching, cb) => {
                //获取发生变化的文件列表
                const changedFiles = watching.compiler.watchFileSystem.watcher.mtimes;

                //changedFiles 格式为键值对,键为发生变化的文件路径 。 
                if (changedFiles[filePath] !== undefined) {
                    //filePath 对应的文件发生了变 化 callback() ;
                }
                cb();

            })


            //3. 修改输出资源
            // compiler.plugin('emit', (compilation, cb) => {


            //     compilation.assets[fileName] = {
            //         //返回文件内容
            //         source: () => {
            //             return fileContent
            //         },
            //         //返回文件大仙
            //         size: () => {
            //             return Buffer.byteLength(fileContent, 'utf8');
            //         }
            //     };
            //     cb()
            // })

            // compiler.plugin('emit', (compilation, callback) => {
            //     // 读取名称为 fileName 的输出资源
            //     const asset = compilation.assets[fileName];
            //     // 获取输出资源的内容
            //     asset.source();
            //     //获取输出资源的文件大小 
            //     console.log('&&&&&&&&&&', asset.size());
            //     callback();
            // });
        }



    }

    module.exports = BasicPlugin;
    
    
    
    示例：
    const path = require('path')
    const fs = require('fs')
    const fse = require('fs-extra')
    const jsonFormat = require('json-format');
    const dateFormat = require('dateformat');
    const shell = require('shelljs');
    const chalk = require('chalk')

    const includeResource = {
      ext: [ 'html', 'css', 'js' ], // 除此后缀名集合
      otherLimit: 10 * 1024, // 小于10kb的图片
    }

    const assetsPath = (_path) => {
      // return path.resolve(__dirname, _path)
      shell.cd('~')
      let basePath = shell.pwd().stdout
      return path.join(basePath + '/Desktop', _path)
    }


    class BundleZipPlugin{
      constructor(options){
        let projectName = options.projectName
        this.options = options
        let currentPath = shell.pwd().stdout
        this.rootOutputFolder = assetsPath(`pkg/${projectName}/`)
        shell.cd(currentPath)
        this.resourceOutputFolder = this.rootOutputFolder + 'resource/'
        this.count = 0 // 遍历文件计数器
        this.options.exclude = this.options.exclude || [] 
      }


      apply (compiler) {
        let cb = (compilation, callback) => {
            this.createWebBundleFolder(compilation.assets)
            this.count += this.options.srcArray.length
            this.options.srcArray.forEach((_path) => {
            this.judgeFile(_path)
          })
          callback();
        }
        // console.log(compiler.hooks)
        if(compiler.hooks){
          compiler.hooks.afterEmit.tapAsync('BundleZipPlugin', cb)
        }else{
            compiler.plugin("after-emit", cb);
        }


      }

      checkInclude (assets, assetName){
        var ext = assetName.split('.').pop()
        var size = assets[assetName].size()
        if(~includeResource.ext.indexOf(ext))
          return true
        return size < includeResource.otherLimit
      }

      prefixUrl (assetName){
        var ext = assetName.split('.').pop()
        if(ext === 'html') return this.options.pageUrl + assetName
        else return this.options.staticUrl + assetName
      }

      createWebBundleFolder (assets) {
        var assetUrls = []

        Object.keys(assets).forEach(assetName=>{
          if( this.checkInclude(assets, assetName) ){
            assetUrls.push(this.prefixUrl(assetName))
          }
        })
        fse.removeSync(this.rootOutputFolder)
        fse.removeSync(`${assetsPath(`pkg/${this.options.projectName}`)}.zip`)
        fse.ensureDirSync(this.resourceOutputFolder)
        var bundleJson = {
          page: this.options.projectName,
          version: dateFormat(new Date(), "yyyymmddHHMM"),
          resource: assetUrls.map(url=>({
            url,
            file: url.split('/').pop()
          }))
        };
        bundleJson.resource.push(...(this.options.urlArray || []))
        fs.writeFileSync(path.join(this.rootOutputFolder, 'bundle.json'), jsonFormat(bundleJson), 'utf8')
      }

      judgeFile (srcPath){
        // console.log(this.options.exclude[0],srcPath)
        let excFile = this.options.exclude.some((excPath) => {
          return srcPath.indexOf(excPath) > -1  
        })
        if(excFile){
          --this.count
          return
        }

        fs.stat(srcPath, (err, stats) => {
          if(err){
            throw err
          }
          if(stats.isFile()){
            let targetPath = path.resolve(this.resourceOutputFolder, srcPath.split('/').pop())
            this.copyFile(srcPath, targetPath)
          }
          else if(stats.isDirectory()){
            --this.count
            this.copyFolder(srcPath)
          }
        })
      }
      copyFolder (srcDir) {
        fs.readdir(srcDir, (err, files) =>{
          if(err){
            throw err
          }
          this.count += files.length
          files.forEach((file) => {
            this.judgeFile(path.resolve(srcDir, file))
          })
        })
      }

      // 赋值文件
      copyFile (fromPath, toPath) {
        var readStream = fs.createReadStream(fromPath),
            writeStream = fs.createWriteStream(toPath);

        readStream.on('error', (err) => {
          console.log(err)
          err && console.log('read error', fromPath)
        })
        writeStream.on('error', (err) => {
          console.log(err)
          err && console.log('write error', toPath)
        })
        writeStream.on('close', () => {
          --this.count
          // console.log(--this.count)
          !this.count && this.zipBundle(this.options.projectName)
        })
        readStream.pipe(writeStream)

      }
      zipBundle (projectName) {
        shell.cd(this.rootOutputFolder)
        shell.cd('..')
        shell.exec(`zip -r -X ${projectName}.zip ${projectName}`)
        this.options.publishGit && this.publishBundle(projectName)
      }
      // 上传zip包
      publishBundle (projectName) {
        if (!shell.which('git')) {
          shell.echo('Sorry, this script requires git');
          shell.exit(1);
        }

        let currentPath = shell.pwd().stdout
        let destPath = '~/Desktop'
        shell.cd(destPath)
        shell.rm('-rf', destPath + '/h5-bundles')
        shell.exec('git clone git@git.xiaojukeji.com:global/h5-bundles.git')
        shell.cd('h5-bundles')
        // shell.exec('git checkout -b dev6')
        console.log(chalk.yellow('copy bundle zip to h5-bundles.git'))
        let destDir = this.options.projectType == 'pax' ? 'offlinebundles-pax' : 'offlinebundles-drv'
        shell.cp('-rf', `${currentPath}/${projectName}.zip`, `${destPath}/h5-bundles/${destDir}`)
        shell.exec(`git add . ; git commit -m 'update ${projectName}'`)
        shell.exec('git push origin master')
        let dateTag = `${projectName}-${dateFormat(new Date(), "yyyymmddHHMM")}`
        shell.exec(`git tag ${dateTag}`)
        shell.exec(`git push origin ${dateTag}`)
        shell.rm('-rf', destPath + '/h5-bundles')
      }
    }

    module.exports = BundleZipPlugin
    
    
    new BundleZipPlugin({
      projectName: 'invite_friend111',
      projectType: 'pax', // pax 乘客端 || drv 司机端
      srcArray:[ path.resolve(__dirname, '../dist/mgm/share/'), path.resolve(__dirname, '../dist/assets/js/vue/vue~all.js')],
      exclude: [path.resolve(__dirname, '../dist/mgm/share/static/js')],
      pageUrl:'https://page.didiglobal.com/global/hulk/mgm/share/',
      staticUrl:'https://static.didiglobal.com/global/hulk/mgm/share/',
      urlArray:[
        {
          "url": "https://page.99taxis.mobi/global/hulk/mgm/share/index.html",
          "file": "index.html"
        },
        {
          "url": "https://page.99app.com/global/hulk/mgm/share/index.html",
          "file": "index.html"
        },
        {
          url:'https://static.didiglobal.com/global/share.global.min.js',
          file: 'share.global.min.js'
        },
        {
          url: 'https://static.didiglobal.com/global/hulk/assets/js/vue/vue~all.js',
          file: 'vue~all.js'
        }
      ],
      publishGit: true

    })




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
            
            


            

    

