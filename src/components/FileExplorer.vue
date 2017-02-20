<script>
	export default {
		name: 'FileExplorer',
		data: function() {
			return {
				currentPath: '/',
				filesDir: []
			};
		},
		props: {
			username: { //用户名
				type: String,
				require: true
			},
			repo: {  //某个代码仓库的名字
				type: String,
				require: true
			}
		},
		computed: {
			fullRepoUrl: function() {
				return this.username + '/' + this.repo;
			},
			sortedFiles: function() {
				return this.filesDir.slice(0).sort(function(a,b){
					if(a.type !== b.type) {
						if(a.type == 'dir'){
							return -1;
						} else {
							return 1;
						}
					} else {
						if(a.name < b.name){
							return -1;
						} else {
							return 1;
						}
					}
				});
			}
		},
		methods: {
			getFiles: function() {
				console.log('in getFiles()');
				var url = 'https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.currentPath;
				console.log('url is',url);
				this.$http
					.get(url)
					.then((res)=>{
						this.filesDir = res.body;
					}, (error)=>{
						console.log('error',error);
					});
			},
			changePath: function(path) {
				this.currentPath = '/' + path;
				this.getFiles();
			},
			goBack: function() {
				// slice(0,-1) 会删除数组最后一个元素
				this.currentPath = this.currentPath.split('/').slice(0,-1).join('/');
				if(this.currentPath == ''){
					this.currentPath = '/';
				}
				this.getFiles();
			}
		},
		watch: {
			repo: function(newVal, oldVal){
				this.currentPath = '/';
				this.getFiles();
			}
		},
		created: function() {
			if (this.username && this.repo) {
				this.getFiles();
			}
		}
	}
</script>
<template>
	<div class="row">
		<div class="col-md-12 col-sm-12">
			<table class="table table-hover">
				<thead>
					<tr>
						<th>当前路径：{{ currentPath }}</th>
						<th class="text-right">
							<button class="btn btn-danger btn-xs" @click="goBack" v-if="currentPath !== '/'">返&emsp;回</button>
						</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="file in sortedFiles">
						<td>
              <div class="file text-left" v-if="file.type === 'file'">
                <span class="octicon octicon-file-text"></span>
                <a href="#"> {{ file.name }}</a>
              </div>
              <div class="directory text-left" v-if="file.type === 'dir'">
                <span class="octicon octicon-file-directory"></span>
                <a href="#" @click="changePath(file.path)"> {{ file.name }}</a>
              </div>
            </td>
						<td class="text-right">
							<a v-bind:href=" file.download_url" download="file" v-if="file.type === 'file'">
								<span class="octicon octicon-cloud-download"></span>
							</a>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>






