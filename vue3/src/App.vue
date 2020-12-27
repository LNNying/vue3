<template>

    <ul>
        <li v-for="(item,index) in state.stu" :key="index" @click="removeStu(item.id)">
            {{item.name}}
        </li>
    </ul>
</template>

<script>

    import {ref, reactive} from 'vue';

    export default {
        name: 'App',
        setUp() {
            // ref 只能监听基本数据类型  不能监听对象
            // reactive 监听对象类型

            let {state, removeStu} = useRemove();
            return {state, removeStu};
        }

    }

    // 组合化API  解决了vue2中数据与业务逻辑分散的问题
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
