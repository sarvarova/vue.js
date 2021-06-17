<template>
  <div class="reviews-page-component page-component">
    <div class="page-content">
      <div class="container">
        <div class="page-header">
          <div class="page-title">
            Блок "Отзывы"
          </div>
        </div>
        <div class="form">
          <app-form />
        </div>
        <ul class="cards">
          <li class="item" v-for="review in reviews" :key="review.id">
            <review-card
              :review="review"
            />
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import appForm from "../../components/reviewForm";
import reviewCard from "../../components/reviewCard";
import { mapState, mapActions } from "vuex";
export default {
  components: { appForm, reviewCard },
  computed: {
    ...mapState("reviews", {
      reviews: (state) => state.data,
    }),
  },
  methods: {
    ...mapActions({
      fetchReviews: "reviews/fetch"
    }),
  },
  mounted() {
    this.fetchReviews();
  }
};
</script>

<style scoped lang="postcss" src="./reviews.pcss"></style>