# **시간표 사이트 만들기**

시간표 사이트를 만들어 봅시다!
Html과 Css 지식이 전무하더라도, 자바스크립트의 기본 지식만 있다면 쉽게 따라오실 수 있습니다.

먼저, [여기](https://github.com/jaknndiius/Time-Table-Template)에서 `[Code ▾]`를 누른 뒤 `[Download ZIP]`을 눌러 파일을 다운로드하세요.<br>
_만약 `mycode.js`의 파일명을 변경하고 싶다면 변경 후 `index.html`의 9번째 줄의 `"mycode.js"` 도 동일하게 변경하세요._

이제 자바스크립트 작성을 시작하겠습니다.<br>
**`mycode.js` 파일을 여세요.**

### **1단계:` TimeTableAPI`를 import하기**

`TimeTableAPI`는 시간표 사이트를 만들기 위한 클래스/함수/변수들을 제공하는 API입니다.<br>
다음을 작성하여 import 하세요.

```typescript
import {} from "https://jaknndiius.github.io/TimeTableAPI/timeTableAPI.js";
```

- 다음부터 배울 `TimeTableAPI`에서 제공하는 (클래스/함수/변수)를 쓰려면 해당 (클래스/함수/변수) 이름을 대괄호 안에 작성해야 합니다.

### **2단계: 과목 생성하기**

과목은 변수에 할당한 뒤 사용합니다.<br>
과목은 유형에 따라 두가지 방법으로 생성할 수 있습니다.

#### **1. 기본 과목**

> 📌 import의 대괄호 안에 `Subject`를 추가해야 합니다.

```typescript
new Subject(subjectName: string, teacher: string)
```

- 물리, 창체, 음악처럼 과목 당 단 하나인 과목을 생성할 때 사용하세요.
- `Subject`로 생성한 과목은 화면에 표시될 때 이름 그대로 표시됩니다.

_예시_

```typescript
const History = new Subject("한국사", "000");
// 표시될 때 '한국사'로 표시되며 선생님 이름은 000입니다.
```

#### **2. 복수 과목**

> 📌 import의 대괄호 안에 `SubjectList`를 추가해야 합니다.

```typescript
new SubjectList(subjectName: string, teachers: string[], options: Object = {}) : (order: number) => MultipleSubject
```

- 수1, 수2와 과목 이름은 같지만 세분화 되는 과목을 생성할 때 사용하세요.
- `SubjectList`는 생성 시 함수를 반환합니다.
- 함수는 `order`를 받고, 해당 순서의 선생님과 과목명으로 `MultipleSubject`를 만들어 반환합니다.
- `order`는 `1`부터 시작하며 과목의 첫 글자와 접미사가 붙어 표시됩니다.
- `SubjectList`로 생성된 과목은 기본적으로 표시될 때 과목 앞글자와 숫자를 붙여 표시되며 이는 `options`로 변경 가능합니다.
- `options`는 생략 가능하며, 화면에 표시되는 이름을 변경할 수 있습니다.
- `options`에는 `suffixType`과 `fullName` 항목을 설정할 수 있습니다.
- `suffixType`은 기본값은 `SuffixType.NUMBER`이며 `SuffixType` 객체의 접미사들을 설정할 수 있습니다.
- > 📌 `SuffixType` 객체를 사용하기 위해선 import의 대괄호 안에 `SuffixType`를 추가하세요.
- `fullName`은 기본 값은 `false`며 만약 전체 이름 표시를 원하면 `true`를 설정할 수 있습니다.

_예시_

```typescript
const Literature = new SubjectList("문학", ["000", "111", "222"]);
console.log(`표시:${Literature(1)}, 선생님:${Literature(1).teacher}`);
// 표시: 문1, 선생님: 000
console.log(`표시:${Literature(2)}, 선생님:${Literature(3).teacher}`);
// 표시: 문2, 선생님: 111
console.log(`표시:${Literature(2)}, 선생님:${Literature(3).teacher}`);
// 표시: 문3, 선생님: 222

const Mathmatics = new SubjectList("수학", ["aaa", "bbb", "ccc"], {
  suffixType: SuffixType.ROMAN,
});
console.log(`표시:${Mathmatics(1)}, 선생님:${Mathmatics(1).teacher}`);
// 표시: 수Ⅰ, 선생님: aaa
console.log(`표시:${Mathmatics(2)}, 선생님:${Mathmatics(2).teacher}`);
// 표시: 수Ⅱ, 선생님: bbb
console.log(`표시:${Mathmatics(3)}, 선생님:${Mathmatics(3).teacher}`);
// 표시: 수Ⅲ, 선생님: ccc

const Info = new SubjectList("정보", ["xxx", "yyy"], {
  suffixType: SuffixType.ALPABET,
  fullName: true,
});
console.log(`표시:${Info(1)}, 선생님:${Info(1).teacher}`);
// 표시: 정보A, 선생님: xxx
console.log(`표시:${Info(2)}, 선생님:${Info(2).teacher}`);
// 표시: 정보B, 선생님: yyy
```

### **3단계: 시험 설정하기**

생성한 과목에 시험 정보를 추가합니다.

> 📌 import의 대괄호 안에 `ExamAttribute`를 추가하세요.

```typescript
(과목).setExam(examAttribute: ExamAttribute)
```

- 과목은 `Subject`, `SubjectList`, `MultipleSubject` 모두 가능합니다.
- `SubjectList`에는 '문학' 처럼 복수 과목이지만 따로 시험을 나누지 않을 때 설정하면 되며,
- `MultiPleSubject`에는 '수1', '수2' 처럼 복수과목인데 시험을 나누는 과목일 때 설정하면 됩니다.
- `ExamAttribute`는 다음과 같이 생성합니다.
- ```typescript
  new ExamAttribute(selective: number, descriptive: number)
  ```
  - 객관식, 서술형 갯수 정보를 가진 `ExamAttribute` 객체를 생성합니다.
- ```typescript
  ExamAttribute.addRange(range: string) : ExamAttribute
  ```
  - 과목의 범위 설명 한 줄을 추가하고 자신을 다시 반환합니다.
  - 자신을 다시 반환하기 때문에, `ExamAttribute`.`addRange`().`addRange`()... 처럼 연쇄적으로 작성할 수 있습니다.
  - 따로 변수에 저장할 필요 없이 바로 `setExam` 메소드에 전달하면 됩니다.

_예시_

```typescript
// Subject에 시험 설정
History.setExam(
  new ExamAttribute(20, 4)
    .addRange("교과서 처음부터 끝까지")
    .addRange("배부한 학습지 전체")
);
// SubjectList에 시험 설정
Literature.setExam(
  new ExamAttribute(25, 2).addRange("교과서 전체").addRange("부교재 전체")
);
// MultipleSubject에 시험 설정
Mathmatics(1).setExam(new ExamAttribute(16, 4).addRange("교과서 싹 다"));
```

### **4단계: 모의고사 설정하기**

모의고사 날짜를 추가하면 시간표 아래 작은 공간에 가장 가까운 모의고사 날짜의 D-day를 알려줍니다.<br>
생략이 가능합니다. 작성하지 않으면 모의고사 정보 칸이 생성되지 않습니다.

> 📌 import의 대괄호 안에 `Setting`를 추가하세요.

```typescript
Setting.addMoakTest(dateFormat: string)
```

- `YYYY/MM/DD 형식`으로 작성하세요. 구분자로 '`-`'을 쓰면 IOS 환경에서 오류가 발생합니다.

_예시_

```typescript
Setting.addMoakTest("2023/03/23");
Setting.addMoakTest("2023/06/01");
Setting.addMoakTest("2023/09/06");
Setting.addMoakTest("2023/11/21");
```

### **5단계: 교시 별 시간 설정**

각 교시가 시작되는 시간을 설정해야 합니다.<br>
생략할 수 있지만, 생략하면 시간에 따라 각 교시에 맞는 과목이 강조되는 기능이 작동하지 않습니다.

> 📌 import의 대괄호 안에 `Setting`를 추가하세요. 위에서 추가했다면 다시 추가하지 않아도 됩니다.

```typescript
Setting.setClassTime(className: ClassName, hours, minutes)
```

- > 📌 import의 대괄호 안에 `ClassName`를 추가하세요.
- `ClassName` 객체를 사용하세요. `ClassName.CLASS1`~`7`, `ClassName.END`를 이용해 어느 교시의 시작 시간인지 설정할 수 있습니다.
- 전 교시가 끝나는 시간으로 설정하는것이 좋습니다.
- 만약, 1교시가 9:00~9:50이고, 쉬는시간이 10분, 2교시가 10:00에 시작할 때
  `ClassName`.`CLASS2`를 10:00으로 설정하면 10:00부터 2교시 과목이 강조되지만,
  `ClassName`.`CLASS2`를 9:50으로 설정하면 쉬는 시간부터 다음 교시인 2교시 과목이 강조되어 다음 교시를 쉽게 확인할 수 있습니다.
- `ClassName.CLASS1`~`7`, `ClassName.END` 중 하나라도 설정하지 않으면 아예 작동되지 않습니다. 설정시 반드시 모두 작성하세요.

_예시_

```typescript
Setting.setClassTime(ClassName.CLASS1, 8, 0);
// 1교시는 아침시간 포함 -> 아침시간에도 1교시 과목이 강조됨.
// 이 시간 전에는 강조되는 과목 없음
Setting.setClassTime(ClassName.CLASS2, 9, 20);
// 1교시 마치고 바로 2교시 과목이 강조됨
Setting.setClassTime(ClassName.CLASS3, 10, 20);
// 2교시 마치고 바로 3교시 과목이 강조됨
Setting.setClassTime(ClassName.CLASS4, 11, 20);
// 3교시 마치고 바로 4교시 과목이 강조됨
Setting.setClassTime(ClassName.CLASS5, 12, 20);
// 4교시 마치고 점심시간~5교시 동안 5교시 과목이 강조됨
Setting.setClassTime(ClassName.CLASS6, 14, 0);
// 5교시 마치고 바로 6교시 과목이 강조됨
Setting.setClassTime(ClassName.CLASS7, 15, 0);
// 6교시 마치고 바로 7교시 과목이 강조됨
Setting.setClassTime(ClassName.END, 16, 0);
// 이 시간 이후로는 강조되는 과목 없음.
```

### **6단계: 과목 그룹화 밑 시간표 설정**

시간표를 채워넣기 위해선 과목 그룹이 필요합니다.

> 📌 import의 대괄호 안에 `Setting`를 추가하세요. 위에서 추가했다면 다시 추가하지 않아도 됩니다.

```typescript
Setting.group(...subjects: ...Subject | SubjectList | MultipleSubject): SubjectGroup
```

- 과목그룹은 `Setting`.`group` 메소드를 사용하며 `SubjectGroup `객체를 반환합니다.
- 과목은 아까 생성한 변수를 전달하세요.

그룹을 생성했다면 이제 시간표를 설정해야 합니다.<br>
시간표는 두가지 종류가 있습니다.

- **정규시간표**: 필수입니다. 작성하지 않을 시 정규 시간표가 덜 채워진 채 나타납니다.
  ```typescript
  SubjectGroup.setToRegularSchedule(day: Day)
  ```
  - 정규 시간표에서 해당 요일(Day)의 과목을 설정할 수 있습니다.
  - 그룹화 된 순서대로 1교시, 2교시 등으로 설정됩니다.
  - > 📌 import의 대괄호 안에 `Day`를 추가하세요.
  - `Day` 객체를 사용하세요. `Day`.`MONDAY`, `Day`.`THEUSDAY` 등등 요일 항목을 제공합니다.
  - 정규 시간표에서 과목을 클릭하면 선생님 이름을 보여줍니다.
- **시험 시간표**: 시험이 없다면 생략가능합니다. 시험 시간표 자체가 생성되지 않습니다.
  ```typescript
  SubjectGrouop.setToExamSchedule(month: number, date: number)
  ```
  - 시험 시간표에서 해당 날짜(`month`/`date`)의 과목을 설정할 수 있습니다.
  - 그룹화 된 순서대로 1교시, 2교시 등으로 설정됩니다.
  - 만약 자습시간이 있다면, API에서 제공하는 `SelfStudy`를 사용하세요.
  - > 📌 `SelfStudy`를 사용하기 위해선 import의 대괄호 안에 `SelfStudy`를 추가하세요.
  - 시험 시간표에서 과목을 클릭하면 해당 과목에 저장된 `ExamAttribute`를 기반으로 알림창을 생성합니다.
  - `setExam` 메소드를 통해 과목에 `ExamAttribute`를 설정한 뒤 과목을 추가해 주세요.
  - `SelfStudy`로 전달한 자습시간은 클릭해도 알림이 생성되지 않습니다.

_예시_

```typescript
Setting.group(Mathmatics(1), History, Literature(3)).setToRegularSchedule(
  Day.MONDAY
);
// 수1, 한국사, 문3 과목을 월요일 시간표로 설정합니다.
Setting.group(Literature(1), Mathmatics(2), Info(1)).setToRegularSchedule(
  Day.THEUSDAY
);
// 문1, 수2, 정보1 과목을 화요일 시간표로 설정합니다.

Setting.group(History, Literature, Mathmatics(3)).setToExamSchedule(5, 1);
// 한국사, 문학, 수3 과목을 5월 1일 시험 시간표로 설정합니다.
Setting.group(SelfStudy, Literature(2), Mathmatics).setToExamSchedule(5, 2);
// 자습, 문2, 수학 과목을 5월 2일 시험 시간표로 설정합니다.
```

### **7단계: 페이지 로드하기**

이제 설정이 완료되었다면, 페이지를 로딩할 시간입니다.

> 📌 import의 대괄호 안에 `loadPage`를 추가하세요.

```typescript
loadPage();
```

- 모든 설정을 끝내고 이 함수를 실행해 주세요.
- 호출하지 않을 시 페이지가 생성되지 않습니다.
- 설정이 완료되기 전 호출 시 시간표가 완전히 표시되지 않을 수 있습니다.

_예시_

```typescript
loadPage();
```

### **8단계: 정리**

필수 요소
|필수 메소드|필요 갯수|
|---|---|
|[SubjectGroup.setToRegularSchedule()](#6단계-과목-그룹화-밑-시간표-설정)|5개(월~금)|
|[loadPage()](#7단계-페이지-로드하기)|1개|

| 생략 가능한 메소드                                                     | 필요 갯수       | 생략시                           |
| ---------------------------------------------------------------------- | --------------- | -------------------------------- |
| [Setting.setClassTime()](#5단계-교시-별-시간-설정)                     | 8개             | 시간 별 과목 강조 기능 작동 안됨 |
| [Setting.addMockTest()](#4단계-모의고사-설정하기)                      | 모의고사 수만큼 | 모의고사 정보 칸 생성 안됨       |
| [SubjectGrouop.setToExamSchedule()](#6단계-과목-그룹화-밑-시간표-설정) | 시험일 수만큼   | 시험 시간표 생성 안됨            |

### **9단계: 사이트 만들기**

이제 끝입니다.<br>
만든 `index.html`과 `mycode.js` 파일을 호스팅 사이트에 올리면 언제든 접속 가능한 사이트를 만들수 있습니다.<br>
사이트를 만들려면 `Github Page`를 추천합니다.

1. 먼저. `Github` 계정을 생성하고 새 `repository`를 생성하세요.
2. 우리가 만든 `index.html`과 `mycode.js` 파일을 업로드 하세요.
3. `Settings`의 `Pages` 탭을 누르세요.
4. `Branch`에서 `master`와 `/(root)` 를 선택하고 `Save`를 누른 뒤 기다리세요.
5. [ ```https://계정이름.github.io/Repository이름/``` ] 으로 접속하세요.
