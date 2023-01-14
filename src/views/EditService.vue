<template>
  <div class="createConsultaion">
    <!-- <navbar /> -->
    <!-- <div v-if="show"  class="loader">
      <scale-loader :loading="loading" :color="color" :height="height" :width="width"></scale-loader>
    </div> -->

    <div class="content">
      <div class="container">
        <div class="header">
          <h2>تعديل الخدمة</h2>
          <p>ستتم مراجعة الخدمة فور انشاءها واعلامك باشعار اذا كانت متاحة لنشر</p>
        </div>

          <el-form
            ref="form"
            :model="form"
            label-width="120px"
            label-position="top"
            :rules="rules"
            @submit="addServise"
          >
            <el-form-item label="عنوان الخدمة  " prop="title">
              <el-input
                v-model="form.title"
                placeholder="أدخل العنوان "
              ></el-input>
            </el-form-item>
            <p class="tip">
              أدخل عنواناً واضحاً باللغة العربية يصف الخدمة التي تريد أن تقدمها.
              لا تدخل رموزاً أو كلمات مثل "حصرياً"، "لأول مرة"، "لفترة محدود"..
              الخ.
            </p>
            <el-form-item label="التصنيف">
              <el-select
                v-model="form.specialties"
                placeholder="اختر التصنيف"
                @change="serviceCategory"
              >
                <el-option
                  v-for="item in get_specialty"
                  :key="item.id"
                  :label="item.title"
                  :value="item.id"
                ></el-option>
              </el-select>
            </el-form-item>
            <p class="tip">
              أدخل عنواناً واضحاً باللغة العربية يصف الخدمة التي تريد أن تقدمها.
              لا تدخل رموزاً أو كلمات مثل "حصرياً"، "لأول مرة"، "لفترة محدود"..
              الخ.
            </p>

            <el-form-item label="نوع الخدمة ">
              <el-select
                v-model="form.types"
                multiple
                placeholder="اختر الخدمة"
              >
                <el-option
                  v-for="item in serviceCategories"
                  :key="item.id"
                  :label="item.title"
                  :value="item.id"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="الوصف" prop="description">
              <editor
                api-key="lq78n5tuhbwnunb6moxdzq6d2cyx8o895bfzepnwbw3mc29e"
                :init="{
                  directionality: 'rtl',
                  language: 'ar',
                  height: 500,
                  menubar: 'false',
                  plugins: [
                    'print preview paste importcss searchreplace autolink autosave save directionality code visualblocks visualchars fullscreen image link media template codesample table charmap hr pagebreak nonbreaking anchor toc insertdatetime advlist lists wordcount imagetools textpattern noneditable help charmap quickbars emoticons',
                  ],
                  toolbar:
                    'undo redo | bold italic underline strikethrough | fontselect fontsizeselect formatselect | alignleft aligncenter alignright alignjustify | outdent indent |  numlist bullist | forecolor backcolor removeformat | pagebreak | charmap emoticons | fullscreen  preview save print | insertfile image media template link anchor codesample | ltr rtl',
                  toolbar_sticky: true,
                  image_caption: true,
                  toolbar_mode: 'sliding',
                }"
                v-model="form.description"
                style="width: 100%"
              />
            </el-form-item>
            <p class="tip">
              أدخل عنواناً واضحاً باللغة العربية يصف الخدمة التي تريد أن تقدمها.
              لا تدخل رموزاً أو كلمات مثل "حصرياً"، "لأول مرة"، "لفترة محدود"..
              الخ.
            </p>

            <!-- <el-switch v-model="form.has_file"> </el-switch> -->

            <span class="text-end d-block fw-bold mb-3">صور الخدمة</span>
            <!-- <input type="file" @change="onfileSelected"  /> -->

            <button
              style="display: block; height: 35px; margin-bottom: 10px"
              class="fileInput1"
              @click.prevent="$refs.fileInput.click()"
            >
              اضغط هنا لتحميل صور الخدمة الرئيسية
            </button>
            <input
              type="file"
              style="display: none"
              ref="fileInput"
              multiple
              @change="onfileSelected"
            />

            <img class="img-preview" v-if="url" :src="url" alt="" />
            <img class="img-preview" v-if="url2" :src="url2" alt="" />

            <el-switch
              class="mt-4 mb-1 text-start"
              style="display: block"
              v-model="type2"
              active-color="#13ce66"
              @change="hideImage"
            >
            </el-switch>

            <div class="" :class="{ hide2: hide2 }">
              <span class="text-end d-block fw-bold mb-3">ملفات الخدمة </span>

              <button
                style="display: block; height: 35px; margin-bottom: 10px"
                class="fileInput1"
                @click.prevent="$refs.fileInput1.click()"
                :class="{ hide2: hide2 }"
              >
                اضغط هنا لتحميل ملفات الخدمة الرئيسية
              </button>
              <input
                type="file"
                style="display: none"
                ref="fileInput1"
                multiple
                @change="onfileSelected2"
              />
            </div>

            <el-switch
              class="mt-4 mb-1 text-start"
              style="display: block"
              v-model="type"
              active-color="#13ce66"
              @change="hideEffect"
            >
            </el-switch>

            <el-form-item label="رابط الخدمة " :class="{ hide: hide }">
              <el-input
                v-model="form.url"
                placeholder="أدخل رابط الخدمة  "
                :disabled="disabel"
              ></el-input>
            </el-form-item>

            <div class="d-flex">
              <div class="dvide">
                <el-form-item class="" label=" سعر الخدمة " prop="price">
                  <el-select v-model="form.price" placeholder="اختر التصنيف">
                    <el-option
                      v-for="item in price_for_servcie"
                      :key="item.price"
                      :label="item.price"
                      :value="item.price"
                    ></el-option>
                  </el-select>
                </el-form-item>
              </div>

              <div class="dvide">
                <el-form-item class="" label="مدة التسليم" prop="price">
                  <el-select v-model="form.time" placeholder="اختر التصنيف">
                    <el-option
                      v-for="item in time_for_servcie"
                      :key="item.day"
                      :label="item.title"
                      :value="item.day"
                    ></el-option>
                  </el-select>
                </el-form-item>
                <p class="tip">
                  أدخل عنواناً واضحاً باللغة العربية يصف الخدمة .. الخ.
                </p>
              </div>
            </div>

            <el-form-item label="كلمات مفتاحية " prop="title">
              <input-tag
                v-model="form.keywords"
                placeholder="أدخل الكلمة المفتاحية "
                add-tag-on-keys="13"
                class="blogInput"
              ></input-tag>
            </el-form-item>

            <el-form-item class="mt-4" label="تعليمات للمشتري">
              <el-input
                type="textarea"
                :autosize="{ minRows: 10, maxRows: 14 }"
                v-model="form.buyer_instructions"
              ></el-input>
            </el-form-item>
            <p class="tip">
              أدخل عنواناً واضحاً باللغة العربية يصف الخدمة التي تريد أن تقدمها.
              لا تدخل رموزاً أو كلمات مثل "حصرياً"، "لأول مرة"، "لفترة محدود"..
              الخ.
            </p>

          

            <div class="service-item" v-for="item in addmore" :key="item">
              <div class="">
                <div @click.prevent="reomveForm(item)" class="remove">
                  <i class="fa-solid fa-xmark del"></i>
                </div>

                <div class="ser-item-inner">
                  <el-form-item label="عنوان الاضافة">
                  <input
                    type="text"
                    v-model="item.title"
                    class="form-control"
                    style="height: 50px !important"
                  />
                </el-form-item>

                <el-form-item label="سعر الاضافة">
                  <el-select v-model="item.price" placeholder="اختر سعر ">
                    <el-option
                      v-for="item in price_for_exta_servcie"
                      :key="item.price"
                      :label="item.title"
                      :value="item.price"
                    ></el-option>
                  </el-select>
                </el-form-item>
                </div>

                <div class="ser-item-inner">
                  <el-form-item>
                    <el-select
                      v-model="item.has_time"
                      placeholder="هل سيزيد من مدة التنفيذ"
                    >
                      <el-option
                        v-for="item in has_time"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value"
                      ></el-option>
                    </el-select>
                  </el-form-item>

                  <el-form-item v-if="item.has_time == 1">
                    <el-select v-model="item.time" placeholder="مدة الزيادة">
                      <el-option
                        v-for="item in time_for_exta_servcie"
                        :key="item.day"
                        :label="item.title"
                        :value="item.day"
                      ></el-option>
                    </el-select>
                  </el-form-item>
                </div>
              </div>
            </div>

              <a @click.prevent="addForm" class="btn mt-4 w-25 me-auto addSer">
              أضف تطويراً لهذه الخدمة +
            </a>

            <el-button type="primary" @click.prevent="addServise('form')"
              >اضافة</el-button
            >
          </el-form>
      </div>
    </div>
  </div>
