# Modern_Java
>    함수형 인터페이스란 무엇인가
>
        함수형 인터페이스는 정확히 하나의 추상 메서드를 지정하는 인터페이스이다
        default method 가 여러개 있더라도 추상메서드가 오직 하나일 경우 그것은 함수형 인터페이스이다
        함수형 인터페이스를 사용하면 코드가 다음과 같은 이점을 얻을 수 있다
            - 유연성: 필요한 동작을 구현하는 람다 표현식을 작성하여 함수형 인터페이스를 구현할 수 있다
            - 재사용성: 동일한 동작을 여러 함수에서 사용할 수 있다
            - 테스트 가능성: 함수형 인터페이스는 하나의 추상 메서드만 정의하기 때문에 테스트하기 쉽다
        결론 : 함수형 인터페이스를 사용하면 "동작 파라미터화"를 유연하게 할수있게된다.
        
        여기서 동작파라미터화란?
        동작 파라미터화(Behavior Parameterization)는 메서드가 호출될 때 어떤 동작을 수행할지 결정하는 책임을 메서드 외부로 전달하는 기법이다 
        이 기법을 사용하면 동일한 메서드를 사용하여 다양한 동작을 수행할 수 있다
        ex) 
            사과가있다 그런데 사과를 초록색 사과만 고를수 있게 만들어야한다
            그러면 코드는 대충 이렇게 만들수가 있다
            List<Apple> greenApple(List<Apple> appleBox) {
                List<Apple> result = new ArrayList<>();
                for (Apple apple : appleBox) {
                    if (apple.color.eqauls(GREEN)) {
                        result.add(apple);
                    }
                }
                return result;
            }

            이렇게 코드를 만들었는데 갑자기 고객이 요구사항이 더 추가가되면 어떡할까? 예를들어 초록색사과에 무게가 10 이상인것만 추가해줘라고 하면
            코드를 복사 붙여넣기 해서 만들수있다 하지만 요구사항이 이것보다 더 +α 가 된다면 곤란해질것이다 
            " 그럴때 함수형 인터페이스를 사용하여 동작파라미터화를 해주면 된다 "
            static <T> List<T> appleValidator(List<T> appleBox, Predicate<T> p) { // 제네릭 메서드 사용
                List<T> result = new ArrayList<>();
                for (T apple : appleBox) {
                    if (p.test(apple)) {
                        result.add(apple);
                    }
                }
                return result;
            }
            이렇게 코드를 작성하면 어떤 상황에도 유연하게 대처할수 있을것이다
        
>    자바 함수형 인터페이스 표준 API
>
        Supplier<T>, T get()
        매개변수 X, 반환값 O
        
        Consumer<T>,  void accept(T t)
        매개변수 O, 반환값 X
        
        Function<T, R>, R apply(T t)
        매개변수 O, 반환값 O <R>
        
        Predicate<T>, boolean test(T t)
        매개변수 O, 반환값 O <boolean>   
        
>    함수 디스크립터 != 메서드 시그니처
>
        함수형 인터페이스의 추상 "메서드 시그니처" 는 람다 표현식의 "시그니처"를 가리킨다.
        람다 표현식의 "시그니처" 를 서술하는 메서드를 "함수 디스크립터" 라고 부른다

        ex) int add(int a, int b);
        메서드 시그니처(반환값X) = (int, int)
        함수 디스크립터(반환값O) = (int, int) -> int

        ex) 함수 디스크립터
             Runnable 인터페이스의 추상 메서드 void run() 은 인수와 반환값이 없으므로 이렇게 나타낼수 있다 () -> void
             Supplier<T> 인터페이스의 추상메서드 T get()은 인수가 없고 반환값 <T>가 있으므로 이렇게 나타낼수 있다 () -> T
             Consumer<T> 인터페이스의 추상메서드 void accept(T t)는 인수<T> 와 반환값이 없으므로 이렇게 나타낼수 있다 (T) -> void
             Function<T, R> 인터페이스의 추상 메서드 R apply(T t) 는 인수<T> 와 반환값 <R>이 있으므로 이렇게 나타낼수 있다 (T) -> R
             Predicate<T> 인터페이스의 추상메서드 boolean test(T t) 는 인수<T> 와 반환값 boolean이 있으므로 이렇게 나타낼수 있다 (T) -> boolean

             ex) 추가
                 interface Cal {
                     int calMethod(int a, int b);
                 }
                이 경우의 함수형 인터페이스인 Cal의 추상메서드인 calMethod의 함수 디스크립터는 이렇게 나타낼수 있다
                    (int, int) -> int
        

