##  前几天看到了日期组件，但是样式和功能不满足现有功能的需求。所以就手动封装了一个组件

##  先是快速搭建vue运行环境

##  npm install -g @vue/cli-service-global
    npm install -g -g @vue/cli

    新建一个App.vue文件
    npm init -y 生成package.json文件
    在改文件目录下运行 vue serve

 ##   技术点总结：
      1.vue指令 v-click-outside 给组件绑定事件  
      然后判断点击当前是否包括当前元素，如果当前元素包含着点击事件，那么久触发日历显示

      2.日历组件首要判断的是 当前月份的1号是周几。再把周几前面的天数算出来。使得6行7列填充满。这样日历组件的核心久出来了。
      ^ _ ^
