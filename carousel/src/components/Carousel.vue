<template>
  <div>
    <h3>Carousel</h3>
    <div class="carousel">
      <ul>
        <li v-for="(item, index) in imgdata" :key="index" :id="'imgWrap'+item.id" 
		class="imglist"
		>
          <img :src="item.imageUrl" />
        </li>
      </ul>
    </div>

	<input type="button" class="button-prev" @click="moveImgAction('prev')" value="<" />
      <span>{{select.title}}</span>
      <input type="button" class="button-next" @click="moveImgAction()" value=">" />
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
			let x = document.querySelector(".carousel").scrollLeft;
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
			let left = changeIndex > nowindex ? x+400 : changeIndex === 0 ? 0 : x-400; 
			document.querySelector(".carousel").scroll({
				left: left,  
      			behavior: 'smooth'});
			this.init(changeIndex);
			
		},
		delectAction() {
			const delIndex = this.imgdata.findIndex(img => img.id == this.deleteId);
			delIndex > -1 ? this.imgdata.splice(delIndex, 1) : '';
			this.deleteId = '';
		},
	},
};
</script>

<style scoped>
.carousel{
	width: 400px;
	overflow: scroll;
	-ms-overflow-style: none;
}
.carousel::-webkit-scrollbar{
	display: none;
}
.carousel ul {
	margin: 0;
	padding: 0;
	width: max-content;
	height: 300px;
	overflow: hidden;
	display: flex;
}
.carousel ul li{
	list-style: none;
	margin: 0 0 0 0;
    padding: 0 0 0 0;
    border : 0;
    float: left;
}
.carousel ul li img{
	object-fit: cover;
	width: 100%;
	height: 100%;
}
</style>