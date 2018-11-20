
## 목차
- 들어가면서
- 의도를 분명히 밝혀라
- 그릇된 정보를 피하라
- 의미 있게 구분하라
- 발음하기 쉬운 이름을 사용하라
- 검색하기 쉬운 이름을 사용하라
- 인코딩을 피하라
  - 헝가리안 표기법
  - 맴버 변수 접두어
  - 인터페이스와 구현
- 자신의 기억력을 자랑하지 마라
- 클래스 이름
- 메서드 이름
- 기발한 이름은 피하라
- 한 개념에 한 단어를 사용하라
- 말장난을 하지 마라
- 해법 영역 용어를 사용하자
- 문제 영역 용어를 사용하자
- 의미 있는 맥락을 추가하라
- 불필요한 맥락을 없애라
- 마치면서

----

## 들어가면서
- 소프트웨어에서 이름은 어디나 쓰인다.
  - 변수, 함수, parameter, 클래스, 패키지, 소스 파일, 디렉토리, jar파일, war파일, ear파일 등
  - 여러 곳에서 사용하므로 잘 지으면 편하다.

## 의도를 분명히 밝혀라
- 변수, 함수, 클래스의 이름은 아래 질문에 모두 답할 수 있어야 한다. (주석이 따로 필요하지 않을 정도로)
  - 존재 이유는?
  - 수행 기능은?
  - 사용 방법은?
- 예
  - 나쁜 예
    - int d; // 경과 시간(단위: 날짜)
  - 좋은 예
    - int elapsedTimeInDays;
    - int daysSinceCreation;
    - int daysSinceModification;
    - int fileAgeInDays;
- 예2
  - 나쁜 예
    ```java
    public List<int[]> getThem() {
        List<int[]> list1 = new ArrayList<int[]>();
        for (int[] x : theList) {
           if (x[0] == 4) {
              list1.add(x);
           }
        }
        return list1;
    }
    ```
    복잡한 코드는 아니나, 코드 맥락이 코드 자체에 명시적으로 드러나지 않는다.

  - 좋은 예
    ```java
    public List<int[]> getFlaggedCells() {
        List<int[]> flaggedCells = new ArrayList<int[]>();
        for (int[] cell : gameBoard) {
            if (cell[STATUS_VALUE] == FLAGGED) {
                flaggedCells.add(cell);
            }
        }
        return flaggedCells;
    }
    ```
    각 개념에 이름을 붙히니 코드가 더욱 명확해졌다.

  - 더 좋은 예
    ```java
    public List<Cell> getFlaggedCells() {
        List<Cell> flaggedCells = new ArrayList<Cell>();
        for (Cell cell : gameBoard) {
            if (cell.isFlagged()) {
                flaggedCells.add(cell);
            }
        }
        return flaggedCells;
    }
    ```
    int 배열을 class화 시키고, isFlagged 함수를 사용해 FLAGGED 상수를 감추니 더 명시적으로 바뀌었다.
## 그릇된 정보를 피하라
- 중의적으로 해석될 수 있는 이름은 지양하라.
- 개발자에게는 특수한 의미를 가지는 단어(List 등)는 실제 컨테이너가 List가 아닌 이상 accountList와 같이 변수명에 붙이지 말자. 차라리 accountGroup, bunchOfAccounts, accounts등으로 명명하자.
- 비슷해 보이는 명명에 주의하자. ( 알파벳 l과 숫자 1, 알파벳 O와 숫자 0)
## 의미 있게 구분하라
- 연속된 숫자를 덧붙여 쓰지말자.
- 나쁜 예
    ```java
    public static void copyChars(char a1[], char a2[]) {
        for (int i = 0; i < a1.length; i++) {
            a2[i] = a1[i];
        }
    }
    ```
- 좋은 예
    ```java
    public static void copyChars(char source[], char destination[]) {
        for (int i = 0; i < a1.length; i++) {
            destination[i] = source[i];
        }
    }
    ```
