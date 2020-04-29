---
title: 从零开始搭建后台管理系统（五）Utils、Mixins
date: 2020-03-19 09:52:27
categories:
- Ant Design
- [Vue,Axios]
tags:
- Vue
- Ant Design
- Axios
thumbnail: https://s2.ax1x.com/2019/12/31/l3AAeK.jpg
---
本项目基于 [Vue全家桶](https://cn.vuejs.org/v2/guide/) 和 [Ant Design](https://www.antdv.com/docs/vue/use-with-vue-cli-cn/) 进行开发。
>目录
>1. 安装脚手架工具（vue-cli）、创建新项目
>2. 引入 Ant Design
>3. 引入 Vue Router
>4. 引入 node-sass/sass-loader
>5. 登录页
>6. 完成页面的基本布局（ 根据路由渲染菜单组件、面包屑组件等 ）
>7. 使用 Vuex 完成菜单的显示隐藏
>8. Ajax 以及 Axios 的封装
>9. **全局方法 Utils、混合 Mixins**
>10. 后续功能开发
<!-- more -->

### **9. 全局方法 Utils、混合 Mixins**
#### **9.1. Utils**
项目中有一些方法很多地方都会用到，我们可以创建一个<span class="backgroundBlock">utils.js</span>文件存放这些方法：
```js src/utils.js
const STORAGE_ENGINE = localStorage;
// const STORAGE_ENGINE = sessionStorage;
const Utils = {
    // 在stroage中存/取数据
    setItem(key, val) {
        if (typeof val === 'object') {
            STORAGE_ENGINE.setItem(key, JSON.stringify(val))
        } else {
            STORAGE_ENGINE.setItem(key, val)
        }
    },
    getItem(key) {
        let ele = STORAGE_ENGINE.getItem(key);
        let val = null;
        try {
            val = JSON.parse(ele);
        } catch (error) {
            val = ele;
        }
        return val;
    },

    // 用户信息相关方法
    // 在stroage中存/取用户token
    setToken(token) {
        STORAGE_ENGINE.setItem('_token', token);
    },
    getToken() {
        return STORAGE_ENGINE.getItem('_token');
    },

    // 在stroage中存/取用户信息
    setUserInfo(userInfo) {
        STORAGE_ENGINE.setItem('_userInfo', userInfo);
    },
    getUserInfo() {
        return STORAGE_ENGINE.getItem('_userInfo');
    },
    
    // 清除stroage中数据
    clear() {
        STORAGE_ENGINE.clear();
    },
    
    //登出
    logout() {
        this.clear();
    },

    // 获取登录时存放的权限字段数组
    getPermission() {
        let val = [];
        try {
            val = JSON.parse(STORAGE_ENGINE.getItem('_permission_'));
        } catch (error) {
            val = [];
        }
        return val;
    },

    // 权限控制：根据权限字段返回Boolean值，用来判断路由或者页面中按钮的显示隐藏
    canVisit(name) {
        let permission = this.getPermission();
        if (!Array.isArray(permission)) {
            permission = [];
        }
        let isPermission = false
        let obj = null;
        obj = permission.find((item, index) => {
            return item.symbol.includes(name);
        });
        if (obj) {
            isPermission = true;
        };
        if (isPermission || name === 'index' || name === 'homeIndex') {
            return true;
        } else {
            return false;
        }
        // return true;
    },
    /**
     * 对象的深度拷贝
     * @param data 需要拷贝的元数据
     * @return {any} 返回拷贝后的新数据
     */
    deepClone(data) {
        const type = this.getType(data);
        let obj;

        if (type === 'array') {
            obj = [];
        } else if (type === 'object') {
            obj = {};
        } else {
            //不再具有下一层次
            return data;
        }
        if (type === 'array') {
            for (let i = 0, len = data.length; i < len; i++) {
                obj.push(this.deepClone(data[i]));
            }
        } else if (type === 'object') {
            for (let key in data) {
                obj[key] = this.deepClone(data[key]);
            }
        }
        const constructor = data.constructor;
        if (constructor) {
            return Object.assign(new constructor(), obj);
        }
        return obj;
    },
    numFormat(num) {
        var c = (num.toString().indexOf('.') !== -1) ? num.toLocaleString() : num.toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,');
        return c;
    },

    /**
     * 处理空的参数
     * @param data
     * @returns
     */
    cleanData(data) {
        let _data = {};
        for (let key in data) {
            if (Object.prototype.toString.call(data[key]) === '[object Object]') {
                _data[key] = null;
            } else if (data[key] instanceof Array) {
                _data[key] = [];
            } else {
                _data[key] = undefined;
            }
        }
        return _data;
    },

    /**
     * 判断对象类型
     * @param {Object} object
     * @return {String} object type
     */
    getType(object) {
        var toString = Object.prototype.toString;
        var map = {
            '[object Boolean]': 'boolean',
            '[object Number]': 'number',
            '[object String]': 'string',
            '[object Function]': 'function',
            '[object Array]': 'array',
            '[object Date]': 'date',
            '[object RegExp]': 'regExp',
            '[object Undefined]': 'undefined',
            '[object Null]': 'null',
            '[object Object]': 'object'
        };
        if (object instanceof Element) {
            return 'element';
        }
        return map[toString.call(object)];
    },
    
    preventClose(e) {
        e = e || window.event;
        // 兼容IE8和Firefox 4之前的版本
        if (e) {
            e.returnValue = '关闭提示';
        }
        // Chrome, Safari, Firefox 4+, Opera 12+ , IE 9+
        return '关闭提示';
    },
    /**
     * 
     * @param {路由守卫next回调} next 
     * @param {vue} that 
     */
    showConfirm(next, that) {
        that.$confirm({
            title: '确定要离开当前页面吗？',
            content: '离开后您将取消本次编辑的全部内容，确定要离开吗？',
            cancelText: "取消",
            okText: '确定',
            centered: true,
            onOk() {
                next();
            },
            onCancel() {
                that.$router.go(1);
            },
            class: 'test',
        });
    },
    //附件url转换
    changefileUrl(url, str = '') {
        let arr = url ? url.split(',') : [];
        let arr1 = [];
        arr.map((ele, index) => {
            let one = ele.lastIndexOf('_');
            let two = ele.lastIndexOf('.');
            let newStr = ele.replace(ele.substring(one, two), '');
            let arr = newStr.split('/')
            let name = arr[arr.length - 1];
            arr1.push({
                uid: (index + 1) / -1 + '',
                name: name,
                url: ele,
                status: 'done'
            })
        })
        return arr1;
    },
};

export default Utils;
```
用的时候引入就可以了
```js
import Utils from "@/utils";
```

#### **9.2. Mixins**
像这类的后台管理系统，基本上每个模块都会有很多的列表页、表格页，会大量的使用到<span class="backgroundBlock">antd</span>的<span class="backgroundBlock">table</span>组件，我们完全可以把与之相关的一些属性和方法写到<span class="backgroundBlock">mixins.js</span>里。
```js src/mixins.js
export default {
    data() {
        return {
            searchObj: {},//搜索条件的对象
            pagination: {
                current: 1,
                pageSize: 10,
                total: 0,
                showTotal() {
                    return `共 ${this.total} 条数据`
                },
                showSizeChanger: true
            },
        }
    },
    methods: {
        // 搜索方法
        query() {
            this.$set(this.pagination, 'current', 1);
            this.$set(this.pagination, 'pageSize', 10);
            this.getList();
        },
        // 重置搜索字段方法
        reset() {
            this.$set(this.pagination, 'current', 1);
            this.$set(this.pagination, 'pageSize', 10);
            this.$refs.dp.reset();
            this.getList();
        },
        paginationChange(pagination, sorter) {
            this.pagination.current = pagination.current;
            this.pagination.pageSize = pagination.pageSize;
            this.getList();
        }
    }
}
```
使用的时候在页面注入就行了：
```js
<script>
    import mixins from "@/plugins/mixins";
    export default {
        mixins: [mixins],
        data() {
            return {
                //
            }
        }
    }
<script>
```

### **10. 后续功能开发**
经过前面的9个步骤，我们已经完成了下列工作：
* 创建了项目，并且引入了 **Ant Design、node-sass/sass-loader、Vue Router、Vuex、Axios** 等<span class="importantBlock">插件</span>。
* 完成了<span class="importantBlock">路由的搭建</span>和<span class="importantBlock">页面的布局</span>，包括一些<span class="importantBlock">组件的创建</span>（登录页、菜单、面包屑、头部组件等）
* 封装了<span class="importantBlock">Axios</span>和一些<span class="importantBlock">全局方法</span>。

项目的基本框架和方法封装已经完成，接下来功能模块的开发都是一些业务逻辑代码，只需要根据已有的模板，按照<span class="importantBlock">模块化、组件化</span>的思想继续开发就行了。

（完）