# Class & Interfaces

## 클래스만들기 

```ts
class Department {
  public name: string;
  private employ: string[] = [];

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log('Department ' + this.name);
  }

  addEmployee(employee: string) {
    this.employ.push(employee);
  }

  printEmploy(this: Department) {
    console.log(this.employ);
  }
}

const accounting = new Department('Accounting');

accounting.describe();
accounting.addEmployee('Bill');
accounting.addEmployee('Will');
accounting.printEmploy();

```

```constructor```는 클래스의 특수 매서드 이다. 처음 클래스의 인스턴스를 만들대 사용된다. 

```public```과 ```private``` 이 있는데 public같은 경우에는 인스턴스를 생성한 후에 인스턴스로 접근이 가능하고 private같은 경우엔는 클래스 내부로만 접근이 가능하기 때문에 수정이 필요한 경우에는
매서드통해 수정하도록 해야한다. 

## 약식으로 줄이기 

```ts
class Department {
  public name: string;
  private employ: string[] = [];

  constructor(n: string) {
    this.name = n;
  }
}

```
위 예시처럼 처음 인스턴스를 생성할때 constructor에서 다시 코드를 처야하는 불편함이 있는데 이를 해결해보자 

```ts
class Department {
  private employ: string[] = [];

  constructor(public name: string) {}
}

```
이렇게 하게되면 ```name```이란 인수로 만들어지고 저장하게 된다. 매우 편리! 또한 인자 뒤에 ```readonly```를 추가하게 되면 변경할 수 없게 된다. 


## 상속

```ts
class Department {
  private employ: string[] = [];

  constructor(public id: string, public name: string) {}
}
```

Department라는 class를 정의 했다. 하지만 일반적인 department가 아닌 전문 department들을 생성할 수 있다. 여기에는 전문적인 특징이 들어가게 되는데   
상속을 이용해서 만들 수 있다. 여기에서 중요한건 하나의 클래스에서만 상속할 수 있기에 필요한 경우 여러 클래스에서 동시에 상속할 수는 없다. 

```ts
class ITDepartment extends Department {
  contructor(id: stirng, public admins: string[]) { // 여기에서 추가된건 admins
    super(id, 'IT'); // Department에서 상속받아서 받아옴
  }
}

const accounting = new ITDepartment('d1', ['Bill']);
```

이렇게 해서 상속받아서 사용할 수 있으며 Department에서 사용할 수 있는 매서드 또한 상속되서 사용할 수 있다. 

##    
