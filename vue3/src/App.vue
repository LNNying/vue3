<template>

    <ul>
        <li v-for="(item,index) in state.stu" :key="index" @click="removeStu(item.id)">
            {{item.name}}
        </li>
    </ul>
</template>

<script>

    /***
     * component API 与 option API(就是vue2 中data，methods中)
     * component API 可以和 option Api 混合一起使用
     * component API(组合或注入API) 本质  在运行的过程中 将component API 暴露出来的数据或方法注入到option API中
     * setUp 执行的时机： 在beforeCreate和create之间执行的
     * 注意： 执行setUp函数的时候还没有执行created所以在setUp中是没法使用data和methods中属性与方法的
     *       由于不能再setUp函数中使用data和methods，所以为了避免使用错误，vue直接将this设置成了undefined  不能用this
     *       setUp中的数据只能是同步的不能是异步
     * reactive：
     *     reactive是vue3中提供实现相应话数据的
     *     在vue2中是通过object.defineProperty实现
     *     在vue3中是通过Proxy实现
     *     本质：将传入的数据包装成一个proxy对象
     * reactive注意：
     *     reactive接收的数据必须是对象（json，arr）
     *     如果给reactive传递了其他对象
     *         默认情况下修改对象，单界面不会自动更新
     *         如果想更新，可以通过重新复制的方式 也就是属性值是一个对象  不能用属性值中set方法  必须用 = 赋值
     * ref：
     *    监听基本属性类型  也能监听对象  但是推荐用reactive监听对象
     *    本质：还是reactive 给ref方法传递一个值，ref会将其转化成reactive ref(18) => reactive({value: 18}) reactive 会默认添加一个属性value
     *    所以直接修改ref定义的变量不行(如 age = 12) 必须 age.value = 12
     *    注意：如果是通过ref创建的数据在<template> 不用通过.value获取 vue会自动添加.value获取数据
     *
     * ref与reactive的区别：
     *    用reactive创建的数据  在<template>中vue中不会自动添加.value获取  而ref则会自动添加.value获取数据
     *    vue中通过ref中私有属性 __v_isRef(boolean) 属性来区分是否为ref定义变量
     *    也可通过isRef ， isReactive 来判断是否为ref或reactive   用法isRef(age)
     *
     * 递归监听   嵌套多层对象
     *    默认情况下ref和reactive都是递归监听
     *    问题： 嵌套多层对象，非常消耗性能
     *
     * 非递归监听  值监听第一层  只有在使用的数据量比较大的时候才使用非递归监听
     *   reactive --->>  shallowReactive   如果第一层更新了 就会发生页面的重新渲染
     *   ref --->>  shallowRef  vue监听的是.value的变化  并不是第一层的变化 state.value = ....
     *   triggerRef --->> 根据传入的数据主动更新界面  vue3 只提供ref的方法  没有提供triggerReactive方法
     *
     *
     */
    import {ref, reactive, isRef, isReactive} from 'vue';

    export default {
        name: 'App',

        data() {
            return {
                msg: 'mg'
            }
        },
        methods: {
          clickBut() {
              alert('msg');
          }
        },


        setUp() {
            // ref 只能监听基本数据类型  不能监听对象
            // reactive 监听对象类型
            let age = ref(12);
            let {state, removeStu} = useRemove();
            // 递归监听
            let data = reactive({
                fg: {
                    name: 'li',
                    children: {
                        fg: {
                            name: 'child'
                        }
                    }
                }
            });
            data.fg.children.fg.name = 'newChild';
            return {state, removeStu, age};
        }

    }

    // 组合化API  解决了vue2中数据与业务逻辑分散的问题  利于进行模块化管理
    function useRemove() {
        let state = reactive({
            stu: [
                {id: 1, name: 'li'},
                {id: 2, name: 'lo'}
            ]
        });

        function removeStu(id) {
            state.stu = state.stu.filter(item => item.id !== id);
        }
        return {state, removeStu}
    }
</script>
