> 参考文档：nuxtjs
## 路由
#### 依据 <font color="#f56565">`views`</font> 目录结构自动生成 <font color="#00c58e">vue-router</font> 模块的路由配置。
<br />

## 基础路由
#### 假据 <font color="#f56565">`views`</font> 的目录结构如下：
```javascript
views/
--| user/
-----| index.vue
-----| one.vue
--| index.vue
```
#### 那么，自动生成的路由配置如下：
```javascript
router: {
  routes: [
    {
      path: "/",
      name: "index",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
          import(/* webpackChunkName: 'index' */ "@/views/index.vue"),
      children: []
    },
    {
      children: [],
      path: "/user",
      name: "user-index",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
          import(/* webpackChunkName: 'user' */ "@/views/user/index.vue")
    },
    {
      children: [],
      path: "/user/one",
      name: "user-one",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
          import(/* webpackChunkName: 'user' */ "@/views/user/one.vue")
    }
  ]
}
```
<br />

## 动态路由

#### 定义带参数的动态路由，需要创建对应的以下划线作为前缀的 Vue 文件 或 目录。
#### 以下目录结构：
```javascript
views/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```
#### 生成对应的路由配置表为：
```javascript
router: {
  routes: [
    {
      path: "/",
      name: "index",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(/* webpackChunkName: 'index' */ "@/views/index.vue"),
      children: []
    },
    {
      children: [],
      path: "/:slug/comments",
      name: "slug-comments",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(/* webpackChunkName: ':slug' */ "@/views/_slug/comments.vue")
    },
    {
      children: [],
      path: "/:slug",
      name: "slug-index",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(/* webpackChunkName: ':slug' */ "@/views/_slug/index.vue")
    },
    {
      children: [],
      path: "/users/:id?",
      name: "users-id",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(/* webpackChunkName: 'users' */ "@/views/users/_id.vue")
    }
  ]
}
```
#### 你会发现名称为 <font color="#f56565">`users-id`</font> 的路由路径带有 <font color="#f56565">`:id?`</font> 参数，表示该路由是可选的。如果你想将它设置为必选的路由，需要在 <font color="#f56565">`users/_id`</font> 目录内创建一个 <font color="#f56565">`index.vue`</font> 文件。
<br />

## 嵌套路由
#### 你可以通过 vue-router 的子路由创建 应用的嵌套路由。

#### 创建内嵌子路由，你需要添加一个 Vue 文件，同时添加一个与该文件同名的目录用来存放子视图组件。

#### 假设文件结构如：
```javascript
views/
--| users/
-----| _id.vue
-----| index.vue
--| users.vue
```

#### 自动生成的路由配置如下：
```javascript
router: {
  routes: [
    {
      children: [
        {
          children: [],
          path: ":id?",
          name: "users-id",
          meta: { title: "" },
          alias: "",
          redirect: "",
          component: () =>
            import(
              /* webpackChunkName: 'users' */ "@/views/users/_id.vue"
            )
        },
        {
          children: [],
          path: "",
          name: "users-index",
          meta: { title: "" },
          alias: "",
          redirect: "",
          component: () =>
            import(
              /* webpackChunkName: 'users' */ "@/views/users/index.vue"
            )
        }
      ],
      path: "/users",
      name: "users",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(/* webpackChunkName: 'users' */ "@/views/users.vue")
    }
  ]
}
```
<br />

## 动态嵌套路由

#### 这个应用场景比较少见，但是 我们仍然支持：在动态路由下配置动态子路由。

#### 假设文件结构如下：

```javascript
views/
--| _category/
-----| _subCategory/
--------| _id.vue
--------| index.vue
-----| _subCategory.vue
-----| index.vue
--| _category.vue
--| index.vue
```
#### 那么，自动生成的路由配置如下：

```javascript
router: {
  routes: [
    {
      path: "/",
      name: "index",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(/* webpackChunkName: 'index' */ "@/views/index.vue"),
      children: []
    },
    {
      children: [],
      path: "/:category/:category?",
      name: "category-:category",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(
          /* webpackChunkName: ':category' */ "@/views/_category/_category.vue"
        )
    },
    {
      children: [
        {
          children: [],
          path: ":id?",
          name: "category-:subCategory-:id",
          meta: { title: "" },
          alias: "",
          redirect: "",
          component: () =>
            import(
              /* webpackChunkName: ':category' */ "@/views/_category/_subCategory/_id.vue"
            )
        },
        {
          children: [],
          path: "",
          name: "category-:subCategory-index",
          meta: { title: "" },
          alias: "",
          redirect: "",
          component: () =>
            import(
              /* webpackChunkName: ':category' */ "@/views/_category/_subCategory/index.vue"
            )
        }
      ],
      path: "/:category/:subCategory?",
      name: "category-:subCategory",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(
          /* webpackChunkName: ':category' */ "@/views/_category/_subCategory.vue"
        )
    },
    {
      children: [],
      path: "/:category",
      name: "category-index",
      meta: { title: "" },
      alias: "",
      redirect: "",
      component: () =>
        import(
          /* webpackChunkName: ':category' */ "@/views/_category/index.vue"
        )
    }
  ]
}
```
