<template>
  <div>
    <h3>Carousel</h3>
    <div class="carousel">
      <ul>
        <li>
          <img :src="select.imageUrl" />
        </li>
      </ul>

      <input type="button" class="button-prev" @click="moveImgAction('prev')" value="<" />
      <span>{{select.title}}</span>
      <input type="button" class="button-next" @click="moveImgAction()" value=">" />
    </div>

    <h3>이미지 삭제</h3>
    <div>
      <label>
        id:
        <input type="number" v-model="deleteId" />
      </label>
      <button @click="delectAction()">삭제</button>
    </div>
  </div>
</template>

<script>
export default {
	data() {
		return {
			imgdata: require('../assets/data.json'),
			select: {},
			deleteId: '',
		};
	},
	mounted() {
		this.init(0);
	},
	methods: {
		init(setIndex) {
			this.select = this.imgdata[setIndex];
			setTimeout(this.moveImgAction, 5000);
		},
		moveImgAction(key) {
			let nowindex = this.imgdata.indexOf(this.select);
			let changeIndex =
				key === 'prev' && nowindex - 1 >= 0
					? nowindex - 1
					: (changeIndex =
							key === 'prev' && nowindex - 1 < 0
								? this.imgdata.length - 1
								: (changeIndex =
										nowindex + 1 <= this.imgdata.length - 1
											? nowindex + 1
											: 0));
			this.init(changeIndex);
		},
		delectAction() {
			const delIndex = this.imgdata.findIndex(img => img.id === this.deleteId);
			delIndex > -1 ? this.imgdata.splice(delIndex, 1) : '';
			this.deleteId = '';
		},
	},
};
</script>

<style scoped>
.carousel {
	overflow: hidden;
}
.carousel ul {
	margin: 0;
	padding: 0;
	list-style-type: none;
	display: flex;
}
</style>