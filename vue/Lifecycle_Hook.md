# Vue life cycle hook 

각 Vue 컴포넌트 들을 생성될때 일련의 과정들을 통해 생성된다. 예를들어 데이터 관찰을 설정하고, 템플릿을 컴파일하고, DOM에 인스턴스를 마운트하고, 데이터가 변경되면 DOM을 업데이트해야 합니다. 그 과정에서 수명 주기 후크(lifecycle hooks)라는 기능도 실행하여 사용자가 특정 단계에서 자신의 코드를 추가할 수 있는 기회를 제공합니다.

## lifecycle hooks 등록 

예를들어, ```mounted```훅은 구성 요소가 초기 랜더링을 완료하고 DOM 노드를 만든 후 코드를 실행하는데 사용할 수 있다. 
```js
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```
인스턴스를 실행할때 다른 훅도 사용할 수 있다. ```created```, ```unmounted```, ```updated``` 등등...

모든 lifecycle 훅들은 현재 활성 인스턴스를 가르키는 ```this``` 컨텍스트를 사용한다. 화살표함수랑 같이 쓰지 않도록 유의해야한다. 


## lifecycle Diagram

![image](https://user-images.githubusercontent.com/59524676/164257166-45017409-56f7-45be-974b-2620a001cdfd.png)

[참고링크](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)
