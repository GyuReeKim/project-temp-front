<template>
  <div
    class="modal fade"
    tabindex="-1"
    role="dialog"
    v-bind:id="`gg-${movie.id}`"
    data-backdrop="static"
    data-keypress="false"
  >
    <div class="modal-dialog" role="document">
      <div class="modal-content bg-dark text-white">
        <div class="modal-header">
          <h5 class="modal-title">🎬 {{movie.title}} ({{movie.title_en}})</h5>
          <button type="button" class="close text-white" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <img
            class="movie--poster my-3"
            v-bind:src="movie.poster_url"
            v-bind:alt="movie.title"
            style="width:50%"
          />

          <hr style="background-color:white" />
          <p class="text-center">DETAIL</p>
          <div>평점 : {{movie.score}}</div>
          <span>등급 : {{movie.grade.name}}</span>
          <div>
            <span>장르 :</span>
            <span v-for="genre in movie.movie_genres" :key="genre.id">{{genre.name}}</span>
          </div>
          <div>
            <span>감독 :</span>
            <span v-for="director in movie.movie_directors" :key="director.id">{{director.name}}</span>
          </div>
          <p>누적 관람객 : {{movie.audience}}명</p>
          <p>{{movie.summary}}</p>

          <hr style="background-color:white" />
          <p class="text-center">예고편</p>
          <span v-if="movie.video_url">
            <iframe :src="movie.video_url" frameborder="0" style="width:100% ;height:300px;"></iframe>
          </span>
          <span v-else>😱</span>

          <hr style="background-color:white" />
          <p class="text-center">OST</p>
          <span v-if="movie.ost_url">
            <iframe :src="movie.ost_url" frameborder="0" style="width:100% ;height:300px;"></iframe>
          </span>
          <span v-else>😱</span>

          <span v-if="isAuthenticated">
            <hr style="background-color:white" />
            <div>
              <p class="text-center">REVIEW</p>
              <div class="input-group">
                <label for="comment">comment</label>
                <input id="comment" class="form-control" type="text" v-model="review.comment" />
              </div>
              <div class="input-group">
                <label for="score">score</label>
                <input id="score" class="form-control" type="number" v-model="review.score" />
              </div>
              <button class="btn btn-primary" @click="createreview">리뷰생성</button>
              <div>{{reviews}}</div>
            </div>
          </span>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-primary" data-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "movielistitemmodal",

  props: {
    movie: Object,
    genres: Array
  },
  data() {
    return {
      review: {
        comment: "",
        score: ""
      },
      isAuthenticated: this.$session.has("jwt"),
      reviews: []
    };
  },
  methods: {
    createreview() {
      const token = this.$session.get("jwt");
      const header = {
        headers: {
          Authorization: `JWT ${token}`
        }
      };

      axios
        .post(
          `http://127.0.0.1:8000/api/v1/movies/${this.movie.id}/reviews/`,
          this.review,
          header
        )
        .then(response => {
          const data = response.data;

          this.reviews.push(data);
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted() {
    // 영화가 가지고 있는 리뷰들을 가져와서,
  //   axios
  //     .get("")
  //     .then(res => {
  //       // 가져왔으면, 그 데이터를 현재 data의 reviews에 저장.
  //     })
  //     .catch(err => console.log(err));
  }
};
</script>

<style>
</style>