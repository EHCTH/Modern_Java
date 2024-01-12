# Modern_Java
>>  자바 람다식 정리(함수형인터페이스)
>>
        
        Predicate<T> 를 기준으로 작성하였다
        Stream<T> filter(Predicate<? super T> predicate )
        필터 내장 메서드를 사용해도 됨

        @FunctionalInterface // 함수형 인터페이스 에너테이션
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
            p.test(element); // 함수형 인터페이스 추상메서드
                            // 함수형 인터페이스 위치에 작성한 람다식 or 메서드 참조가 추상메서드로 인해 적용된다
        }

    
