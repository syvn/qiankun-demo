<template>
    <div id="main-root">
        <div v-if="loading">loading</div>
        <div id="nav"
             v-else>
            <button @click="clickHandle">主页面状态修改</button>
            <div @click="pageChange('/app1/home')">app1</div>
            <div @click="pageChange('/app2/home')">app2</div>
        </div>
        <!-- 子应用 -->
        <div id="root-view"
             class="app-view-box"
             v-html="content"></div>
    </div>
</template>

<script>
import actions from '@/shared/actions';

export default {
    name: 'App',
    props: {
        loading: Boolean,
        content: String,
    },
    data() {
        return {
            count: 0,
        };
    },
    methods: {
        pageChange(url) {
            this.routerGo(url);
        },
        routerGo(href = '/', title = null, stateObj = {}) {
            window.history.pushState(stateObj, title, href);
        },
        clickHandle() {
            this.count++;
            actions.setGlobalState({ count: this.count });
        },
    },
    mounted() {
        // 注册一个观察者函数
        actions.onGlobalStateChange((state, prevState) => {
            // state: 变更后的状态; prevState: 变更前的状态
            console.log('主应用：状态改变前的值为 ', prevState.count);
            console.log('主应用：状态发生改变，改变后的值为 ', state.count);
        });
    },
};
</script>
<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

#nav {
    padding: 30px;
}

#nav a {
    font-weight: bold;
    color: #2c3e50;
}

#nav a.router-link-exact-active {
    color: #42b983;
}
#nav {
    padding: 30px;
    display: flex;
    justify-content: center;
}
#nav div {
    margin: 0 10px;
    font-weight: bold;
    font-size: 30px;
    color: #42b983;
    cursor: pointer;
}
</style>
