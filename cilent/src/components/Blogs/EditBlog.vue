<template>
	<div class="push_right">
		<h1>แก้ไขรีวิว</h1>
		<form v-on:submit.prevent="editBlog">
			<p>ชื่อเหล้า: <input type="text" v-model="blog.title" /></p>
            <p>ราคา: <input type="text" v-model="blog.category" /></p>
            <p>ระดับแอลกอฮอลล์: <input type="text" v-model="blog.status" /> </p>
			<p><strong>รูปภาพ:</strong></p>
			<p><strong> Select Image :</strong></p>
			<form enctype="multipart/form-data" novalidate>
        <div class="dropbox">
          <input
            type="file"
            multiple
            :name="uploadFieldName"
            :disabled="isSaving"
            @change="
              filesChange($event.target.name, $event.target.files);
              fileCount = $event.target.files.length;
            "
            accept="image/*"
            class="input-file"
          />
          <!-- <p v-if="isInitial || isSuccess"> -->
          <h2> <p v-if="isInitial">Select file</p> </h2>
          <p v-if="isSaving">Uploading {{ fileCount }} files...</p>
          <p v-if="isSuccess">...Successfully executed...</p>
          <p v-if="isFailed">...Failed...</p>
        </div>
        <div>
          <ul class="pictures">
            <li v-for="picture in pictures" v-bind:key="picture.id">
              <img
                style="margin-bottom: 5px"
                :src="BASE_URL + picture.name"
                alt="picture image"
              />
              <button v-on:click.prevent="delFile(picture)">Delete Image</button>
			        <button v-on:click.prevent="useThumbnail(picture.name)">Reduce image size</button>
             <div class="highlight"><h5><p>**Click on the picture and drag it down to the bottom box.**</p></h5></div>
            </li>
          </ul>
          <div class="clearfix"></div>
        </div>
      </form>
      <p>
        <vue-ckeditor
          v-model.lazy="blog.content"
          :config="config"
          @blur="onBlur($event)"
          @focus="onFocus($event)"
        />
      </p>
	  		<p>
	  			<button type="submit">ตกลง</button>
				<button v-on:click="navigateTo('/blogs')">กลับ</button>
			</p>
		</form>
	</div>
</template>

<script>
import BlogsService from "@/services/BlogsService";
import VueCkeditor from 'vue-ckeditor2'

export default {
	data() {
		return {
			blog: {
				title: "",
				thumbnail: "null",
				pictures: "null",
				content: "",
				category: "",
				status: "",
			},

			config:{
				toolbar: [
					{ 
						name: "document",
						items: [
							"Source",
							"-",
							"Save",
							"NewPage",
							"Preview",
							"Print",
							"-",
							"Templates",
						],
					},

					{
						name: "clipboard",
						items: [
							"Cut",
							"Copy",
							"Paste",
							"PasteText",
							"PasteFromWord",
							"-",
							"Undo",
							"Redo",
						],
					},

					{
						name: "editing",
						items: ["Find", "Replace", "-", "SelectAll", "-", "Scayt"],
					},

					{
						name: "forms",
						items: [
							"Form",
							"Checkbox",
							"Radio",
							"TextField",
							"Textarea",
							"Select",
							"Button",
							"ImageButton",
							"HiddenField",
						],
					},
					"/",

					{
						name: "basicstyles",
						items: [
							"Bold",
							"Italic",
							"Underline",
							"Strike",
							"Subscript",
							"Superscript",
							"-",
							"CopyFormatting",
							"RemoveFormat",
						],
					},

					{
						name: "paragraph",
						items: [
							"NumberedList",
							"BulletedList",
							"-",
							"Outdent",
							"Indent",
							"-",
							"Blockquote",
							"CreateDiv",
							"-",
							"JustifyLeft",
							"JustifyCenter",
							"JustifyRight",
							"JustifyBlock",
							"-",
							"BidiLtr",
							"BidiRtl",
							"Language",
						],
					},
					{ name: "links", items: ["Link", "Unlink", "Anchor"] },

					{
						name: "insert",
						items: [
							"Image",
							"Flash",
							"Table",
							"HorizontalRule",
							"Smiley",
							"SpecialChar",
							"PageBreak",
							"Iframe",
							"InsertPre",
						],
					},
					"/",
					{ name: "styles", items: ["Styles", "Format", "Font", "FontSize"] },
					{ name: "colors", items: ["TextColor", "BGColor"] },
					{ name: "tools", items: ["Maximize", "ShowBlocks"] },
					{ name: "about", items: ["About"] },
				],
				height: 300
			}
		};
	},

	methods: {
		async editBlog() {
			try {
				await BlogsService.put(this.blog);
				this.$router.push({
					name: "blogs",
				});
			}catch (err) {
				console.log(err);
			}
		},
	},

	async created() {
		try {
			let blogId = this.$route.params.blogId;
			this.blog = (await BlogsService.show(blogId)).data;
		}catch (error) {
			console.log(error);
		}
	},

	components:{
		VueCkeditor
	}
	
};
</script>

<style scoped>
.push_right{
    margin-top: 50px;
    margin-left: 470px;
}
.nv-navbar {
  background-color: #DCDCDC;
  width: 100%;
  padding: 10px 0px 10px 0px;
}
.nv-navbar .nav {
  list-style: none;
  margin: 0;
  padding: 0;
  float: left;
}
.nv-navbar .nav li {
  float: left;
}
.nv-navbar .nav li a {
  padding: 10px;
  text-decoration: none;
  color: #778890;
  font-weight: bold;
}
.nv-navbar .nav li a:hover {
  padding: 10px;
  text-decoration: none;
  color: black;
}
.nv-navbar .nav li a.router-link-active {
  background-color: #708090;
  color: black;
}
.clearfix {
  clear: left;
}
</style>
