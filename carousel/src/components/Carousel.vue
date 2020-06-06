<template>
  <div>
    <h3>Carousel</h3>
	<main class="carousel">
		<div class="imglist">
			<div class="item" v-for="(item, index) in imgdata" :key="index" :id="'imgWrap'+item.id">
				<img :src="item.imageUrl" />
			</div>
		</div>
	</main>
	<nav>
		<input type="button" class="button-prev" @click="moveImgAction('prev')" value="<" />
		<span>{{select.title}}</span>
		<input type="button" class="button-next" @click="moveImgAction()" value=">" />
	</nav>
	<section>
		<h3>이미지 삭제</h3>
		<div>
			<label>
				id:
				<input type="number" v-model="deleteId" />
			</label>
			<button @click="delectAction()">삭제</button>
		</div>
	</section>
  </div>
</template>

<script>
export default {
	data() {
		return {
			imgdata: require('../assets/data.json'),
			select: {},
			deleteId: '',
			speed: 500,
			timer: ''
		};
	},
	mounted() {
		this.loof(0);
	},
	methods: {
		loof(index){
			this.select = this.imgdata[index];
			this.timer = setTimeout(this.moveImgAction, 5000);	
		},
		moveImgAction(key){
			clearTimeout(this.timer);
			let nowindex = this.imgdata.indexOf(this.select);
			let nextIndex = nowindex + 1;
			if(key == "prev") nextIndex = nowindex - 1;
			if(nextIndex < 0) nextIndex = this.imgdata.length-1;
			if(nextIndex > this.imgdata.length-1) nextIndex = 0;
			let list = document.querySelector(".imglist");
			list.style.transition = this.speed+"ms";
			list.style.transform = "translate(-"+(400 * nextIndex)+"px, 0)";

			this.loof(nextIndex);
		},
		delectAction() {
			const delIndex = this.imgdata.findIndex(img => img.id == this.deleteId);
			if(delIndex > -1){
				if(this.imgdata.indexOf(this.select) == delIndex){
					let nextIndx = delIndex+1;
					if(nextIndx > this.imgdata.length-1) nextIndx = 0;
					this.loof(nextIndx);
				}
				this.imgdata.splice(delIndex, 1);
			}else{
				alert("해당하는 이미지 번호는 존재 하지않습니다.");
			}
			this.deleteId = '';
		},
	},
};
</script>

<style scoped>
.carousel{
	width: 400px;
	overflow: hidden;
	position: relative;
	margin: auto;
}
.imglist{
	display: flex;
}
nav{
	text-align: center;
}
</style>