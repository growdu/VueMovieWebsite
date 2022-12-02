<template>
  <div>
    <div>
      <user-message></user-message>
    </div>
    <div>
      <Row>
        <i-col span="2" offset="0">
          <admin-menu></admin-menu>
        </i-col>
        <i-col span="20" offset="1">
          <div class="box">
          <div style="width: 800px;padding-top: 6%; alignment: center">
            <h2>添加课程</h2>
            <Divider />
              <table>
                <tr>
                  <td>课程名称：</td>
                  <td>
                    <i-input type="text" v-model="movieName" placeholder="请输入课程名称" autofocus  size="large" clearable style="width: 600px">
                      <Icon type="ios-person" slot="prepend"></Icon>
                    </i-input>
                  </td>
                </tr>
                <tr>
                  <td>课程图片地址：</td>
                  <td>
                    <i-input type="text" v-model="movieImg" placeholder="请输入课程图片链接" size="large" clearable>
                      <Icon type="md-camera" slot="prepend"></Icon>
                    </i-input>
                  </td>
                </tr>
                <tr>
                  <td>课程预告片地址：</td>
                  <td>
                    <i-input type="text" v-model="movieVideo" placeholder="请输入课程预告片链接"  size="large" clearable>
                      <Icon type="ios-videocam" slot="prepend"></Icon>
                    </i-input>
                  </td>
                </tr>
                <tr>
                  <td>课程播放地址：</td>
                  <td>
                    <i-input type="text" v-model="movieDownload" placeholder="请输入课程播放链接" size="large" clearable>
                      <Icon type="md-play" slot="prepend"></Icon>
                    </i-input>
                  </td>
                </tr>
                <tr>
                  <td>课程上传日期：</td>
                  <td>
                    <i-input type="date" v-model="movieTime" placeholder="请输入课程上传日期" size="large" clearable>
                      <Icon type="md-calendar" slot="prepend"></Icon>
                    </i-input>
                  </td>
                </tr>
<!--                <tr>-->
<!--                  <td>课程点赞次数：</td>-->
<!--                  <td>-->
<!--                    <i-input type="text" v-model="movieNumSuppose" placeholder="请输入课程点赞次数（数字）" size="large" clearable number>-->
<!--                      <Icon type="md-thumbs-up" slot="prepend"></Icon>-->
<!--                    </i-input>-->
<!--                  </td>-->
<!--                </tr>-->
<!--                <tr>-->
<!--                  <td>课程观看次数：</td>-->
<!--                  <td>-->
<!--                    <i-input type="text" v-model="movieNumDownload" placeholder="请输入课程观看次数（数字）" size="large" clearable number>-->
<!--                      <Icon type="md-calculator" slot="prepend"></Icon>-->
<!--                    </i-input>-->
<!--                  </td>-->
<!--                </tr>-->
                <tr style="height: 40px;">
                  <td>是否在主页显示该课程：</td>
                  <td>
                    <RadioGroup v-model="movieMainPage" size="large">
                      <Radio label="false">否</Radio>
                      <Radio label="true">是</Radio>
                    </RadioGroup>
                  </td>
                </tr>
                <br>
                <tr>
                  <td colspan="2">
                    <i-button type="success" v-on:click=addMovie() style="margin-left: 30px; width: 100px">添加</i-button>
                  </td>
                </tr>
              </table>
          </div>
          </div>
        </i-col>
      </Row>
    </div>
    <div>
      <common-footer></common-footer>
    </div>
  </div>
</template>

<script>
import MovieIndexHeader from '../components/MovieIndexHeader'
import CommonFooter from '../components/CommonFooter'
import UserMessage from '../components/UserMessage'
import AdminMenu from '../components/AdminMenu'
export default {
  components: {AdminMenu, UserMessage, CommonFooter, MovieIndexHeader},
  data () {
    return {
      movieName: '',
      movieImg: '',
      movieVideo: '',
      movieDownload: '',
      movieTime: '',
      // movieNumSuppose: 0,
      // movieNumDownload: 0,
      movieMainPage: 'false'
    }
  },
  methods: {
    addMovie: function (event) {
      // 添加课程到数据库
      let movie_data;
      if (typeof (sessionStorage.username) !== 'undefined') {
        movie_data = {
          username: sessionStorage.username,
          token: sessionStorage.token,
          user_id: sessionStorage._id,
          movieName: this.movieName,
          movieImg: this.movieImg,
          movieVideo: this.movieVideo,
          movieDownload: this.movieDownload,
          movieTime: this.movieTime,
          movieMainPage: this.movieMainPage
        }
        console.log(movie_data)
        this.$http.post('http://localhost:3000/admin/movieAdd', movie_data).then((addData) => {
          if (addData.body.status === 0) {
            this.$Modal.success({
              title: addData.body.message
            })
            this.movieName = ''
            this.movieImg = ''
            this.movieVideo = ''
            this.movieDownload = ''
            this.movieTime = ''
          } else {
            this.$Modal.error({
              title: addData.body.message
            })
          }
        })
      } else {
        this.$Modal.error({
          title: '请先登录'
        })
      }
    }
  }
}
</script>

<style lang="css" scoped>
  .box{
    display: flex;
    justify-content: center;
    align-items: center;
    padding-top: 10px;
    font-size: 16px;
  }
  td{
    height: 50px;
  }
</style>
