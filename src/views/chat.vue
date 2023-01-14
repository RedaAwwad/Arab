<template >
  <div class="blogItem">
    <!-- <app-nav-inner /> -->

    <!-- <appNavUser v-if="userAuth" :imageSrc="acc.image" />
    <appNav v-if="!userAuth" /> -->

    <div class="container">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">الرئيسية </el-breadcrumb-item>
        <el-breadcrumb-item><a href="">مستخدم </a></el-breadcrumb-item>
        <el-breadcrumb-item
          ><a href="" v-if="post && post.title">{{
            post.title
          }}</a></el-breadcrumb-item
        >
      </el-breadcrumb>
      <h2 v-if="post && post.title">{{ post.title }}</h2>

      <div v-if="post" class="row">
        <div class="col-lg-8">
          <div class="main-content">
            <div v-if="user" class="userInfo">
              <div class="img-box">
                <img :src="user.image" alt="" />
              </div>
              <div class="text-end">
                <p v-if="user" v-html="user.pio">
                  Lorem ipsum dolor sit amet consectetur adipisicing elit.
                </p>
                <h4 v-if="user">{{ user.name }}</h4>
                <h4 v-if="user.country">{{ user.country.title }}</h4>
                <h4 v-if="user">{{ user.name }}</h4>
                <h4 v-if="user">{{ user.email }}</h4>
              </div>
            </div>
            <div class="comments">
              <el-form ref="form" :model="form" label-width="120px">
                <!-- <h3>الرسائل</h3>        
                <el-divider></el-divider> -->

                <h3>جميع الرسائل:</h3>


                  <div v-if="allMassages" class="massages text-end">
                  <div
                    class="massage"
                    v-for="item in allMassages"
                    :key="item.id"
                  >
                    <div class="sender">
                      <div class="img-box">
                        <img :src="item.sender.image" alt="" />
                      </div>
                      <div class="">
                        <span>{{ item.sender.name }}</span>
                        <h6>{{ item.sender.type }}</h6>
                      </div>
                    </div>

                    <div class="massageBox">
                    <p class="massageContent" v-html="item.message"></p>
                    <span>{{ item.time }}</span>

                    </div>




                    <!-- <p v-html="msg"></p> -->


                    <el-divider></el-divider>
                  </div>

                </div>

                <div  class="massages text-end">
                  <div
                    class="massage"
                    v-for="item in chatObj"
                    :key="item.id"
                  >
                    <div class="sender">
                      <div class="img-box">
                        <img :src="item.image" alt="" />
                      </div>
                      <div class="">
                        <span>{{ item.name }}</span>
                        <h6>{{ item.type }}</h6>
                      </div>
                    </div>

                    <div class="massageBox">
                    <p class="massageContent" v-html="item.message"></p>
                    <span>{{ item.time }}</span>

                    </div>




                    <!-- <p v-html="msg"></p> -->


                    <el-divider></el-divider>
                  </div>

                </div>

                <p v-if="!allMassages">لا توجد رسائل حتى الأن ؟</p>

                <!-- <ckeditor
                  :editor="editor"
                  v-model="body"
                  :config="editorConfig"
                  class="mt-4"
                >
                </ckeditor> -->

                <textarea class="form-control" placeholder="ادخل الرسالة" name="" id="" v-model="body" cols="30" rows="10"></textarea>

                <el-form-item>
                  <el-button type="primary" @click.prevent="send"
                    >أرسل</el-button
                  >
                </el-form-item>
              </el-form>
            </div>
          </div>
        </div>

        <div class="col-lg-4">
          <div class="sideBar">

            <h5>جميع المحادثات</h5>

            <p v-if="messages.length == 0">لا يوجد محادثات بعد </p>

            <div class="" v-if="messages">
                            <div class="allMassages pointer" v-for="item in messages" :key="item.user_id">
                  <div @click.prevent="chat(item.mention)">
                       <div class="userMassageId d-flex" >
                      <div class="img-box">
                        <img :src="item.image" alt="">
                      </div>

                      <div class="item-info">
                        <h4>{{item.user_name}}</h4>
                        <p>{{item.user_type}}</p>

                      </div>
                    </div>
                  </div>
              </div>
            </div>

            <!-- <div class="markerter-item">
              <div class="img-cover">
                <img src="../assets/back.png" alt="" />
                <img :src="post.user_info.image" class="img-man" alt="" />
              </div>

              <div class="item-body">
                <h5 v-if="post && post.user_info.name">
                  {{ post.user_info.name }}
                </h5>
                <div class="stars">
                  <el-rate
                    v-model="post.user_info.rate"
                    disabled
                    show-score
                    text-color="#ff9900"
                    score-template="{value}"
                  >
                  </el-rate>
                </div>
                <div class="info">
                  <div class="massege">
                    <i class="fa-regular fa-message"></i>
                  </div>

                  <div class="user">
                    <i class="fa-regular fa-user"></i>
                  </div>

                  <div class="star">
                    <i class="fa-solid fa-star"></i>
                  </div>
                </div>
              </div>
            </div> -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>





