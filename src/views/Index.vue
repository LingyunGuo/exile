<template>
  <div>
    <Row type="flex" justify="center" v-if="!portable">
      <iCol span="20">
        <Row type="flex" justify="space-between">
          <iCol span="17" class="wrapper-card">
            <div class="title-topic">
              <Row type="flex" align="middle">
                <iCol>
                  <div class="title-topic-icon">
                    <img src="../assets/icons/bookmark.svg" />
                  </div>
                </iCol>
                <iCol span="3">
                  <span>热门同人</span>
                </iCol>
                <iCol offset="17">
                  <Button type="info" size="small" ghost>
                    <b>
                      <Icon type="md-refresh" />换一换
                    </b>
                  </Button>
                </iCol>
              </Row>
            </div>
            <Row>
              <iCol span="6" v-for="article in articleList" :key="article.id" style="padding: 6px;">
                <div class="article-card">
                  <div class="card-deco card-deco-top"></div>
                  <div class="card-textarea">
                    <div class="card-quote card-quote-top">“</div>
                    <div class="card-article">
                      <div class="card-article-title"><router-link :to="'/article/'+article.aid" style="color:#515a6e">{{article.article_title}}</router-link></div>
                      <div class="card-article-content">
                        <p>{{filterText(article.chapter_content)}}</p>
                      </div>
                    </div>
                    <div class="card-quote card-quote-bottom">”</div>
                  </div>
                  <div class="card-footer">
                    <Row type="flex" align="middle" style="height: 100%;">
                      <iCol offset="1">
                        <span>
                          <Icon type="md-person" />
                          &nbsp;<router-link :to="'/self/'+article.article_author+'/info'" style="color: #515a6e;">{{article.user_nickname}}</router-link>
                        </span>
                      </iCol>
                    </Row>
                  </div>
                  <div class="card-deco"></div>
                </div>
              </iCol>
            </Row>
          </iCol>
          <iCol span="6" class="wrapper-card">
            <div class="title-sub">
              <Row type="flex" align="middle" style="margin-bottom:1em;">
                <iCol>
                  <span>CP墙</span>
                </iCol>
                <iCol offset="13">
                  <Button type="info" size="small" ghost>
                    <b>
                      <Icon type="md-refresh" />换一换
                    </b>
                  </Button>
                </iCol>
              </Row>
            </div>
            <Row>
              <iCol>
                <Button
                  v-for="cp in cpList"
                  v-bind:key="cp.id"
                  size="small"
                  class="tag"
                  @click="jumpCPSerch(cp.cp_name)"
                >{{cp.cp_name}}</Button>
              </iCol>
            </Row>
          </iCol>
        </Row>
        <Row type="flex" justify="space-between">
          <iCol span="17" class="wrapper-card">
            <div class="title-topic">
              <Row type="flex" align="middle">
                <iCol>
                  <div class="title-topic-icon">
                    <img src="../assets/icons/tinder.svg" />
                  </div>
                </iCol>
                <iCol span="3">
                  <span>热门大大</span>
                </iCol>
                <iCol offset="17">
                  <Button type="info" size="small" ghost>
                    <b>
                      <Icon type="md-refresh" />换一换
                    </b>
                  </Button>
                </iCol>
              </Row>
              <Row :gutter="16" type="flex" justify="space-between">
                <iCol span="12" v-for="topUser in userList.slice(0,2)" :key="topUser.id">
                  <Row>
                    <Card>
                      <Row slot="title">
                        <iCol span="3">
                          <Avatar :src='topUser.user_avatar_url' shape="square" icon="ios-person" />
                        </iCol>
                        <iCol span="20" class="wrapper-popular-user-card">
                          <p class="popular-user-card-title" @click="$router.push({name:'SelfInfo',params:{uid:topUser.uid}})">
                            {{topUser.user_nickname}}
                            <Badge text="Lv.1" type="info" style="margin-left:8px;z-index:1" />
                            <span></span>
                          </p>
                          <span class="popular-user-card-motto">{{topUser.user_signature}}</span>
                        </iCol>
                      </Row>
                      <Button slot="extra" type="info" @click="followUser(topUser.uid)">{{topUser.followed?'已关注':'关注'}}</Button>
                      <Row type="flex" justify="space-between">
                        <iCol span="11">
                          <div
                            class="article-poster"
                            :style="{'background-image':'url('+'https://chinocdn.cafuchino.cn/pic/202003071245654.png'+')'}"
                          ></div>
                        </iCol>
                        <iCol span="11">
                          <div
                            class="article-poster"
                            :style="{'background-image':'url('+'https://chinocdn.cafuchino.cn/pic/202003071245654.png'+')'}"
                          ></div>
                        </iCol>
                      </Row>
                    </Card>
                  </Row>
                </iCol>
              </Row>
            </div>
          </iCol>
          <iCol span="6" class="wrapper-card">
            <div class="title-sub">
              <Row type="flex" align="middle" style="margin-bottom:2em;">
                <iCol>
                  <span>人气关注</span>
                </iCol>
                <iCol offset="11">
                  <Button type="info" size="small" ghost>
                    <b>
                      <Icon type="md-refresh" />换一换
                    </b>
                  </Button>
                </iCol>
              </Row>
            </div>
            <Card v-for="user in userList" :key="user.uid" style="cursor: pointer">
              <Row type="flex" align="middle">
                <iCol span="4">
                  <Avatar shape="square" icon="ios-person" />
                </iCol>
                <iCol span="13">
                  <span @click="jumpUser(user.uid)">{{user.user_nickname}}</span>
                </iCol>
                <iCol span="5">
                  <Button type="info" ghost @click="followUser(user.uid)">{{user.followed?'已关注':'关 注'}}</Button>
                </iCol>
              </Row>
            </Card>
          </iCol>
        </Row>
        <Row type="flex" justify="space-between">
          <iCol span="24" class="wrapper-card">
            <div class="title-topic">
              <Row type="flex" align="middle">
                <iCol>
                  <div class="title-topic-icon">
                    <img src="../assets/icons/chat.svg" />
                  </div>
                </iCol>
                <iCol span="3">
                  <span>热门专题</span>
                </iCol>
                <iCol offset="18">
                  <Button type="info" size="small" ghost>
                    <b>
                      <Icon type="md-refresh" />换一换
                    </b>
                  </Button>
                </iCol>
              </Row>
            </div>
            <Row :gutter="16" type="flex" justify="space-between" style="overflow:hidden">
              <Spin fix style="border-radius: 18px;">开发中——敬请期待</Spin>
              <iCol span="9">
                <div
                  class="popular-topic-poster"
                  :style="{'background-image':'url('+'https://chinocdn.cafuchino.cn/pic/20200307134897.png'+')'}"
                >
                  <div class="popular-topic-content">
                    <Row type="flex" align="bottom" style="height:100%;">
                      <iCol>
                        <div class="popular-topic-lable">
                          <p class="popular-topic-lable-text">时之歌——光与暗注定交汇</p>
                        </div>
                      </iCol>
                    </Row>
                  </div>
                </div>
              </iCol>
              <iCol span="5">
                <div
                  class="popular-topic-poster"
                  :style="{'background-image':'url('+'https://chinocdn.cafuchino.cn/pic/202003071349487.png'+')'}"
                >
                  <div class="popular-topic-content">
                    <Row type="flex" align="bottom" style="height:100%;">
                      <iCol>
                        <div class="popular-topic-lable">
                          <p class="popular-topic-lable-text">从古至今，冷兵器的浪漫</p>
                        </div>
                      </iCol>
                    </Row>
                  </div>
                </div>
              </iCol>
              <iCol span="5">
                <div
                  class="popular-topic-poster"
                  :style="{'background-image':'url('+'https://chinocdn.cafuchino.cn/pic/202003071349487.png'+')'}"
                >
                  <div class="popular-topic-content">
                    <Row type="flex" align="bottom" style="height:100%;">
                      <iCol>
                        <div class="popular-topic-lable">
                          <p class="popular-topic-lable-text">狗崽精选同人文2</p>
                        </div>
                      </iCol>
                    </Row>
                  </div>
                </div>
              </iCol>
              <iCol span="5">
                <div
                  class="popular-topic-poster"
                  :style="{'background-image':'url('+'https://chinocdn.cafuchino.cn/pic/202003071349487.png'+')'}"
                >
                  <div class="popular-topic-content">
                    <Row type="flex" align="bottom" style="height:100%;">
                      <iCol>
                        <div class="popular-topic-lable">
                          <p class="popular-topic-lable-text">阴阳师BG向同人精选</p>
                        </div>
                      </iCol>
                    </Row>
                  </div>
                </div>
              </iCol>
            </Row>
          </iCol>
        </Row>
        <Row type="flex" justify="space-between">
          <iCol span="24" class="wrapper-card">
            <div class="title-topic">
              <Row type="flex" align="middle">
                <iCol>
                  <div class="title-topic-icon">
                    <img src="../assets/icons/bookmark-simple.svg" />
                  </div>
                </iCol>
                <iCol span="3">
                  <span>热门圈子</span>
                </iCol>
              </Row>
            </div>
            <Row :gutter="16">
              <iCol v-for="(fandom,index) in fandomList" :key="fandom.id" span="3">
                <Card class="fandom-card">
                  <Row justify="center">
                    <iCol span="20">
                      <Avatar
                        size="100"
                        style="color: #EAE0D0;background-color: #151F2F;font-weight: bold;"
                      ><span style="cursor: pointer;" @click="jumpSearchFandom(fandom.fandom_name)">{{fandom.fandom_name}}</span></Avatar>
                    </iCol>
                  </Row>
                  <div class="fandom-card-content">
                    <span style="cursor: pointer" @click="jumpSearchFandom(fandom.fandom_name)">{{fandom.fandom_name}}</span>
                    <span class="fandom-card-popularity">人气{{fandom.fandom_article_amount}}</span>
                    <Button :loading="fandom.loading" type="info" @click="followFandom(fandom.id,index)">{{fandom.followed?'取消关注':'关注圈子'}}</Button>
                  </div>
                </Card>
              </iCol>
            </Row>
          </iCol>
        </Row>
      </iCol>
    </Row>
    <Row v-if="portable">
      <iCol span="12" v-for="article in articleList" :key="article.id" style="padding: 6px;">
        <div class="article-card">
          <div class="card-deco card-deco-top"></div>
          <div class="card-textarea">
            <div class="card-quote card-quote-top">“</div>
            <div class="card-article">
              <div class="card-article-title"><router-link :to="'/article/'+article.aid" style="color:#515a6e">{{article.article_title}}</router-link></div>
              <div class="card-article-content" @click="$router.push('/article/'+article.aid)">
                <p>{{filterText(article.chapter_content)}}</p>
              </div>
            </div>
            <div class="card-quote card-quote-bottom">”</div>
          </div>
          <div class="card-footer">
            <Row type="flex" align="middle" style="height: 100%;">
              <iCol offset="1">
                <span>
                  <Icon type="md-person" />
                  &nbsp;<router-link :to="'/selfmobile/'+article.article_author+'/info'" style="color: #515a6e;">{{article.user_nickname}}</router-link>
                </span>
              </iCol>
            </Row>
          </div>
          <div class="card-deco"></div>
        </div>
      </iCol>
    </Row>
  </div>
