<template>
  <div class="container">
    <div class='toolbar'>
      <el-button type="primary" icon="el-icon-edit" size="medium" @click='changeMode' plain>{{edit ? '确认' : '编辑内容'}}</el-button>
      <el-button type="success" icon='el-icon-success' size="medium" @click='generageScreenShot' plain>生成截图</el-button>
<!--      <el-upload :show-file-list="false" action="" :on-success="handlePicSuccess" :before-upload="beforePicUpload">-->
<!--        <el-button type="warning" icon='el-icon-success' size="medium" plain>添加图片</el-button>-->
<!--      </el-upload>-->
    </div>
    <el-dialog title="" :visible.sync="dialogVisible" @opened='showImage'>
      <div id='image-container'>
      </div>
      <span slot="footer" class="dialog-footer">
        <a id="download" download="shuirong.png">
          <el-button type="primary" @click="download">下载图片</el-button>
        </a>
      </span>
    </el-dialog>

    <div class="tweet tweet-has-context stream-item twitter" id="TweetBox">
      <div class="context">
        <div class="tweet-context with-icn">
          <span class="Icon Icon--small Icon--heartBadge"></span>
          <span :contenteditable="edit">SOMEONE liked</span>
          <span id="watermark">什么是真相？fakes.netlify.com</span>
        </div>
      </div>

      <div class="content">
        <div class="stream-item-header">
            <div class="account-group">
              <el-upload v-if='edit' class="avatar-uploader-twitter-simple" action="" :show-file-list="false" :on-success="handleAvatarSuccess" :before-upload="beforeAvatarUpload">
                <img v-if="avatar" :src="avatar" class="avatar2" alt="">
                <i v-else class="el-icon-plus avatar-uploader-icon"></i>
              </el-upload>
              <img :src='avatar' class="avatar" alt=""/>
              <span class="FullNameGroup">
                <strong class="fullname u-textTruncate">
                  <a v-html='nickname' :contenteditable="edit"></a>
                </strong>
                <span class="UserBadges"></span>
                <span class="UserNameBreak">&nbsp;</span>
              </span>
              <span class="username u-dir u-textTruncate" dir="ltr">@<b v-html='username' :contenteditable="edit"></b></span>
            </div>
            <small class="time">
              <a class="tweet-timestamp" title="6:00 AM - 28 Mar 2019">
                <span class="_timestamp" aria-hidden="true" :contenteditable="edit">很久很久以前</span>
              </a>
            </small>
            <div class="ProfileTweet-action ProfileTweet-action--more">
              <div class="dropdown">
                <button class="ProfileTweet-actionButton u-textUserColorHover dropdown-toggle" type="button" aria-haspopup="true">
                  <div class="IconContainer" title="More">
                    <span class="Icon Icon--caretDownLight Icon--small"></span>
                  </div>
                </button>
              </div>
            </div>
          </div>

          <div>
            <p class="TweetTextSize js-tweet-text tweet-text" v-html='content' :contenteditable="edit"></p>
          </div>

          <div class="stream-item-footer">
            <div class="ProfileTweet-actionCountList u-hiddenVisually">
              <span class="ProfileTweet-action--reply u-hiddenVisually">
                <span class="ProfileTweet-actionCount">
                  <span class="ProfileTweet-actionCountForAria" :contenteditable="edit">34 replies</span>
                </span>
              </span>
              <span class="ProfileTweet-action--retweet u-hiddenVisually">
                <span class="ProfileTweet-actionCount">
                  <span class="ProfileTweet-actionCountForAria" :contenteditable="edit">18 retweets</span>
                </span>
              </span>
              <span class="ProfileTweet-action--favorite u-hiddenVisually">
                <span class="ProfileTweet-actionCount">
                  <span class="ProfileTweet-actionCountForAria" :contenteditable="edit">460 likes</span>
                </span>
              </span>
            </div>

            <div class="ProfileTweet-actionList js-actions" role="group" aria-label="Tweet actions">
              <div class="ProfileTweet-action ProfileTweet-action--reply">
                <button class="ProfileTweet-actionButton js-actionButton js-actionReply" type="button">
                  <div class="IconContainer js-tooltip" title="Reply">
                    <span class="Icon Icon--medium Icon--reply"></span>
                    <span class="u-hiddenVisually">Reply</span>
                  </div>
                  <span class="ProfileTweet-actionCount ">
                    <span class="ProfileTweet-actionCountForPresentation" aria-hidden="true">34</span>
                  </span>
                </button>
              </div>
              <div class="ProfileTweet-action ProfileTweet-action--retweet js-toggleState js-toggleRt">
                <button class="ProfileTweet-actionButton  js-actionButton js-actionRetweet" type="button">
                  <div class="IconContainer js-tooltip" title="Retweet">
                    <span class="Icon Icon--medium Icon--retweet"></span>
                    <span class="u-hiddenVisually">Retweet</span>
                  </div>
                  <span class="ProfileTweet-actionCount">
                    <span class="ProfileTweet-actionCountForPresentation" aria-hidden="true">18</span>
                  </span>
                </button>
              </div>
              <div class="ProfileTweet-action ProfileTweet-action--favorite js-toggleState">
                <button class="ProfileTweet-actionButton js-actionButton js-actionFavorite" type="button">
                  <div class="IconContainer js-tooltip" title="Like">
                    <span role="presentation" class="Icon Icon--heart Icon--medium"></span>
                    <div class="HeartAnimation"></div>
                    <span class="u-hiddenVisually">Like</span>
                  </div>
                  <span class="ProfileTweet-actionCount">
                    <span class="ProfileTweet-actionCountForPresentation" aria-hidden="true">460</span>
                  </span>
                </button>
              </div>
              <div class="ProfileTweet-action ProfileTweet-action--dm">
                <button class="ProfileTweet-actionButton u-textUserColorHover js-actionButton js-actionShareViaDM" type="button">
                  <div class="IconContainer js-tooltip" title="Direct message">
                    <span class="Icon Icon--medium Icon--dm"></span>
                    <span class="u-hiddenVisually">Direct message</span>
                  </div>
                </button>
              </div>
            </div>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