<script>
import Vue from "vue";
import appNavInner from "@/components/navComponentInner.vue";
import axios from "axios";
// import VueAppend from 'vue-append'

// import myform from "@/main"

// Vue.use(VueAppend);



export default {
  name: 'ChatPage',
  components: {
    appNavInner,
  },
  data() {
    return {
      value: 5,
      body: null,

      // editor: ClassicEditor,

      editorConfig: {},
      post: {},
      RealetedBlog: {},
      plogId: null,
      allMassages: {},
      user: undefined,

      msg: undefined,
      html: "",
      all_message:undefined,
      chatObj:[],
      messages:undefined
    };
  },
  created() {
    this.$pusher.subscribe('chat-user', (channel) => {
      channel.bind('ChatUser', (data) => {
        console.log(`data: ${data}`);
      });
    });
  },
  mounted() {


  // });
  //         this.channel = this.$pusher.subscribe('chat-user');
  //     this.channel.bind('ChatUser', data => {
  //       // this.chatObj.push({
  //       //   ...obj,
        
  //       // });
  //       console.log('55555'+data);

  //       //   this.chatObj.push({
  //       //   message: obj.message,
  //       //   time: obj.time,
  //       //   id: obj.id,
  //       //   name:obj.user_name,
  //       //   image:obj.user_image,
        
  //       // });
  //       });


      var channel = this.$pusher.subscribe('chat-user');
      channel.bind('ChatUser', obj => {
        console.log('000000000000000'+chatObj);
      this.chatObj.push({
        ...obj,
        
      });
    });

    const receiver_id = this.$route.params.id;

    axios
      .get(
        `https://dashboard.arabicreators.com/api/get_markter/${receiver_id}`,
        {
  headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }
      )
      .then((res) => {
        this.user = res.data.data;
        localStorage.setItem("mainId", this.user.id);
        localStorage.setItem("mainIdNew", this.user.id);
      });



          this.axios
      .get(
        "https://dashboard.arabicreators.com/api/all_message",{
                    headers: {
            "Content-Type": "multipart/form-data",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }

      )
      .then((res2) => {
        this.messages = res2.data.data;


        

      });

    this.Massages();

  },
  methods: {
      send() {

       const receiver_id = localStorage.getItem("mainIdNew");

      const comment = new FormData();
      comment.append("message", this.body);
      comment.append("receiver_id", receiver_id);

      this.axios
        .post(
          "https://dashboard.arabicreators.com/api/send_messsage",
          comment,
          {
            headers: {
              "Content-Type": "multipart/form-data",
              Authorization: "Bearer " + localStorage.getItem("token"),
            },
          }
        )
        .then((resComment) => {
          this.msg = resComment.data.data.message;

              this.chatObj.push({
        message: resComment.data.data.message,
        time: resComment.data.data.time,
        id: resComment.data.data.id,
        name:resComment.data.data.sender.name,
        image:resComment.data.data.sender.image,
        type:resComment.data.data.sender.type,
      });
        });

      this.body = '';

      // console.log(this.chatObj)


    },

    chat(mention){
        this.$router.push(`/chat/${mention}`)
        this.$router.go()
    },

    massageAppend(){
        this.chatObj.push({
        massage: "",
        time: "",
        id: this.id,
      });
      
    },

    Massages() {
      let urlMassage = `https://dashboard.arabicreators.com/api/all_message_between_user/${localStorage.getItem(
        "mainId"
      )}/${localStorage.getItem("userId")}`;

      axios
        .get(urlMassage,{
          headers: {
            "Content-Type": "multipart/form-data",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((res3) =>{
          this.allMassages =  res3.data.data;
        });
    },

    sendComment() {
      this.Massages();

      const receiver_id = localStorage.getItem("mainIdNew");

      const comment = new FormData();
      comment.append("message", this.body);
      comment.append("receiver_id", receiver_id);

      this.axios
        .post(
          "https://dashboard.arabicreators.com/api/send_messsage",
          comment,
          {
            headers: {
              "Content-Type": "multipart/form-data",
              Authorization: "Bearer " + localStorage.getItem("token"),
            },
          }
        )
        .then((resComment) => {
          this.msg = resComment.data.data.message;


        this.chatObj.push({
        message: resComment.data.data.message,
        time: resComment.data.data.time,
        id: resComment.data.data.id,
        name:resComment.data.data.sender.name,
        image:resComment.data.data.sender.image,
        type:resComment.data.data.sender.type,
      });

     
      this.body=' '

      this.send()
          // alert(this.msg)

          // if (resComment.data.code == 200) {
          //   // this.$message({
          //   //   showClose: true,
          //   //   message: "تمت الاضافة بنجاح",
          //   //   type: "success",
          //   // });

          // } else if (resComment.data.code == 400) {
          //   this.$message.error("حدث خطأ !");
          // }
        });

         
    },
  },
};
</script>



<style lang="scss" scoped>
.sideBar {
  height: 700px;
  width: 100%;
  background-color: rgb(255, 255, 255);
  box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  padding: 15px;

  h5{
    margin-bottom: 20px;
    text-align: right;
  }

  .pointer{
    cursor: pointer;
  }

  .userMassageId{

    display: flex;
    margin-bottom: 10px;
    border: 1px solid rgb(226, 226, 226);
    padding: 15px;
    border-radius: 10px;
    align-items: center;

    .img-box{
      width: 50px;
      height: 50px;
      border: 2px solid black;
      border-radius: 50%;
      img{
        height: 100%;
        width: 100%;
      }
    }

    .item-info{
      text-align: right;
      margin-right: 10px;
      h4{
        margin-bottom: 0px;
        font-size: 20px;
      }

      p{

      }
    }

  }
}

.blogItem {
  direction: rtl;
  font-family: "Tajawal", sans-serif;
  background-color: rgb(243, 243, 243);

  h2 {
    text-align: right;
    font-size: 40px;
    color: var(--darkColor);
    margin-bottom: 30px;
    font-weight: 600;
    position: relative;
  }

  .el-breadcrumb {
    display: flex;
    justify-content: start;
    margin-bottom: 15px;
    padding-top: 20px;

    .el-breadcrumb__item {
      font-size: 18px;
    }
  }

  .markerter-item {
    position: relative;
    box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
    padding: 10px;
    border-radius: 15px;
    width: 70%;
    .img-cover {
      position: relative;
      top: 0;
      height: 200px;
      width: 100%;
      margin-bottom: 15px;

      img {
        position: absolute;
        top: 0;
        right: 0;
        height: 100%;
        width: 100%;
        filter: sepia(1);
      }

      .img-man {
        right: 10%;
        width: 75%;
        filter: sepia(0);
      }
    }

    .item-body {
      h5 {
        font-weight: 700;
        margin-bottom: 15px;
      }

      .stars {
        margin-bottom: 15px;
        .fa-star {
          color: gold;
          margin-left: 6px;
        }
      }

      .info {
        display: flex;
        justify-content: center;
        align-items: center;

        div {
          color: white;
          background: var(--darkColor);
          border-radius: 2px;
          padding: 6px 16px;
          margin: 8px;
        }
      }
    }
  }

  .comments {
    // background-color: #fdfdfd;
    border-radius: 5px;
    padding: 15px;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
    margin-bottom: 20px;
    background: white;

    .el-form-item {
      .el-form-item__content {
        margin-left: unset !important;
      }
    }
    .el-button {
      width: 100%;
      background: var(--darkColor);
      height: 50px;
      margin: 30px auto;
    }

    .allComments {
      display: flex;

      .sort {
        display: flex;
      }
    }

    .massage {
      margin-bottom: 10px;
      .sender {
        display: flex;
        justify-content: start;
        align-items: center;
        margin-bottom: 10px;

        .img-box {
          width: 60px;
          height: 60px;
          border-radius: 50%;
          overflow: hidden;
          margin-left: 10px;

          img {
            width: 100%;
            height: 100%;
          }
        }

        span {
          font-weight: 700;
          color: var(--darkColor);
          font-size: 20px;
        }

        h6 {
          font-size: 13px;
        }
      }
    }
    h3 {
      margin-bottom: 40px;
      font-weight: 700;
      font-size: 20px;
      text-align: right;
    }
    .massages {
      //   height: 400px;
      //   overflow-y: auto;
      margin-bottom: 40px;

      .massage{
        width: 100%;
      }


      .massageBox{
        display: flex;
        justify-content: space-between;
      }

      .massageContent {
        padding: 0;
        margin-bottom: 0 !important;
        font-weight: 700;
        p {
          margin-bottom: 0 !important;
        }
      }

    }
  }
}

a {
  text-decoration: none;
  color: #535151;
}

.userInfo {
  display: flex;
  justify-content: start;
  align-items: center;
  margin-bottom: 40px;
  box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  padding: 30px;
  background: white;

  .img-box {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    overflow: hidden;
    margin-left: 20px;
    img {
      width: 100%;
      height: 100%;
    }
  }
}
</style>

<style lang="scss">
.el-form-item__content {
  margin-left: unset !important;
}

.ck.ck-editor {
  width: 95%;
}
</style>