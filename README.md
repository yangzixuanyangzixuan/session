引入express
const express=require("express");
连接数据库
const mongo = require("./tools/mongo");
引入body-parser模块
const bodyParser=require("body-parser");
引入用户的路由
const userRouter=require("./router/user");
const cookieParser=require("cookie-parser");
const expressSession=require("express-session");

配置服务器
const app=express();
配置bodyParser中间件
//app.use(bodyParser());或是使用以下
app.use(bodyParser.urlencoded({extended:false}));
app.use(bodyParser.json());
app.use(cookieParser());
app.use(expressSession({
    resave:false,
    saveUninitialized:false,
    secret:"hello"
}));


配置静态资源
app.use(express.static("public"));
配置模板
app.set("view engine","ejs");
app.set("views","views");



配置一个路由 是每一个页面都能直接使用session的值
app.use((req,res,next)=>{
    res.locals.session=req.session;
    //console.log(res.locals.session);
    next();

})

配置路由,通过userRouter找到userRouter对象
app.use("/user",userRouter);
创建一个更目录的目录
app.get('/',(req,res)=>{
    res.redirect("/user/login")

});

配置一个处理404的中间件
app.use((req,res)=>{
    当请求到达最后一个中间件时，证明浏览器地址肯定是填写错了
    可以定位到404页面
    res.status(404);
    res.render("404.ejs")

})

对服务器设置端口监听
app.listen(3000,()=>{
    console.log("服务器已经启动")
})

