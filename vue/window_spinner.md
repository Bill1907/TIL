# Spinner 추가 하기 

spinner를 추가할 때 ```window``` 전역객체안에 추가할 수 있다. 

## window 객체 안에 spinner 추가

```js
<template>
  <div
    v-if="isSpinner"
    id="spinnerContainer"
  >
    <div />
  </div>
</template>
```
```js 
<script>
export default {  
  data() {
    return {
      isSpinner: false,
    };
  }
  created() {
    window.spinner = this.spinner;
  },
  methods: {
    clearDebounce() {
      if (isSpinner) {
        this.isSpinner = !this.isSpinner;
      }
    },
    spinner(flag) {
      this.clearDebounce();
      this.isSpinner = flag;
    },
  }
}
</script>
```

```js
<style scoped>
#spinnerContainer {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #000000ab;
}
#spinnerContainer div {
  width: 5vw;
  height: 5vw;
  background: var(--primary-color);
  -webkit-animation: square-spin 3s 0s cubic-bezier(.09,.57,.49,.9) infinite;
  animation: square-spin 3s 0s cubic-bezier(.09,.57,.49,.9) infinite;
}

@keyframes square-spin {
  25% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(0);
    transform: perspective(100px) rotateX(180deg) rotateY(0);
  }
  50% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(180deg);
    transform: perspective(100px) rotateX(180deg) rotateY(180deg);
  }
  75% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(180deg);
    transform: perspective(100px) rotateX(0) rotateY(180deg);
  }
  100% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(0);
    transform: perspective(100px) rotateX(0) rotateY(0);
  }
}
</style>
```
