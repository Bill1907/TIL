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
그리고 추가적으로 매서드나 속성을 추가 할 수 있는데 
```ts
class Department {
  private employ: string[] = [];

  constructor(public id: string, public name: string) {}
}

class ITDepartment extends Department {
  contructor(id: stirng, public admins: string[]) {
    super(id, 'IT'); // Department에서 상속받아서 받아옴
  }
  
  addEmployee(name: string) {
    if(name === 'Max') return;
    this.employ.push(name);
  }
}
```
위와 같은 방법으로 할 수 있다. 하지만 에러가 날것이다. 왜냐하면 Department클래스의 employ가 private 속성이기 때문이다. private속성은 정의된 클래스내에서만 접근이 가능하다.   
여기에서는 ```protected``` 속성을 사용할 수 있다. ```protected```속성은 ```private```속성과 비슷하면서 확장된 클래스에서도 접근이 가능하기 때문이다. 

```ts
class Department {
  protected employ: string[] = []; // protected 속성으로 수정

  constructor(public id: string, public name: string) {}
}

class ITDepartment extends Department {
  contructor(id: stirng, public admins: string[]) {
    super(id, 'IT'); // Department에서 상속받아서 받아옴
  }
  
  addEmployee(name: string) {
    if(name === 'Max') return;
    this.employ.push(name);
  }
}
```

## Getter & Setter

getter는 값을 가져올때 함수나 매스드를 실행하는 속성으로 개발자가 더 복잡한 로직을 실행할 수 있도록 도와준다. 

```ts

class AccountingDepartment extends Department {
  private lastReport: string;

  get mostRecentReport() {
    if (this.lastReport) {
      return this.lastReport;
    }
    throw new Error('No report found.');
  }

  set mostRecentReport(value: string) {
    if (!value) {
      throw new Error('Please pass in a valid value!');
    }
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }

  printReports() {
    console.log(this.reports);
  }
}

const accounting = new AccountingDepartment('d2', []);

accounting.mostRecentReport = 'Year End Report';
accounting.addReport('Something went wrong...');
```
위 예시에서 보이는 것처럼 getter 는 return 값이 있어야한다. 
그리고 setter 역시 사용방법은 위 예시와 같고 이어서 설정할 속성과 관련된 원하는 이름을 입력하면 된다. 
이들은 로직을 캡슐화하고 속성을 읽거나 설정하려 할 때 실행되어야하는 추가적인 로직을 추가하는 데 유용하다.

## 정적 매서드 & 속성

정적 속성과 메서드를 사용해 클래스의 인스턴스에 접근할 수 없는 속성과 매서드를 클래스에 추가 할 수 있다. 따라서 새 클래스에 이름을 먼저 호출하지 않고 클래스에 직접 접근한다. 
```ts 
class Department {
  static fiscalYear = 2022;
  contructor(private readonly id: string, public name: string) {
    // this 연산자로 fiscalYear 접근할 수 없다.  
  }
  static createEmployee(name: string) {
  return { name };
  }
}
console.log(Department.fiscalYear) // 2022
console.log(Department.createEmployee('Bill')) // { name: 'Bill' }
```
위 예시처럼 인스턴스를 만들지 않고 사용할 수 있으며 유의 해야할 점은 thtis는 static 요소들에 접근 할 수없다. 
왜냐하면 this는 클래스기반으로 생성된 인스턴스를 참조하기 때문이다. 정적 속성은 인스턴스에서 유효하지 않다. 정적 속성과 정적 매서드는 인스턴스와 분리되어 있기 때문이다. 

## Abstract Class

특정 클래스를 사용하여 작업하거나 특정 클래스를 확장 시키는 개발자들이 특정 매서드를 구현하거나 재정의 하도록 해야하는 경우가 있다. 
그런경우에는 ```abstract``` 속성을 사용해야한다.

```ts 
class Department {
  abstact describe(this: Department): void
}
```
위의 예시와 같이 추상적으로 describe에는 어떤 인수가 들어가야하며 어떤 값이 리턴되는지 정해져 있다. 이거에 맞춰서 상속받는 클래서에서 describe를 구성하도록 해야한다. 

```ts
class ITDepartment extends Department {
  constructor() {
    super();
  }
  describe() {
    console.log(this.id);
  }
}
```

## 싱글톤 패턴

class 에서 하나의 객체만 생성하게끔 할 수 있는데 이것을 싱글톤 패턴이라고 한다. 
```ts 
class AccountingDepartment extends Department {
  private static instance: AccountingDepartment;
  
  private constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }
  static getInstance() {
    if (AccountingDepartment.instance) {
      return this.instance;
    }
    this.instance = new AccountingDepartment('d2', []);
    return this.instance;
  }
}

const accounting = AccountingDepartment.getInstance();

```
