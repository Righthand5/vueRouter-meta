# vueRouter-meta
vueRouter-meta关于学习VueRouter写的Demo
参考文献：  
<https://router.vuejs.org/zh/guide/advanced/meta.html>  
参考视频：bilibili👍山地人老师 
<https://www.bilibili.com/video/BV1Pt411e7aH?p=12>
# 关键代码分析：
``` javascript
const router = new VueRouter({
    routes:[
        {
            path:'/foo',
            component:Foo,
            children:[
                {
                    path:'bar',
                    component:Bar,
                    meta:{requiresAuth:true} //如何使用
                }
            ]
        },
        {
            path:"/login",
            component: Login
        }
    ]
})
const auth = {
    loggedIn(){
        return false;//初始化没有管理员权限
    }
}
router.beforeEach((to, from, next) => {
    if (to.matched.some(record => record.meta.requiresAuth)) { //当你进入带有meta的路径中
        if (!auth.loggedIn()) { //判断是否有管理员权限
            next({
                path: '/login', //如果没有（代码初始时为false）进入'/login'路径中
                query: { redirect: to.fullPath } //把要跳转的地址'/login'作为参数传到下一步
            })
        } else {
            next() //如果有管理员权限直接进入下个路径
        }
    } else {
        next() //如果没有meta直接进入下个路径
    }
})
```
