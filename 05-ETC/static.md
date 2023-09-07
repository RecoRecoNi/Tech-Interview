# static
## 💡 static 키워드는 어떤 의미를 갖나요?
`static` 키워드는 정적 변수(static variable)를 선언하기 위해서 사용하는 키워드입니다. **프로그램이 실행될 때 만들어지는 변수**로 프로그램 전체에 고정되어 있는 변수라고 할 수 있습니다. 프로그램 전체에 고정되어 있기 때문에 정적 변수는 **프로그램의 생성과 함께 할당되고, 프로그램의 종료 시에 해제**됩니다.

<br>

## 📑 꼬리질문
### 컴파일 할  때, static 키워드가 붙은 변수와 함수는 어떻게 처리되나요?
- `static` 키워드로 정의된 변수와 함수는 컴파일 시간에 초기화되고, 해당 범위 내에서 메모리에 상주하며 실행 시간에 특정 조건을 만족할 때까지 메모리에 남아있습니다.
- 함수 내부의 정적 변수는 함수 호출 사이에서 상태를 유지하고, 클래스 내부의 정적 멤버 변수는 클래스의 모든 인스턴스에서 공유됩니다.
- 일반적으로 **Class는 메모리의 Static 영역에 생성되고, 일반 객체는 Heap 영역에 생성**됩니다.
    - 자동으로 메모리 할당 해제를 수행하는 Java에서 Heap영역의 메모리는 가비지 컬렉터를 통해 수시로 관리받습니다.
    - `static` 키워드가 붙은 변수와 함수는 **Class와 마찬가지로 static 영역에 할당**되어 가비지 컬렉터의 영향 없이 모든 인스턴스가 공유 가능합니다.

#### C++에서 static
- C++에서 함수 내부 또는 클래스 내부에서 `static`키워드로 정의된 정적 변수는 **컴파일 시간**에 초기화되며, 해당 변수는 프로그램의 실행 동안 메모리에 상주하고 유지됩니다.
- 정적 변수는 메모리의 모든 범위 내에서 공유되므로 특정 함수 범위에서도 값이 유지됩니다.
```c++
// 정적 변수 예제
#include <iostream>void myFunction() {
    static int x = 0; // 정적 변수
    x++;
    std::cout << "x: " << x << std::endl;
}   

int main() {
    myFunction(); // x: 1
    myFunction(); // x: 2
    return 0;
}
```

#### Java에서의 static
- Java에서 `static`키워드는 C++와 마찬가지로 **함수 내부 또는 클래스 내부에서 변수와 메서드를 정의할 때 사용**됩니다. 
- 클래스 변수와 메서드는 클래스의 모든 인스턴스에서 공유되며, 객체를 생성하지 않고 클래스 이름을 통해 접근할 수 있습니다.
``` java
public class MyClass {
    public static int staticVariable = 10;
}

public class Main {
    public static void main(String[] args) {
        MyClass.staticVariable = 20;
        System.out.println("MyClass.staticVariable: " + MyClass.staticVariable); // 20
    }
}
```
<br>


## 📚 Reference
[유튜브 - JAVA 객체 지향 프로그래밍 - 6. static](https://www.youtube.com/watch?v=hvTuZshZvIo) 

[티스토리 - [java/자바] Static이란? Static정리](https://mi-nya.tistory.com/251)

[티스토리 - **[Java] Static 키워드 바로 알고 사용하자**](https://vaert.tistory.com/101)