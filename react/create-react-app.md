https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-css-preprocessor-sass-less-etc

yarn eject: 把项目的配置文件eject出来，可修改配置

create-react-app: 增加scss支持：

https://medium.com/@Connorelsea/using-sass-with-create-react-app-7125d6913760

```js
{
   loader: require.resolve('file-loader'),
   // Exclude `js` files to keep "css" loader working as it injects
   // it's runtime that would otherwise processed through "file" loader.
   // Also exclude `html` and `json` extensions so they get processed
   // by webpacks internal loaders.
   exclude: [/\.(js|jsx|mjs)$/, /\.html$/, /\.json$/, /\.scss$/, /\.sass$/],
   options: {
     name: 'static/media/[name].[hash:8].[ext]',
   },
},
```
###方案
```js
    {
      test: /\.scss$/,
      include: paths.appSrc,
      loaders: ["style-loader", "css-loader", "sass-loader"]
    },
```

需要exclude 特定的后缀，==》 才能被其他的loader 处理
file-loader： 生成文件，输出到输出目录并返回 public URL，默认情况下，生成的文件的文件名就是文件内容的 MD5 哈希值并会保留所引用资源的原始扩展名。