import html2canvas from "html2canvas";
import json from "./image.json";

export default {
  name: "TwitterSimple",
  data() {
    return {
      dialogVisible: false,
      edit: false,
      nickname: "鲁迅",
      username: "Lu Xun",
      avatar: json.avatar,
      reblogNumber: 99999,
      commentNumber: 99999,
      starNumber: 99999,
      from: "iPhone客户端",
      time: "1912-11-06 14:31",
      content: "我即使是死了，钉在棺材里了，也要在墓里，用这腐朽的声带喊出：“我没说过这句话”。",
      canvas: "",
      mentionPerson: "",
      isMentioned: false,
      picture: "",
      imageUrl: ""
    };
  },
  methods: {
    changeMode() {
      this.edit = !this.edit;
    },
    getBase64(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => resolve(reader.result);
        reader.onerror = error => reject(error);
      });
    },
    generageScreenShot() {
      let screenShot = document.querySelector("#TweetBox");
      let width = screenShot.offsetWidth;
      let height = screenShot.offsetHeight;
      html2canvas(screenShot, {
        allowTaint: true,
        useCORS: true,
        height: height,
        width: width
      }).then(canvas => {
        this.canvas = canvas;
        this.dialogVisible = true;
      });
    },
    showImage() {
      const dom = document.querySelector("#image-container");
      if (dom.childNodes.length) {
        dom.removeChild(dom.childNodes[0]);
      }
      dom.appendChild(this.canvas);
    },
    handleAvatarSuccess(res, file) {
      this.imageUrl = URL.createObjectURL(file.raw);
    },
    beforeAvatarUpload(file) {
      this.getBase64(file).then(res => {
        this.avatar = res;
      });
      return true;
    },
    handlePicSuccess(res, file) {
      this.picture = URL.createObjectURL(file.raw);
    },
    beforePicUpload(file) {
      this.getBase64(file).then(res => {
        this.picture = res;
        let image = new Image(167);
        image.src = this.picture;
        document.getElementById("DIV_27").appendChild(image);
        image.style = "display:block";
      });
    },
    download() {
      let download = document.getElementById("download");
      let image = document
        .querySelector("canvas")
        .toDataURL("image/png")
        .replace("image/png", "image/octet-stream");
      download.setAttribute("href", image);
    }
  }
};
</script>

<style scoped src='./style.css'></style>
<style lang='scss'>
.avatar-uploader-twitter-simple {
  .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409eff;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 45px;
    height: 45px;
    line-height: 45px;
    text-align: center;
  }
  .avatar2 {
    width: 45px;
    height: 45px;
    display: block;
  }
}
</style>
<style scoped lang='scss'>
.container {
  width: fit-content;
  margin: auto;
  padding: 20px 0;
}
.toolbar {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  justify-content: space-around;
}
#image-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
#watermark {
  -webkit-transform: rotate(-180deg);
  -moz-transform: rotate(-180deg);
}
</style>