</template>


<script>
import Vue from "vue";
import Editor from "@tinymce/tinymce-vue";

import navbar from "@/components/navComponentInner.vue";
// import ClassicEditor from "@ckeditor/ckeditor5-build-classic";

export default {
  components: {
    navbar,

    editor: Editor,
  },

  data() {
    return {
      radio1: "red",
      textXlac: "0",
      // editor: ClassicEditor,
      // editorData: "<p>Content of the editor.</p>",
      // editorConfig: {},
      readonly: true,

      form: {
        title: localStorage.getItem("itemTitle"),
        category: [localStorage.getItem("categories")],
        description: localStorage.getItem("itemDes"),
        keywords: [localStorage.getItem("keywords")],
        meta_description: localStorage.getItem("meta_description"),
        alt: "",
        tags: [localStorage.getItem("tags")],
        slug: "",
        metaTitle: "",
        scheduledDate: "",
      },

      rules: {
        title: [
          { required: true, message: "هذا الحقل مطلوب", trigger: "blur" },
          // { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' }
        ],
        description: [
          { required: true, message: "هذا الحقل مطلوب", trigger: "blur" },
        ],
        keywords: [
          { required: true, message: "هذا الحقل مطلوب", trigger: "blur" },
        ],
        category: [
          { required: true, message: " هذا الحقل مطلوب ", trigger: "change" },
        ],
      },

      image: {
        Alt: "",
        imgTitle: "",
        imgDescription: "",
      },
      isActive: false,
      // borderStyle:'none',
      // displayStyle:'0',
      url: null,

      marks: {
        100: "0",
        0: "5000",
      },

      alert: "",
      alert2: "",
      alert3: "",
      alert4: "",

      show: false,
      show2: false,
      show3: false,
      show4: false,

      dialogImageUrl: "",
      dialogVisible: false,
      disabled: false,
      imageSelcteed: null,
      selectItems: undefined,

      imageSelcteedModel: [],

      imagesModel: undefined,

      image_id: undefined,
    };
  },

  mounted() {
    if (!localStorage.getItem("token")) {
      this.$router.push("/signIn");
    }

    Vue.axios
      .get("https://dashboard.arabicreators.com/api/blog_category")
      .then((rr) => {
        console.log(rr);
        this.selectItems = rr.data.data;
      });
  },

  methods: {
    onfileSelected(event) {
      this.imageSelcteed = event.target.files[0];
    },

    postData(e) {
      this.$refs[e].validate((valid) => {
        if (valid) {
          console.log("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });

      const dataForm = new FormData();
      dataForm.append("image", this.image_id);
      // dataForm.append("image", this.imageSelcteed, this.imageSelcteed.name);
      dataForm.append("title", this.form.title);
      dataForm.append("category", JSON.stringify(this.form.category));
      dataForm.append("description", this.form.description);
      dataForm.append("keywords", this.form.keywords);
      dataForm.append("meta_description", this.form.meta_description);
      // dataForm.append("alt", this.form.alt);
      dataForm.append("tags", JSON.stringify(this.form.tags));
      dataForm.append("slug", this.form.slug);
      dataForm.append("meta_title", this.form.metaTitle);
      dataForm.append("blog_id", localStorage.getItem("blogID"));
      dataForm.append("publish_time", this.form.scheduledDate);

      this.axios
        .post("https://dashboard.arabicreators.com/api/update_blog", dataForm, {
          headers: {
            "Content-Type": "multipart/form-data",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((res) => {
          console.log(res);
          if (res) {
            this.$message({
              showClose: true,
              message: "تم التعديل بنجاح",
              type: "success",
            });

            this.$router.push("/blog");
          }
        });
    },


  
  },
};
</script>

<style lang="scss">
.createConsultaion {
  position: relative !important;
  padding-top: 50px;
  position: relative;
  font-family: "Tajawal", sans-serif;

  direction: rtl;
  background-color: #f0efef;
  padding-bottom: 50px;
  .content {
    padding-top: 50px;
    padding-bottom: 50px;
    margin-top: 0 !important;
    .header {
      text-align: right;
      h2 {
        font-weight: 700;
      }

      p {
      }
    }

    .form {
      max-width: 60%;
      margin: auto;

      .el-form-item__label {
        display: block;
        text-align: right;
        padding-bottom: 0;
        font-weight: 700;
      }

      .el-select {
        width: 100%;
      }

      .el-input__inner {
        height: 50px;
        border-radius: 10px;
      }

      .el-button-group {
        display: flex;
        justify-content: space-between;
        margin-bottom: 60px;
      }

      .el-button-group > .el-button {
        border-radius: 50%;
        width: 50px;
        height: 50px;
      }

      .el-upload-dragger {
        width: 100%;
        height: 200px;
      }

      .el-upload {
        width: 100%;
        margin-bottom: 30px;
      }

      .el-slider__runway {
        background: linear-gradient(
          270.1deg,
          rgba(227, 0, 0, 0.99) 10.15%,
          #66b345 69.1%
        );
      }
    }

    .el-tag .el-icon-close {
      right: unset;
      left: 0;
    }

    .el-select .el-tag {
      margin: unset;
      margin-right: 6px;
    }
  }

  .transition-box {
    margin-bottom: 10px;
    width: 200px;
    height: 100px;
    border-radius: 4px;
    background-color: #ff4040;
    text-align: center;
    color: #fff;
    padding: 40px 20px;
    box-sizing: border-box;
    margin-right: 20px;
  }

  .el-form .el-form-item .blogInput .el-input__inner {
    text-align: right !important;
    background-color: transparent !important;
  }

  .blogInput {
    background-color: transparent !important;
  }

  .vue-input-tag-wrapper {
    width: 100%;
    // background-color: #fafafa;

    border-radius: 5px;
    height: 45px;
    border: none;
  }
}

.ck.ck-editor {
  width: 100%;
  margin-right: 10px;
}

.ck.ck-editor__editable_inline > :first-child {
  height: 200px;
}

.el-input-group__prepend {
  border: none !important;
  border-left: 2px solid #dcdfe6 !important;
  background-color: transparent !important;
}

// .el-textarea__inner {
//   border: none !important;
// }

.blogIcon {
  font-size: 20px !important;
  border-left: 2px solid #dbdbdb !important;
  padding: 8px;
  margin: 5px;
}

.fileInput1 {
  border: var(--darkColor) 2px solid;
  padding: 6px;
  background-color: transparent;
  color: var(--darkColor);
  margin-left: 10px;
  font-weight: 600;
  margin-top: 5px;
}

.btnBlog {
  display: inline-block !important;
  width: unset !important;
  padding: 0px 28px !important;
  background: var(--darkColor) !important;
  border-radius: 5px !important;
  color: white;
  height: 44px !important;
  margin-bottom: 10px;
  margin-top: 40px;
  margin-left: 10px !important;
  border: none;
}

.loader {
  display: flex;
  justify-content: center;
  width: 100%;
  height: 100%;
  position: absolute;
  right: 0;
  top: 0;
  z-index: 50;
}

.v-spinner {
  margin-top: 20%;
}

.el-switch {
  display: block !important;
  text-align: left;
  margin-bottom: 10px;
}
</style>


<style scoped>
.modal-dialog {
  max-width: 1200px;
}

.modal {
}

.modal .head {
}

.modal .head form {
  display: flex;
  justify-content: space-between;
}

.modal form .selects {
  display: flex;
}

.modal .main-content {
  height: 540px;
  width: 75%;
  overflow-y: scroll;
  overflow-x: hidden;
  margin: 10px;
}

.modal .img-info {
  width: 25% !important;
  padding: 10px;
  margin: 10px;
  padding-top: 20px;
  background-color: #f6f6f6;
}

.modal .main-content .item {
  box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  /* padding: 10px; */
  border-radius: 5px;
  margin: 10px;
  text-align: right;
  overflow: hidden;
  width: 100%;
  height: 208px;
  cursor: pointer;
}

.modal .main-content .item .img-box {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.modal .main-content .item .img-box img {
  width: 100%;
  height: 100%;
}

.modal .main-content .item p {
  margin-bottom: 5px;
  font-size: 16px;
  font-weight: 600;
}

.modal .main-content .item span {
  font-size: 12px;
}

.modal-footer {
  justify-content: space-between;
}

.modal-header .btn-close {
  margin: 0;
}

.btn-close:focus {
  box-shadow: none;
}
.form-control:focus {
  box-shadow: none;
}

.img-preview {
  width: 150px;
  height: 150px;
  display: block;
  margin: auto;
  margin-top: 30px;
}
</style>