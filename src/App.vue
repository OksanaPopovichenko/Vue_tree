<template>
    <div id="app">
        <div class="main-content">
			<Node
				v-for="node in nodes"
				:node="node"
				:delete="deleteMethod"
			>
			</Node>
			<div class="btns-group">
				<button type="button" class="btn btn-success" @click="create">Create</button>
			</div>
        </div>
    </div>
</template>

<script>
import Node from './components/Node.vue'

export default {
	name: 'app',
	components: {
		Node,
	},

	data: function() {
		return {
			nodes: [{ 
				title: '' ,
				desc: '' ,
				nodes: [],
				id: this.randomNum()
			}]
		}
	},

  	methods: {
		create() {
			this.nodes.push({ 
				title: '' ,
				desc: '' ,
				nodes: [],
				id: this.randomNum()
			});
		},

		deleteNode() {
			this.nodes.shift();
		},

		randomNum(){
			var rand = - 0.5 + Math.random() * 1001;
			rand = Math.round(rand);
			return rand;
		},

		deleteMethod(nodeId) {
			let idx = -1;
			this.nodes.forEach((node, index)=>{
				if (node.id === nodeId) {
					idx = index;
				}
			})
			
			if (idx > -1) {
				this.nodes.splice(idx, 1);
			} 
		}
  	}
}
</script>

<style>
.main-content{
	margin: 15px;
}

.btns-group{
	width: 200px;
    display: flex;
    flex-direction: row;
	margin-left: 15px;
}

.btn-group>.btn-group:not(:first-child)>.btn, .btn-group>.btn:not(:first-child) {
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;
}

.btn-group>.btn-group:not(:last-child)>.btn, .btn-group>.btn:not(:last-child):not(.dropdown-toggle) {
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
}

</style>
