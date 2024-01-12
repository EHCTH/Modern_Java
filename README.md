# Modern_Java
>    함수형 인터페이스란 무엇인가
>
        함수형 인터페이스는 정확히 하나의 추상 메서드를 지정하는 인터페이스이다
        defual method 가 여러개 있더라도 추상메서드가 오직 하나일 경우 그것은 함수형 인터페이스이다
        함수형 인터페이스를 사용하면 코드가 다음과 같은 이점을 얻을 수 있다
            - 유연성: 필요한 동작을 구현하는 람다 표현식을 작성하여 함수형 인터페이스를 구현할 수 있다
            - 재사용성: 동일한 동작을 여러 함수에서 사용할 수 있다
            - 테스트 가능성: 함수형 인터페이스는 하나의 추상 메서드만 정의하기 때문에 테스트하기 쉽다
        결론 : 함수형 인터페이스를 사용하면 동작 파라미터화를 유연하게 할수있게된다.
        
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
>    함수 디스크립터
>
        ㅇㄹㅇㄴㅁㄹ

>    람다 형식검사, 형식추론 제약
>
        ㅇㄹㅇㄴㄹ
        
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

    
