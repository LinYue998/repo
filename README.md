# to-do

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

<!-- 此次demo为待办事项
        基础功能为:
            1.用户能够添加新的待办事项,
            2.用户能够删除未完成事项与完成事项,
            3.用户可将完成后的事项添加到完成事项中,
            4.用户能够给未完成事项通过时间进行排序,
        技术栈:
            此demo运用webpack搭建了vue框架,通过ElementUi进行页面的布局与设置,利用node的http搭建了一个简易的本地服务器,端口号为3000;
        技术难点:
            1.在首次将待办事项的值赋值给数组，通过localStorage保存到本地，在获取的时候如果返回为null,就会报错
            (解决办法):在获取本地值的时候,在最后面加上一个空数组,如果获取的值为空，就返回一个空数组 ---  ||[];
            2.在进行排序的时候，是通过时间戳来进行排序的，利用forEach循环数组的每一项，将其时间转为时间戳，排序以后，时间不能正常显示
            (解决办法):在排序完成以后，再通过ForEach将时间戳转为时间，再重新赋值，就能够正常显示
            3.未完成事项与完成事项的切换，只能从上到下依次执行，且点击一次后，下一个复选框必须点两次才能正常显示;如果随机选择，还是从第一个开始执行，且第一个的value值变为点击的value值
            (暂时还未解决)
 -->
 <!-- 加分项目因为时间原因，尚未完成，十分抱歉! -->
