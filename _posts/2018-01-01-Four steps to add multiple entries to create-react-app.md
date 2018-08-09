---
layout:     post
title:      Four steps to add multiple entries to create-react-app
subtitle:   Add multiple entries.
date:       2018-2-11
author:     f.achilles
header-img: 
catalog: true
tags:
    - React.js
---

After created a react app with create-react-app, there will be only one entry that is index.html. Sometimes, we want much more pure pages, such as: admin.html, detail.html and so on in one project. So,we need multiple entries in on project. And try follow tutorial.

## 1. Enject the project

```
npm run eject
```

## 2. Add multiple entry to webpack.config.dev.js

```
entry: {  
    index: [
      require.resolve('react-dev-utils/webpackHotDevClient'),
      require.resolve('./polyfills'),
      require.resolve('react-error-overlay'),
      paths.appIndexJs,
    ],
    admin:[
      require.resolve('react-dev-utils/webpackHotDevClient'),
      require.resolve('./polyfills'),
      require.resolve('react-error-overlay'),
      paths.appSrc + "/admin.js",
    ]
},
output: {
    path: paths.appBuild,
    pathinfo: true,
    filename: 'static/js/[name].bundle.js',
    chunkFilename: 'static/js/[name].chunk.js',
    publicPath: publicPath,
    devtoolModuleFilenameTemplate: info =>
      path.resolve(info.absoluteResourcePath),
}

```

## 3.Modify HtmlWebpackPlugin

Add a new plugin node:

```
new HtmlWebpackPlugin({
    inject: true,
    chunks: ["index"],
    template: paths.appHtml,
}),
new HtmlWebpackPlugin({
    inject: true,
    chunks: ["admin"],
    template: paths.appHtml,
    filename: 'admin.html',
})
```

## 4.Webpack Dev/Prod Server

Rewrite urls in /config/webpack.config.dev.js or /config/webpack.config.prod.js

```
historyApiFallback: {
    disableDotRule: true,
    rewrites: [
        { from: /^\/admin.html/, to: '/build/admin.html' },
    ]
}
```