</template>

<script>
import cookie from "js-cookie";
export default {
  data() {
    return {
      loading: {
        article: true,
        tag: true,
        user: true,
        fandom: true
      },
      cpList: [],
      userList: [],
      articleList: [],
      fandomList: [],
      addFandomLoading: false,
    };
  },
  computed: {
    portable() {
      return window.screen.width < 1024;
    },
    csrfToken() {
      return cookie.get("csrfToken");
    }
  },
  methods: {
    async getData() {
      const res = await this.$axios.get("/api/statistics/full");
      this.userList = res.data.user.slice(0, 4);
      this.articleList = res.data.article.slice(0, 4);
      this.cpList = res.data.cp;
      this.fandomList = res.data.fandom.slice(0, 8);
    },
    filterText(text) {
      const preg = /<("[^"]*"|'[^']*'|[^'">])*>/gm;
      return text.replace(preg, "").replace(/&nbsp;/gm, " ");
    },
    async jumpCPSerch(query){
      this.$router.push({
          path:'/article/searchresult',
          query:{
            fandom: '',
            title: '',
            author: '',
            relationship: query,
          }
        })
    },
    async followUser(uid) {
      const res = await this.$axios.post(
        "/api/follow/user",
        { uid: Number(uid) },
        { headers: { "x-csrf-token": this.csrfToken }, withCredentials: true }
      ).catch(err => {
        this.$Message.error("关注/取关失败");
      });
      if (res.data.result) {
        this.$Message.success("关注/取关成功！");
      }else{
        this.$Message.error("关注/取关失败");
      }
      this.getData();
    },
    async followFandom(fandom_id,index) {
      this.$set(this.fandomList[index],"loading",true);
      const res = await this.$axios.post(
        '/api/follow/fandom',
        {fandomId:fandom_id}
      ).catch(err => {
        this.fandomList[index].loading = false;
        this.$Message.error("关注/取关失败");
      });
      if (res.data.result) {
        this.fandomList[index].loading = false;
        this.$Message.success("关注/取关成功！");
      } else{
        this.fandomList[index].loading = false;
        this.$Message.error("关注/取关失败");
      }
      this.getData();
    },
    async jumpUser(id){
      console.log('jumpuser')
      const isMobile=this.$store.state.isMobile;
      if(isMobile){
        this.$router.push(`/selfmobile/${id}/info`)
      }else{
        this.$router.push(`/self/${id}/info`)
      }
    },
    async jumpSearchFandom(fandomName){
      this.$router.push({
        path:'/article/searchresult',
        query:{
          fandom:fandomName,
          relationship:'',
          title:'',
          author:'',
        }
      });
    },
  },
  mounted() {
    this.getData();
  }
};
</script>

