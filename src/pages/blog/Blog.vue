<template>
    <div id="main">
        <TopBar />
        <div id="content">
            <div id="blogHead">
                <BlogHead :markdownFileContent="markdownFileContent" />
            </div>
            <div id="blogContent">
                <BlogContent :markdownFileContent="markdownFileContent" />
            </div>
        </div>
    </div>
</template>

<script>
import TopBar from "../../components/TopBar";
import BlogHead from "../../components/BlogHead";
import BlogContent from "../../components/BlogContent";

export default {
    name: "Blog",
    data() {
        return {
            markdownFileContent: null,
            markdownFilePath: ""
        };
    },
    components: {
        TopBar,
        BlogHead,
        BlogContent
    },
    created() {
        let url = window.location.href;
        let reg = /\?.+/;
        let parmas = reg.exec(url)[0] ? reg.exec(url)[0] : null;
        if (parmas) {
            parmas = parmas.slice(1).split("&");
            this.markdownFilePath =
                "./blogs/" + parmas[0].slice(5) + "/" + parmas[1].slice(9);
        }
        this.getMarkdownFileContent();
    },
    mounted() {},
    methods: {
        async getMarkdownFileContent() {
            try {
                this.markdownFileContent = await (
                    await fetch(this.markdownFilePath)
                ).text();
            } catch (e) {
                console.log("获取博文markdown失败");
                throw e;
            }
        }
    }
};
</script>

<style lang="less" scoped>
#main {
    width: 100vw;
    
    #content {
        width: 80vw;
        margin: 0 10vw;
        margin-top: 3rem;
    }
}
</style>
