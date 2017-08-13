# Webpack

## 前言

&emsp;&emsp;不知道是不是有些人和我一样，已经使用了很久的webpack,但还是对webpack一知半解，就觉得打包编译好用，在网上找个配置就可以使用。webpack经常和grunt，gulp混淆在一起，感觉就是替代grunt和glup的。但这种观点是不正确的。grunt和gulp更像是任务管理器，通过配置任务进行执行。而webpack是什么呢？

&emsp;&emsp;在webpack的官网，对webpack有一个最简单的定义，webpack就是一个模块打包器。处理的核心问题是代码的模块化，一种模块依赖关系的处理。这样其实就可以理解，为什么经常会有webpack和grunt配合使用的情况。webpack其实就是完成了其中的一项任务。

## 核心

webpack的配置有4大核心：

Entry：处理依赖关系的入口，想要将依赖的文件打包，必须指定打包的入口文件是什么。感觉类似爬虫给一个页面，然后抓取所有页面中的引用关系。

Output：有了输入，必须有输出，output执行打包后文件的输出位置，以及输出文件的名称

Loader：装载器，webpack实际上默认处理的都是js文件，对于css,html,scss等文件，webpack都会将其作为一个module处理，但是在这之前必须将其进行一个装载处理，相当于在其外面包上一层包装。这就是装载器的功能。在1.x版本中对于json也是需要进行装载处理的，在2.x中认为json已经是javascript中的一个功能，因此在2.x版本已经不需要对json进行处理了。

Plugin：插件，webpack相当于提供了一个接口，我们可以在plugin中完成个性化的需求

## 使用

webpack的安装和使用

1.  安装（用npm和yarn两种安装方式）

        npm i webpack --save-dev
        
        yarn add webpack --dev

2. 配置文件

在安装的目录下，执行webpack命令，默认查找webpack.config.js配置文件，这里我们新建一个配置文件webpack.config.js

        module.exports = {
            entry: {
                index: './src/index.js'
            },
            output: {
                path: __dirname + '/dist',
                filename: '[name]-[hash].js'
            },
            modules: {
                rules: [
                    {
                        test: /\.css/,
                        use: [
                            'style-loader',
                            'css-loader'
                        ]
                    }
                ]   
            },
            plugins: [
            ]
        }


3. 实际工作中遇到的问题

   1. 单页面应用和多页面应用，入口文件的处理
   
        entry有多种形式，简写，json，数组
        
        简写：单页面应用可用

            entry: './src/index.js'
        
        json：单页面和多页面应用都可用，可配置多个入口
        
            entry: {
                index: './src/index.js',
                main: './src/main.js'
            }
        
        数组：一般和json形式一起使用，主要是配合CommonsChunkPlugin插件使用的
        
            entry: {
                index: './src/index.js',
                vendor: ['./src/include/include1.js', './src/include/include2.js']
              //这两个文件是在index中引用的公用文件，可以单独抽取出去使用
            }

   2. 单页面应用打包文件太大，如何处理

    使用CommonsChunkPlugin，将共通文件进行打包处理，拆分成不同的文件分别引入。   

   3. 文件缓存的处理方式
   
    我们常用的处理方式是在文件名后面加hash，output支持这种命名方式，但是在引用的时候如何替换成对应的名称，可以使用htmlWebpackPlugin，生成对应的html文件
