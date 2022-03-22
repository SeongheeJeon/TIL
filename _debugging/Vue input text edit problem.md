[관련커밋 1](https://github.com/SeongheeJeon/VueNuxtStudy/commit/c230e9b81126faf8f9ccfdefa4a5f5bc691a64bf)  
[관련커밋 2](https://github.com/SeongheeJeon/VueNuxtStudy/commit/8d89ffb6e4f7ccb3f1dd9bb8bfcc4e1144a33ba1)  

### 문제 : 생성된 Task의 input 값을 수정 가능하도록 하려는데 수정한 뒤 newTask를 생성하면 수정전 값으로 돌아감.

```jsx
<Input v-model=”content”>

...

<script>
export default {
  props: ['task'],
  data() {
    return {
      content: this.task.content,
    }
  }
...
```

### 상황

- Task component의 mounted() 에서 출력되는 값은 모두 전부 처음 값. mounted는 각 Task가 mount 될 때 실행되는게 아닌가..?
- addTask에서는 newTask 제대로 저장 잘 됨.
- Task component에 props.task.content 전달 잘 됨.
- Task component mounted() 에서 this 출력했더니 예상과 달랐다. 새 컴포넌트가 task list의 위쪽이 아닌 아래쪽에 추가되는거였음.

### 원인

- 새로운 task를 입력하면 그 값이 tasks list에서 가장 위쪽에 위치했고, 그래서 그 Task component가 새로 생성된건 줄 알았다. 근데 그렇지 않고, 가장 아래쪽 컴포넌트가 새로 생성된 컴포넌트였음. 제일 처음 입력한 task 값이 새 컴포넌트의 props로 전달되고, 그래서 Task component mounted()에 props.task.content를 출력했을 때 계속 제일 처음 입력한 task 값이 출력된것.
- 컴포넌트는 위에서 아래로 나열되는게 기본인데.. 값이 반대로 할당돼서 헷갈렸다. store의 ADD_TASK()에서 newTask를 배열의 끝이 아닌 처음에(tasks[0]에) 할당하고, 기존값들은 뒤로 밀려나서 tasks list의 순서가 그랬던것.
- 그래서 render 되어있는 Task input 값을 수정해도, newTask를 생성하면 컴포넌트가 하나씩 밀리면서 $store.state.tasks 값이 다시 할당되어 수정전으로 되돌아갔음.

### 해결

- ADD_TASK()에서 배열의 끝에 newTask 추가하는 것으로 수정.