<style scoped>
.wrapper-card {
  margin: 15px 0 15px 0;
  padding: 15px 30px 15px 30px;
  border: 1px solid rgb(226, 226, 226);
  background-color: #ffffff;
  box-shadow: 0px 10px 15px 12px rgba(208, 208, 208, 0.5);
  border-radius: 18px;
}
.title-topic {
  font-size: 20px;
  font-weight: 600;
}
.title-sub {
  font-weight: 600;
}
.article-card {
  background: #fff;
  border: 1px solid rgb(226, 226, 226);
  border-radius: 5px;
  overflow: hidden;
}
.card-textarea {
  height: 300px;
  display: flex;
  flex-direction: column;
  padding: 5px;
}
.card-article {
  padding: 5px;
}
.card-article-title {
  font-weight: bold;
  margin-bottom: 1em;
  display: block;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.card-article-content {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 10;
  overflow: hidden;
  font-size: 0.9em;
}
.card-footer {
  height: 35px;
  border-top: 1px solid rgb(226, 226, 226);
}
.card-quote {
  height: 30px;
  color: #a2d6cd;
  font-size: 40px;
  font-weight: 600;
  font-style: italic;
}
.card-quote-top {
  margin-bottom: auto;
}
.card-quote-bottom {
  margin-left: auto;
  margin-top: auto;
}
.card-deco {
  height: 5px;
  background: #a2d6cd;
}
.card-deco-top {
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}
.tag {
  margin: 8px;
}
.wrapper-popular-user-card {
  line-height: 10px;
}
.popular-user-card-motto {
  font-size: 0.5em;
  color: #828282;
}
.article-poster {
  height: 200px;
  width: 100%;
  background-size: cover;
  background-position: center;
}
.popular-user-card-title {
  cursor: pointer;
}
.popular-user-card-footer {
  height: 40px;
  border-top: 1px solid #dcdee2;
}
.popular-topic-poster {
  border-radius: 6px;
  width: 100%;
  height: 200px;
  background-size: cover;
  background-position: center;
}
.popular-topic-content {
  display: inline-block;
  margin: 5px;
  border-radius: 6px;
  border: 1px solid #fff;
  width: calc(100% - 12px);
  height: calc(100% - 12px);
}
.popular-topic-lable {
  position: relative;
  right: 1px;
  background: #fff;
  display: inline-block;
  padding: 5px 15px 5px 15px;
  border-radius: 2px;
  border-bottom-left-radius: 5px;
}
.popular-topic-lable-text {
  cursor: pointer;
}
.popular-topic-lable-text::before {
  content: " ";
  display: inline-block;
  position: relative;
  top: 3px;
  height: 17px;
  width: 3px;
  background-color: #f4b3cf;
  margin-right: 8px;
  border-radius: 3px;
}
.fandom-card {
  min-height: 180px;
}
.fandom-card-poster {
  background: url("../assets/posterBg.jpg");
  width: 100%;
  border-radius: 200px;
  box-shadow: 0 0 7px #13131385;
  padding-bottom: 100%;
  background-size: cover;
}
.fandom-card-poster-content {
  text-align: center;
  position: absolute;
}
.fandom-card-content {
  width: 100%;
  display: flex;
  text-align: center;
  line-height: 1.8em;
  flex-direction: column;
  justify-content: center;
  margin-top: 8px;
}
.fandom-card-popularity {
  color: #9e9e9e;
  font-size: 0.8em;
  margin-bottom: 1em;
}
.demo-spin-icon-load {
  animation: ani-demo-spin 1s linear infinite;
}
@keyframes ani-demo-spin {
  from {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(180deg);
  }
  to {
    transform: rotate(360deg);
  }
}
.demo-spin-col {
  height: 100px;
  position: relative;
  border: 1px solid #eee;
}
</style>
