> 下面是文件`vue.config.js`中的各项配置说明
> 参考地址：https://github.com/Believel/vue-cli/tree/dev/docs/zh/config

```js
const path = require("path");
const isPro = process.env.NODE_ENV === "production";

function resolve(dir) {
    return path.join(__dirname, dir);
}
module.exports = {
    baseUrl: "/vue/", // 部署应用时的根路径(默认'/'),也可用相对路径(存在使用限制)
    outputDir: "dist", // 运行时生成的生产环境构建文件的目录(默认''dist''，构建之前会被清除)
    assetsDir: "static", //放置生成的静态资源(s、css、img、fonts)的(相对于 outputDir 的)目录(默认'')
    indexPath: "index.html", //指定生成的 index.html 的输出路径(相对于 outputDir)也可以是一个绝对路径。
    pages: {
        //pages 里配置的路径和文件名在你的文档目录必须存在 否则启动服务会报错
        index: {
            //除了 entry 之外都是可选的
            entry: "src/main.js", // page 的入口,每个“page”应该有一个对应的 JavaScript 入口文件
            template: "public/index.html", // 模板来源
            filename: "index.html", // 在 dist/index.html 的输出
            title: "VueCli3.0", // 当使用 title 选项时,在 template 中使用：<title><%= htmlWebpackPlugin.options.title %></title>
            chunks: ["chunk-vendors", "chunk-common", "index"] // 在这个页面中包含的块，默认情况下会包含,提取出来的通用 chunk 和 vendor chunk
        }
        // subpage: 'src/subpage/main.js'//官方解释：当使用只有入口的字符串格式时,模板会被推导为'public/subpage.html',若找不到就回退到'public/index.html',输出文件名会被推导为'subpage.html'
    },
    lintOnSave: false, // 是否在保存的时候检查
    productionSourceMap: true, // 生产环境是否生成 sourceMap 文件
    css: {
        extract: true, // 是否使用css分离插件 ExtractTextPlugin
        sourceMap: false, // 开启 CSS source maps
        loaderOptions: {
            // 给 sass-loader 传递选项
            sass: {
                // @/ 是 src/ 的别名
                // 所以这里假设你有 `src/variables.scss` 这个文件
                data: `@import "@/assets/css/common.scss";@import "@/assets/css/mixin.scss";`
            }
        }, // css预设器配置项
        modules: false // 启用 CSS modules for all css / pre-processor files.
    },
    devServer: {
        // 环境配置
        host: "localhost",
        port: 8084,
        https: false,
        hotOnly: false,
        open: true, //配置自动启动浏览器
        proxy: {
            // 配置多个代理(配置一个 proxy: 'http://localhost:4000' )
            // 接口是'/api'开头的才用代理
            "/api": {
                target: "https://api.github.com",
                ws: true,
                changeOrigin: true
            },
            "/foo": {
                target: "<other_url>"
            }
        },
        // 提供在服务器内部的其他中间件之前执行自定义中间件的实例
        before: app => {
            // app是一个expres实例
        }
    },
    pluginOptions: {
        // 第三方插件配置
        // ...
    },
    // 是一个函数，会接收一个基于webpack-chain的ChainableConfig实例。允许对内部的webPack配置进行更细粒度的修改。
    chainWebpack(config) {
        config.resolve.alias.set("components", resolve("src/components"));
    },
    configureWebpack: config => {
        if (isPro) {
            return {
                plugins: [
                    new CompressionWebpackPlugin({
                        // 目标文件名称。[path] 被替换为原始文件的路径和 [query] 查询
                        asset: "[path].gz[query]",
                        // 使用 gzip 压缩
                        algorithm: "gzip",
                        // 处理与此正则相匹配的所有文件
                        test: new RegExp("\\.(js|css)$"),
                        // 只处理大于此大小的文件
                        threshold: 10240,
                        // 最小压缩比达到 0.8 时才会被压缩
                        minRatio: 0.8
                    })
                ]
            };
        }
    }
};
```
