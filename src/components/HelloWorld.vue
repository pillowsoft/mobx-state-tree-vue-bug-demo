<template>
  <div class="hello">
    <p>
     Click on each button to test mobx and mobx-state-tree for updates.
    </p>
	<p><button @click="mobxStateTreeClick">mobx-state-tree</button>mobx-state-tree says (should toggle): {{mstState.todo.done}}</p>
	<p><button @click="mobxClick">mobx</button>mobx says (should increment): {{mobxState.age}}</p>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { Observer } from 'mobx-vue';
import { action, computed, observable } from 'mobx';
import { types, onSnapshot } from 'mobx-state-tree';

const Todo = types
	.model('Todo', {
		title: types.string,
		done: false
	})
	.actions(self => ({
		toggle() {
			self.done = !self.done;
		}
	}));

const Store = types.model('Store', {
	todos: types.array(Todo),
	todo: Todo
});

// create an instance from a snapshot
const theMstState = Store.create({
	todos: [
		{
			title: 'Get coffee'
		}
	],
	todo: {
		title: 'Get stuff'
	}
});

// listen to new snapshots
// onSnapshot(theMstState, snapshot => {
// 	console.info('todo updated:', snapshot);
// });

class ViewModel {
	@observable age = 10;
	@observable users = [];

	@computed
	get computedAge() {
		return this.age + 1;
	}

	@action.bound
	setAge() {
		this.age++;
	}
}

const theMobxState = new ViewModel();

@Observer
@Component
export default class HelloWorld extends Vue {
	mstState = theMstState;
	mobxState = theMobxState;

	mobxClick() {
		this.mobxState.setAge();
		// console.log('mobx click!');
	}

	mobxStateTreeClick() {
		// this.mstState.todo.toggle();
		// console.log('mobx-state-tree click!');
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
button {
	padding: 10px;
	margin: 20px;
	height: 40px;
	width: 200px;
	text-align: left;
}
h3 {
	margin: 40px 0 0;
}
ul {
	list-style-type: none;
	padding: 0;
}
li {
	display: inline-block;
	margin: 0 10px;
}
a {
	color: #42b983;
}
</style>
