<template>
  <div class="home">
    <HelloWorld msg="mobx-state-tree/mobx-vue issue demo"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src

@Component({
	components: {
		HelloWorld
	}
})
export default class Home extends Vue {
	//
	dummyData = 0;
}
</script>
