<template>
    <div class="home">
        app1
        <div style="margin-bottom: 50px">状态值: {{count}}</div>

        <button @click="childClickHandle">子页面状态修改</button>
    </div>
</template>

<script>
import actions from '@/shared/actions';

export default {
    name: 'Home',
    components: {},
    data() {
        return {
            count: null,
        };
    },
    mounted() {
        console.log(actions, 'actions');
        actions.onGlobalStateChange((state) => {
            const { count } = state;

            console.log('子应用监听状态变化: ' + count);
            this.count = count;
        }, true);
    },
    methods: {
        childClickHandle() {
            this.count++;
            actions.setGlobalState({ count: this.count });
        },
    },
};
</script>
