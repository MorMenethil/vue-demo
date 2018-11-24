<template>
    <div class="comment-container">
        <h2>发表评论</h2>
        <hr>
        <textarea placeholder="请输入您想BB的话(最多BB120字)" maxlength="120"></textarea>
        <mt-button type="primary" size="large">发表评论</mt-button>

        <div class="comment-list">
            <div class="comment-item" v-for="(item, index) in commentList" :key="index">
                <div class="comment-title">
                    第{{ index + 1 }}楼&nbsp;&nbsp;用户：{{ item.user_name }}&nbsp;&nbsp;发表时间：{{item.add_time | dateFormat}}
                </div>
                <div class="comment-content">
                    {{ item.content || "这个逼有点懒" }}
                </div>
            </div>
        </div>

        <mt-button type="danger" size="large" plain @click="getMore">加载更多</mt-button>
    </div>
</template>

<script>
    import { Toast } from "mint-ui"
    export default {
        props: ["id"],
        data() {
            return {
                pageIndex: 1,
                commentList: []
            }
        },
        created() {
            this.getComments()
        },
        methods: {
            getComments() {
                this.$http.get("api/getcomments/" + this.id + "?pageindex=" + this.pageIndex).then(result => {
                    if(result.body.status === 0) {
                        // this.commentList = result.body.message
                        this.commentList = this.commentList.concat(result.body.message)
                    } else {
                        Toast("数据获取失败！请重新获取！！")
                    }
                })
            },
            getMore() {
                this.pageIndex++,
                this.getComments()
            }
        }
    }
</script>

<style lang="less">
.comment-container {
    h2 {
        font-size: 18px;
        font-weight: 800;
    }
    textarea {
        font-size: 14px;
        height: 85px;
        margin: 0;
    }
  .comment-list {
    .comment-item {
      margin: 5px 0;
      .comment-title {
        font-size: 13px;
        background-color: #ccc;
      }
      .comment-content {
        font-size: 13px;
        line-height: 35px;
        text-indent: 2em;
      }
    }
  }
}
</style>