>    람다 형식검사, 형식추론, 제약
>
        람다가 사용되는 "콘텍스트" 를 이용해서 람다의 형식을 추론할 수 있다. 어떤 콘텍스트(예를 들면 람다가 전달될 메서드 파라미터나 람다가 할당되는 변수 등)에서 
        기대되는 람다 표현식의 형식을 "대상 형식" 이라고 부른다
        
        여기서 콘텍스트란?
            람다 콘텍스트는 람다 표현식을 실행하는 데 필요한 정보를 제공하는 객체이다 
            람다 콘텍스트는 람다 표현식의 유형, 범위, 함수형 인터페이스의 추상 메서드의 시그니처 등과 같은 정보를 제공한다
            ex)
                public static <T> List<T> filter(List<T> list, Predicate<T> predicate) {
                    List<T> filteredList = new ArrayList<>();
                    for (T t : list) {
                        if (predicate.test(t)) {
                            filteredList.add(t);
                        }
                    }
                    return filteredList;
                }

                타입: Predicate<T>
                범위: filter() 함수의 스코프
                함수형 인터페이스의 추상 메서드의 시그니처: boolean test(T t) = (T t) -> boolean
                이렇게 볼 수 있다

        여기서 대상형식이란?
            대상 형식은 람다 표현식이 사용되는 위치에 따라 결정되는 함수형 인터페이스의 타입이다 
            따라서, 대상 형식은 인터페이스를 의미한다고 볼 수 있다
        
        ex)
            List<Apple> heavierThan159g = filter(inventory, (Apple apple) -> apple.getWeight() > 150);
            이렇게 코드가 있을 경우 위 코드의 형식 확인 과정을 보여준다
                1. filter 메서드의 선언을 확인한다
                2. filter 메서드는 두 번째 파라미터로 Predicate<Apple> 형식(대상 형식)을 기대한다
                3. Predicate<Apple>은 boolean test(T t) 라는 한 개의 추상 메서드를 정의하는 함수형 인터페이스다
                4. test 메서드는 Apple을 받아 boolean을 반환하는 함수 디스크립터를 묘사한다.
                5. filter 메서드로 전달된 인수는 이와 같은 요구사항을 만족해야 한다
>    형식추론
>
    자바 컴파일러는 람다 표현식이 사용된 콘텍스트를 이용해서 람다 표현식과  관련된 함수형 인터페이스를 추론한다. 즉,
    대상 형식을 이용해서 함수 디스크립터를 알 수 있으므로 컴파일러는 람다의 시그니처도 추론할 수있다.
    결과적으로 컴파일러는 람다 표현식의 파라미터 형식에 접근할 수 있으므로 람다 문법에서 이를 생략할 수 있다.
    즉, 자바 컴파일러는 다으머럼 람다 파라미터 형식을 추론할 수 있다
    List<Apple> greenApples = 
            filter(inventory, (apple) -> GREEN.equals(apple.getColor())); // 파라미터 apple에 형식을 명시적으로 지정하지 않았다
    ex)
        Comparator<Apple> c = (Apple a1, Apple a2) -> a1.getWeight() - a2.getWeight(); // 형식을 추론하지 않음
        Comparator<Apple> c = (a1, a2) -> a1.getWeight() - a2.getWeight(); // 형식을 추론함

> 지역 변수 사용(제약)
>
    람다 표현식에서는 익명 함수가 하는 것처럼 자유변수(파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수)를 활용할 수 있다.
    이와 같은 동작을 "람다 캡처링" 이라고 부른다
    int portNumber = 1337;
    Runnable r = () -> Systme.out.println(portNumber);

    하지만 자유 변수에도 약간의 제약이 있다. 람다는 인스턴스 변수와 정적 변수를 자유롭게 캡처(자신의 바디에서 참조할 수 있도록)할 수있다.
    하지만 그러려면 " 지역 변수는 명시적으로 final로 선언되어 있어야 하거나 실질적으로 final로 선언된 변수와 똑같은 사용되어야 한다 "
    이 소리가 왜냐하면
        우선 내부적으로 인스턴스 변수와 지역 변수는 태생부터 다르다. 인스턴스 변수는 "힙"에 저장되는 반면 지역변수는 "스택"에 위치한다
        " 자바 구현에서는 원래 변수에 접근을 허용하는 것이 아니라 자유 지역 변수의 복사본을 제공한다. 따라서 복사본의 값이 바뀌지 않아야 하므로
        지역 변수에는 한 번만 값을 할당해야 한다는 제약이 생긴 것이다 "
        ex) 여기서 자유 지역 변수의 복사본이란?
            int a = 1; id : 20487654;
            int b = 2; id : 45431431;
            a = a + b;
            a, id : 12341515;
            이렇게 복사본을 제공한다
        
        
>    자바 람다식 정리(함수형인터페이스)
>
        Predicate<T> 를 기준으로 작성하였다
        Stream<T> filter(Predicate<? super T> predicate )
        필터 내장 메서드를 사용해도 됨

        @FunctionalInterface // 함수형 인터페이스 어노테이션
        interface Predicate<T> { // 함수형 인터페이스
            boolean test(T t); // 추상 메서드
        }

        static <T> List<T> listValidator(List<T> list, Predicate<T> p) { // 제네릭 메서드 사용
            List<T> result = new ArrayList<>();
            for (T element : list) {
                if (p.test(element)) {
                    result.add(element);
                }
            }
            return result;
        }
        
        boolean isGreenApple() {
            return getColor().equals(GREEN);
        }

        실행 시킨 코드 
        ex)
            List<Apple> greenAppleBox = listValidator(appleBox, Apple::isGreenColor);

        < 해설 >
        1) 함수형 인터페이스 자리에 람다식 or 메서드참조를 작성한다
        (함수형 인터페이스 위치는 큰 따옴표로 표기 했음)
        listValidator(List<T> list, "Predicate<T> p");
        
        ex) 
            List<Apple> greenAppleBox = listValidator(appleBox, Apple::isGreenApple); // 메서드 참조
            List<Apple> greenAppleBox = listValidator(appleBox, (apple) -> apple.isGreenApple()); // 람다식
  
        2) 함수형 인터페이스의 추상 메서드로 그 람다식 or 메서드 참조를 적용한다
        listValidator(List<T> list, Predicate<T> p) {
            ...코드생략
            if (p.test(element));{    // 함수형 인터페이스 추상메서드, 함수형 인터페이스 위치에 작성한 람다식 or 메서드 참조가 추상메서드로 인해 적용된다
                result.add(element);
            }              
        }

> Stream
    
