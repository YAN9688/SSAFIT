<template>
  <div class="container">
    <h4 class="text-center" v-if="!store.videos || store.videos.length === 0">
      <br />
      <br />
      아직 검색된 결과가 없습니다.
      <br /><br />검색어를 입력해주세요.
    </h4>
    <div v-else>
      <div id="youtube-carousel" class="carousel slide carousel-dark">
        <div class="carousel-inner">
          <div
            class="carousel-item"
            :class="{ active: index === currentGroup }"
            v-for="(group, index) in videoGroups"
            :key="index"
          >
            <div class="row">
              <div
                class="col-md-6"
                v-for="video in group"
                :key="video.id.videoId"
              >
                <YoutubeVideoListItem
                  :video="video"
                  :active="index === currentGroup"
                />
              </div>
            </div>
          </div>
        </div>

        <button
          class="carousel-control-prev"
          type="button"
          data-bs-target="#youtube-carousel"
          data-bs-slide="prev"
          @click="prev"
        >
          <span class="carousel-control-prev-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Previous</span>
        </button>
        <button
          class="carousel-control-next"
          type="button"
          data-bs-target="#youtube-carousel"
          data-bs-slide="next"
          @click="next"
        >
          <span class="carousel-control-next-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Next</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import YoutubeVideoListItem from "@/components/youtube/YoutubeVideoListItem.vue";
import { useYoutubeStore } from "@/stores/youtube";
import { ref, computed } from "vue";

const store = useYoutubeStore();
const currentGroup = ref(0);

const videoGroups = computed(() => {
  const groups = [];
  for (let i = 0; i < store.videos.length; i += 4) {
    // Change 9 to 4
    groups.push(store.videos.slice(i, i + 4));
  }
  return groups;
});

const prev = function () {
  if (currentGroup.value > 0) {
    currentGroup.value--;
  }
};

const next = function () {
  if (currentGroup.value < videoGroups.value.length - 1) {
    currentGroup.value++;
  }
};
</script>

<style scoped>
#youtube-carousel {
  position: relative;
}
.carousel-control-prev,
.carousel-control-next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 10;
  width: 50px; /* 버튼의 너비 조절 */
  height: 50px; /* 버튼의 높이 조절 */
}

.carousel-control-prev-icon,
.carousel-control-next-icon {
  font-size: 20px; /* 아이콘의 크기 조절 */
  width: 100%; /* 아이콘의 너비를 버튼 크기에 맞춤 */
  height: 100%; /* 아이콘의 높이를 버튼 크기에 맞춤 */
  color: dodgerblue;
}

.carousel-control-prev {
  left: -100px; /* 필요에 따라 조절 */
}

.carousel-control-next {
  right: -100px; /* 필요에 따라 조절 */
}
.video-wrapper {
  position: relative;
  padding-top: 56.25%; /* 16:9 Aspect Ratio */
  height: 0; /* Collapse the container to just the padding */
}

.video-wrapper iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.row {
  display: flex;
  flex-wrap: wrap;
}

.col-md-6 {
  flex: 0 0 50%; /* Adjust this to change how much space the column takes up */
  max-width: 50%; /* Same as above */
}
</style>
