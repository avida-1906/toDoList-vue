<!-- 写一个todolist小项目可以让你快速掌握一个框架的基本知识，以后记得了，react官方文档看完了也要写一个这样的小项目 -->

<script setup lang="ts">
import { ref, computed } from 'vue'
		let dataPass = ref('')
		let inputContent = ref('')
		let toDoListData = ref([])  //以后就要用ref一把梭，尽量不要用reactive太坑了。
		toDoListData.value = [
			{
				id: 0,
				title: '看react官方文档',
				isChecked: false
			}
		]
		function addItem() {
			toDoListData.value.push({
				id: toDoListData.value.length,
				title: dataPass.value,
				isChecked: false,
			})
			dataPass.value = ''
		}
		function clearDone() {
			//使用数组方法filter()抽取数组中满足条件的元素，并且把返回结果放回原来的数组就可以达到删除指定元素的目的。
			toDoListData.value = toDoListData.value.filter(item=>!item.isChecked)
		}
		function checkItem(item) {
			item.isChecked ? item.isChecked = false : item.isChecked = true;
		}

		const done = computed( ()=>{
			const y = toDoListData.value.filter( item=>item.isChecked )
			return y.length
		} )
		const totally = computed( ()=>{
			const y = toDoListData.value.length
			return y
		} )

</script>

<template>
	<div class="container">
		<h1>做一个简单的todo list 项目</h1>
		<header>
			<input type="text" v-model="dataPass" @change="addItem()" placeholder="请输入您的任务名称，按回车键确认">
		</header>
		<ul class="toDoList-parent">
			<!-- 给外层li绑定点击事件，代替checkbox原来的点击事件，这样就可以扩大点击范围了 -->
			<li class="toDoList-child" v-for="item in toDoListData" :key="item.id" @click="checkItem(item)">
				<input type="checkbox" :id="item.title" :name="item.title" v-model="item.isChecked" />
				{{ item.title }}
			</li>
		</ul>
		<footer>
			<div class="listState">
				<span>
					已完成：{{ done }} / 所有任务：{{ totally }}
				</span>
			</div>
			
			<div class="addItemButton">
				<button @click="clearDone()">清除已完成的任务</button>
			</div>
		</footer>
	</div>
</template>

<style scoped>
	* {
		margin: 0; 
		padding:0;
		box-sizing: border-box;
	}
    .container {
		display: flex;
		flex-direction:column;  /* 改变主轴的默认方向，让它垂直向下 */
		/* justify-content: space-between; */
	}
	.container>* {
		margin-bottom: 5%;
	}
	.container header input {
		width: 100%;
		line-height: 30px;
	}
	.toDoList-parent {
		list-style-type: none;
	}
	.toDoList-child {
		text-align: left;
		border: 1px solid black;
		padding: 2%;
		cursor: pointer;
	}
	.container footer button {
		width: 100%;
		font-size: 16px;
		border: 1px solid black;
		padding: 5%;
		background: red;
	}
	footer {
		display: grid;
		grid: 60px/auto auto;
		gap: 45%;
	}
	footer>* {
		background: #666;
	}
	footer>* {
		display: grid;
		align-content: center;  /* 这个办法可以用在grid布局中让item垂直居中 */
	}
</style>
