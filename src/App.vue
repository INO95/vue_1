<script>
import ListItem from './ListItem.vue'

//옵션 API 스타일
export default {
  components:{
    ListItem
  },
  //변수 선언과 비슷하다.
  data(){
    return {
      message: '안녕 vue!',
      isRed: true,
      color: 'green',
      show: true,
      list: [1,2,3],
      text: '수정해보세요',
      checked: true,
      checkedNames: ['철수'],
      picked: '서울',
      selected: '가',
      multiSelected: ['무궁화'],
      groceryList: [
        { id:0, text:'채소'},
        { id:1, text:'치즈'},
        { id:2, text:'인간이 먹을 수 있는 모든 것'}
      ]
    }
  },
  //함수 선언과 비슷하다.
  methods:{
    reverseMessage(){
      this.message = 
      this.message.split('').reverse().join('')
    },
    notify(){
      alert('탐색이 금지되었습니다.')
    },
    toggleRed(){
      //클릭하면 turthy, falsy 값이 바뀜
      this.isRed = !this.isRed
    },
    toggleColor(){
      //클릭했을 때, green이면 blue가 되고 아니면 그대로 green
      this.color = this.color === 'green' ? 'blue' : 'green'
    }
  }
}
</script>

<template>
  <div>
  <!-- 컴포지션 api 스타일의 경우 ref는 템플릿에서 자동으로 "언래핑" 되므로 
  템플릿 내에서 .value가 필요하지 않습니다. -->
  <h1>{{message}}</h1>
  <!-- 메서드/함수에 바인딩합니다.
  @click 문법은 v-on:click의 줄임말입니다. -->
  <button @click="reverseMessage">메세지 뒤집기</button>
  <!-- 인라인 표현식 문장이 될 수도 있습니다. -->
  <button @click="message += '!'">"!" 추가하기</button>
  <!-- vue는 e.preventDefault() 및 e.stopPropagation()과 
  같은 일반적인 작업에 대한 수식어를 제공합니다. -->
  <!-- e.preventDefault는 어떠한 동작을 방지하는 엘리먼트이다 -->
  <a href="https://vuejs.org" @click.prevent="notify">
    e.preventDefault()가 작동하는 링크
  </a>
  <p>
    <!-- 바인딩 -->
    <span :title="message">
      동적으로 바인딩 된 나의 title을 보려면 몇 초 동안 마우스를 올려놓으세요.
    </span>
  </p>
  <!-- 클래스 바인딩은 일반 문자열 외에도 객체 및 배열을 지원합니다. -->
  <p :class="{ red: isRed }" @click="toggleRed">
  이것은 빨간색이어야 합니다. 하지만 전환하려면 클릭하세요.
  </p>
  <!-- 스타일 바인딩은 객체 및 배열도 지원합니다. -->
  <p :style="{ color }" @click="toggleColor">
  이것은 녹색이어야 합니다. 하지만 파란색으로 바꾸려면 클릭하세요.
  </p>
  <button @click="show = !show">목록 전환</button>
  <button @click="list.push(list.length + 1)">숫자 추가</button>
  <button @click="list.pop()">숫자 제거</button>
  <button @click="list.reverse()">목록 뒤집기</button>

  <!-- show는 true며 list.length가 0이면 v-else 실행 -->
  <ul v-if="show && list.length">
    <!-- key는 필수이며 v-bind로 선언할 것 -->
    <li v-for="item in list" v-bind:key="item">{{ item }}</li>
  </ul>
  <!-- show값을 false로 바꿔 숨김 처리 -->
  <p v-else-if="list.length">목록이 비어있지 않지만, 숨김처리 되었습니다.</p>
  <p v-else>목록이 비었습니다.</p>

  <h2>텍스트 입력</h2>
  <input v-model="text">{{ text }}

  <h2>체크박스</h2>
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">체크됨: {{ checked }}</label>
  <!-- 여러 체크박스 값을 v-model을 사용하여 하나의 배열에 바인딩할 수 있습니다. -->
  <h2>멀티 체크박스</h2>
  <input type="checkbox" id="chulsoo" value="철수" v-model="checkedNames">
  <label for="chulsoo">철수</label>
  <input type="checkbox" id="yuri" value="유리" v-model="checkedNames">
  <label for="yuri">유리</label>
  <input type="checkbox" id="maenggu" value="맹구" v-model="checkedNames">
  <label for="maenggu">맹구</label>
  <p>체크된 이름: <pre>{{ checkedNames }}</pre></p>
  <h2>라디오</h2>
  <input type="radio" id="seoul" value="서울" v-model="picked">
  <label for="seoul">서울</label>
  <br>
  <input type="radio" id="busan" value="부산" v-model="picked">
  <label for="busan">부산</label>
  <br>
  <span>고른 것:  {{ picked }}</span>
  
  <h2>선택</h2>
  <select v-model="selected">
    <option disabled value="">하나를 선택하세요</option>
    <option>가</option>
    <option>나</option>
    <option>다</option>
  </select>
  <span>선택함: {{ selected }}</span>

  <h2>복수 선택</h2>
  <select v-model="multiSelected" multiple style="width:100px">
    <option>무궁화</option>
    <option>진달래</option>
    <option>개나리</option>
  </select>
  <span>선택함: {{ multiSelected }}</span>

  <!-- item 객체를 제공하여 list-item이 컨텐츠를 동적으로 나타낼 수 있도록
  합니다. 그리고 컴포넌트에 "key"를 제공해야 합니다. -->
  <ol>
    <ListItem
      v-for="item in groceryList" 
      :todo="item" 
      :key="item.id"
      ></ListItem>
  </ol>

</div>
</template>

<style>

.red {
  color: red;
}

button, a {
  display: block;
  margin-bottom: 1em;
}
</style>