- 불용어(noise word)를 추가하지 말자.
  - Name, NameString
  - Customer, CustomerObject
  - 
    ```java
    getActiveAccount();
    getActiveAccounts();
    getActiveAccountInfo();
    ```
## 발음하기 쉬운 이름을 사용하라
- 나쁜 예
    ```java
    // Bad
    class DtaRcrd102 {
        private Date genymdhms;
        private Date modymdhms;
        private final String pszqint = "102";
        /* ... */
    };
    ```
- 좋은 예
    ```java
    // Good
    class Customer {
        private Date generationTimestamp;
        private Date modificationTimestamp;
        private final String recordId = "102";
        /* ... */
    };
    ```
## 검색하기 쉬운 이름을 사용하라
- e를 변수 이름으로 쓰지 마라.
- 상수를 이름으로 만들어라.
- 나쁜 예
    ````java
    for (int j=0; j<34; j++) {
        s += (t[j]*4)/5;
    }
    ````
- 좋은 예
    ```java
    int realDaysPerIdealDay = 4;
    const int WORK_DAYS_PER_WEEK = 5;
    int sum = 0;
    for (int j=0; j < NUMBER_OF_TASKS; j++) {
        int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
        int realTaskWeeks = (realTaskDays / WORK_DAYS_PER_WEEK);
        sum += realTaskWeeks;
    }
    ```
    이렇게 바꾸면 검색에도 굉장히 유용하다.
## 인코딩을 피하라(변수에 부가 정보를 덧붙이는 것을 피하라)
- 헝가리식 표기법
  - 요즘에는 컴파일러가 타입을 점검하고, IDE에서 타입을 바로 확인할 수 있다.
- 멤버 변수 접두어
  - 멤버 변수 접두어를 붙이지 말자(?)
  - m_dsc -> description
- 인터페이스 클래스와 구현 클래스
  - 인터페이스 클래스와 구현 클래스를 나눠야 한다면 구현 클래스의 이름에 정보를 인코딩하자.

    | Do / Don't | Interface class | Concrete(Implementation) class |
    | ---------- | --------------- | ------------------------------ |
    | Don't      | IShapeFactory   | ShapeFactory                   |
    | Do         | ShapeFactory    | ShapeFactoryImp                |
    | Do         | ShapeFactory    | CShapeFactory                  |

## 자신의 기억력을 자랑하지 마라
- 독자가 머리 속으로 한 번 더 생각해 변환해야 할 만한 변수명을 쓰지 말라.
- 똑똑한 프로그래머와 전문가 프로그래머를 나누는 기준 한 가지는 "명료함"이다.
## 클래스 이름
- 명사 혹은 명사구를 사용하라. (Customer, WikiPage, Account, AddressParser 등과 같이)
- Manager, Processor, Data, Info 등과 같은 단어는 피하자.
- 동사는 사용하지 않는다.
## 메서드 이름
- 동사 혹은 동사구를 사용하라. (postPayment, deletePage, save 등과 같이)
- 접근자, 변경자, 조건자는 앞에 get, set, is를 붙인다.
    ```java
    string name = employee.getName();
    customer.setName("mike");
    if (paycheck.isPosted())...
    ```
- 생성자를 오버로드할 때는 정적 팩토리 메서드를 사용하자.
    ```java
    // 첫 번째 보다 두 번쨰 방법이 더 좋다.
    Complex fulcrumPoint = new Complex(23.0);  
    Complex fulcrumPoint = Complex.FromRealNumber(23.0);
    ```
    > 정적 팩토리 메서드
    : 객체 생성을 캡슐화하는 기법. 객체를 생성하는 메소드를 만들고, static으로 선언하여 사용한다.
## 기발한 이름은 피하라
- 특정 문화에서만 사용되는 재미난 이름, 구어체, 속어 대신 의도를 분명히 표현하는 이름을 사용하라.
  - HolyHandGrenade → DeleteItems
  - whack() → kill() 
  - eatMyShort() -> Abort()
