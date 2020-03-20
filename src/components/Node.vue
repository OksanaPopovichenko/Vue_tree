<template>
	<div class="node">
		<div class="first-row">
			<div class="inputs-group">
				<form>
					<div class="form-group">
						<label for="formGroupExampleInput" class="title">Title</label>
						<input type="text" 
							:class="{view: !isEditing}"	
							id="formGroupExampleInput" 	
							v-model="node.title"
							:disabled="!isEditing"
							v-autowidth="{maxWidth: '225px', minWidth: '20px', comfortZone: 0}"
						>
					</div>
					<div class="form-group">
						<label for="formGroupExampleDesc" class="desc">Description</label>
						<input type="text" 
							:class="{view: !isEditing}"
							id="formGroupExampleDesc"
							v-model="node.desc"
							:disabled="!isEditing"
							v-autowidth="{maxWidth: '225px', minWidth: '20px', comfortZone: 0}"
						>
					</div>
				</form>
			</div>
			<div class="btn-group">
				<button type="button" class="btn btn-light" @click="isEditing = !isEditing" >{{ isEditing ? 'Save' : 'Edit' }}</button>
			</div>
		</div>
		<div class="second-row">
			<button type="button" class="btn btn-secondary" @click="create">Create</button>
			<button type="button" class="btn btn-warning" @click="toggle" v-if="haveChild">Show childs</button>
			<button type="button" class="btn btn-primary" @click="deleteChild">Delete</button>
		</div>	
		<Node
			v-for="node in node.nodes"
			:node="node"
			:delete="deleteMethod"
			v-if="isExpanded"
		>
		</Node>
	</div>
</template>

<script>
import Vue from 'vue'
import VueInputAutowidth from 'vue-input-autowidth'
Vue.use(VueInputAutowidth)

export default {
	name: 'Node',

	data: function() {
		return {
			isEditing: false,
			isExpanded: true,
		}
	},

	props: ['node', 'delete'],

	
  	methods: {
		create() {
			this.node.nodes.push({ 
				title: '' ,
				desc: '' ,
				nodes: [],
				id: this.randomNum()
			});
			Vue.set(this.node, 'isExpanded', !this.isExpanded);
			console.log(this.isExpanded);
		},

		deleteChild() {
			if (this.delete) {
				this.delete(this.node.id)
			}
		},

		toggle() {
			if (this.haveChild) {
				Vue.set(this, 'isExpanded', !this.isExpanded);
			}
		},

		randomNum(){
			var rand = - 0.5 + Math.random() * 10001;
			rand = Math.round(rand);
			return rand;
		},

		deleteMethod(nodeId) {
			let idx = -1;
			this.node.nodes.forEach((node, index)=>{
				if (node.id === nodeId) {
					idx = index;
				}
			})
			
			if (idx > -1) {
				this.node.nodes.splice(idx, 1);
			} 
		}
  	},

	computed: {
		haveChild : function() {
			return this.node.nodes && this.node.nodes.length;
		}
	}

}

</script>


<style scoped>

.node{
	background: #fff;
    border-radius: 5px;
	box-shadow: 0 0 5px 1px rgba(221, 221, 221, 1);
	padding: 15px;
	margin: 15px;
}

.node .first-row{
	display: flex;
    flex-direction: row;
    justify-content: space-between;
	margin-bottom: 15px;
}

.form-group{
	display: flex;
	flex-direction: row;
}

.form-group .title, .form-group .desc{
	margin-bottom: 0;
	padding-right: 10px;
    font-size: 22px;
}

.btn-group{
	display: flex;
    flex-direction: column;
	justify-content: center;
}

.btn-group .btn{
	margin: 5px;
}

.btn-group>.btn-group:not(:first-child)>.btn, .btn-group>.btn:not(:first-child) {
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;
}

.btn-group>.btn-group:not(:last-child)>.btn, .btn-group>.btn:not(:last-child):not(.dropdown-toggle) {
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
}

.second-row{
	display: flex;
    flex-direction: row;
    justify-content: flex-end;
	padding-top: 15px;
    border-top: 1px solid #eee;
}

.second-row .btn{
	margin: 0 15px;
}

.form-control{
	border: 0;
}

.view {
	border-color: transparent;
	background-color: initial;
}


#formGroupExampleInput, #formGroupExampleDesc{
	padding: 5px;
}

</style>
