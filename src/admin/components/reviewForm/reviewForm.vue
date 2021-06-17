<template>
  <div class="form-component">
    <form class="form" @submit.prevent="handleSubmit">
      <card title="Добавление отзыва">
        <div class="form-container" slot="content">
          <div class="form-cols">
            <div class="form-col">
              <label
                :style="{backgroundImage: `url(${newReview.preview})`}"
                :class="[ 'uploader', {active: newReview.preview}, {
                  hovered: hovered
                }]"
                @dragover="handleDragOver"
                @dragleave="hovered = false"
                @drop="handleChange"
              >
                <div class="uploader-title">Добавить фото</div>
                <div class="uploader-btn">
                  <app-button typeAttr="file" @change="handleChange"></app-button>
                </div>
              </label>
            </div>
            <div class="form-col">
              <div class="form-row">
                <app-input v-model="newReview.title" title="Имя автора" />
              </div>
              <div class="form-row">
                <app-input v-model="newReview.link" title="Титул автора" />
              </div>
              <div class="form-row">
                <app-input v-model="newReview.description" field-type="textarea" title="Отзыв" />
              </div>
            </div>
          </div>
          <div class="form-btns">
            <div class="btn">
              <app-button title="Отмена" plain></app-button>
            </div>
            <div class="btn">
              <app-button title="Сохранить" typeAttr="submit"></app-button>
            </div>
          </div>
        </div>
      </card>
    </form>
  </div>
</template>

<script>
import card from "../card";
import appButton from "../button";
import appInput from "../input";
import tagsAdder from "../tagsAdder";
import { mapActions } from "vuex";
export default {
  components: { card, appButton, appInput, tagsAdder },
  data() {
    return {
      hovered: false,
      newReview: {
        title: "",
        link: "",
        description: "",
        techs: "",
        photo: {},
        preview: "",
      },
    };
  },
  methods: {
    ...mapActions({
      addNewReview: "reviews/add",
    }),
    handleDragOver(e) {
      e.preventDefault();
      this.hovered = true;
    },
    async handleSubmit() {
      await this.addNewReview(this.newReview);
    },
    handleChange(event) {
      event.preventDefault();
      const file = event.dataTransfer 
        ? event.dataTransfer.files[0] 
        : event.target.files[0];
      this.newReview.photo = file;
      this.renderPhoto(file);
      this.hovered = false;
    },
    renderPhoto(file) {
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onloadend = () => {
        this.newReview.preview = reader.result;
      };
    },
  },
};
</script>

<style src="./reviewForm.pcss" lang="postcss" scoped></style>