## 한 개념에 한 단어를 사용하라
- 추상적인 개념 하나에 단어 하나를 선택해 고수하라. (일관성)
  - fetch, retrieve, get 중에 하나만
  - controller, manager, driver 중에 하나만
## 말장난을 하지 마라
- (위와 같이) 한 개념에 한 단어를 사용하라고, 한 단어를 두 가지 목적으로 사용하지 마라.
    ```java
    // 더하기 함수
    public static int add(int a, int b)
    // 배열 뒤에 element 하나 추가
    public List<Element> add(Element element)
    ```
- 위는 아래와 같이 add를 insert나 append라는 이름으로 바꾸는게 적당하다.
    ```java
    public static int add(int a, int b)
    public List<Element> append(Element element)
    ```
- 코드는 읽는 사람이 최대한 이해하기 쉽도록 짜야한다.
## 해법 영역 용어를 사용하자
- 코드를 읽을 사람도 프로그래머이므로, 전산 용어, 알고리즘 이름, 패턴 이름, 수학 용어 등을 사용해도 좋다.
- 개발자라면 당연히 알고 있을 JobQueue, AccountVisitor(VISITOR 패턴) 등을 사용하지 않을 이유는 없다. (기술 개념에는 기술 이름이 가장 적합)
## 문제 영역 용어를 사용하자
- 적절한 프로그래머 용어가 없거나 문제영역과 관련이 깊은 용어의 경우 문제 영역 용어를 사용하자.
## 의미 있는 맥락을 추가하라
- 클래스, 함수, namespace 등으로 감싸서 맥락(Context)을 표현해라.
- 그래도 불분명하다면 접두어를 붙이자.(ex. addrFirstName, addrLastName, addrState)
- 나쁜 예
    ```java
    private void printGuessStatistics(char candidate, int count) {
        String number;
        String verb;
        String pluralModifier;
        if (count == 0) {
            number = "no";
            verb = "are";
            pluralModifier = "s";
        }  else if (count == 1) {
            number = "1";
            verb = "is";
            pluralModifier = "";
        }  else {
            number = Integer.toString(count);
            verb = "are";
            pluralModifier = "s";
        }
        String guessMessage = String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );

        print(guessMessage);
    }
    ```
  함수를 끝까지 읽어보고 나서야 number, verb, pluralModifier라는 변수 세 개가 통계 추측 메시지에 사용된다는 사실이 드러난다. (코드를 읽는 사람이 맥락을 유추해야만 한다.)
- 좋은 예
    ```java
    public class GuessStatisticsMessage {
        private String number;
        private String verb;
        private String pluralModifier;

        public String make(char candidate, int count) {
            createPluralDependentMessageParts(count);
            return String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );
        }

        private void createPluralDependentMessageParts(int count) {
            if (count == 0) {
                thereAreNoLetters();
            } else if (count == 1) {
                thereIsOneLetter();
            } else {
                thereAreManyLetters(count);
            }
        }

        private void thereAreManyLetters(int count) {
            number = Integer.toString(count);
            verb = "are";
            pluralModifier = "s";
        }

        private void thereIsOneLetter() {
            number = "1";
            verb = "is";
            pluralModifier = "";
        }

        private void thereAreNoLetters() {
            number = "no";
            verb = "are";
            pluralModifier = "s";
        }
    }
    ```
## 불필요한 맥락을 없애라
- Gas Station Deluxe(고급 휘발유 충전소)라는 애플리케이션을 짠다고 모든 클래스 이름 앞에 GSD를 붙이지는 말자. IDE에서는 G를 입력하고 자동 완성 키를 누르면 모든 클래스를 열거하므로, 효율적이지 못하다. AccountAddress 클래스 앞에 GSD를 붙이는 것 또한 이름이 부적절하다.
- 의미가 분명한 경우에 한해, 짧은 이름이 긴 이름보다 좋다.
## 마치면서
- 이 장에서 소개한 이름 규칙 몇 개를 적용해도 코드 가독성이 높아진다.
- 코드를 개선하려는 노력을 생활화해라.
- 단기적으로는 물론 장기적으로도 좋다.
