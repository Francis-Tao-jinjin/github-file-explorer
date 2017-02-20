<script>
	import FileExplorer from './FileExplorer.vue'

	export default {
		name: 'Github',
		data: function() {
			return {
				fullRepoName: '',
				username: '',
				repo: ''
			};
		},
		components: {
			FileExplorer
		},
		methods: {
			changeRepo: function() {
				this.username = this.fullRepoName.split('\/').shift();
				this.repo = this.fullRepoName.split('\/').pop();
				console.log('username',this.username);
				console.log('repo',this.repo);	
			}
		}
	}
</script>
<template>
	<div id="container" class="container">
		<div class="row">
			<legend><h2><span class="mega-octicon octicon-mark-github">GitHub 文件浏览器</span></h2></legend>
			<!-- 提交事件不再重载页面 -->
			<form id="changeRepoForm" @submit.prevent="changeRepo" class="form-inline">
				<div class="form-group">
					<!-- label for 属性的值为等待绑定的输入框的 id -->
					<label for="fullRepoName">Full Repo Name：</label>
					<input type="text" id="fullRepoName" class="form-control" placeholder="username/repo" v-model="fullRepoName" style="width: 300px; color: black;">
				</div>
				<button type="submit" class="btn btn-primary">
					获&emsp;取					
				</button>
			</form>
			<hr>
		</div>
		<file-explorer :username="username" :repo="repo"></file-explorer>
	</div>
</template>

<style type="text/css">
#fullRepoName {
    margin-right: 10px;
    width: 290px;
}
#changeRepoForm {
	margin: 15px;
}
</style>