> 스프링 프레임워크 

스프링 프레임워크 개발할때 대부분(?)

start spring 이라고 구글에 검색하게됩니다.

[https://start.spring.io/](https://start.spring.io/)

[##_Image|kage@b8wfNn/btsiNbc5kMJ/1fuvcX5x4abvNYK6ZonMTk/img.png|CDM|1.3|{"originWidth":1531,"originHeight":819,"style":"alignCenter","filename":"스크린샷 2023-06-07 오전 11.05.02.png"}_##]

2023.06.07 기준입니다.

Springboot version 벌써 3.1.0 , 2.7.12 많은 업데이트가 있었네요

> Version 변경 과정

Spring Boot 프로젝트의 버전 번호는 SemVer (Semantic Versioning) 체계를 따르며, 이는 주.부.수 (Major.Minor.Patch)의 세 가지 부분으로 나뉩니다. 3.1.0와 2.7.12의 경우

주(major) 버전 : 3,2

부(minor) 버전 : 1,7

수(patch) 버전 : 0,12

1.  Major 버전은 주요한 변경 사항을 나타냅니다. 이 변경 사항들은 대체로 하위 호환성을 깨뜨리는 큰 변화가 포함되어 있습니다.
2.  Minor 버전은 새로운 기능이 추가되거나 기존 기능이 개선되었음을 나타냅니다. 이러한 변경 사항들은 일반적으로 하위 호환성을 유지합니다.
3.  Patch 버전은 주로 버그 수정이나 보안 패치와 같은 소규모 변경사항을 나타냅니다.

위에 미세팁에 공유한것 처럼 Springboot 3.x.x 버전과 Springboot 2.x.x 버전은 major 변경으로

3.x.x 버전부터 Java17이상부터 동작하며 Intellij 설정부터 변경이 필요한상황입니다.

> Intellij 프로젝트 Springboot 3.x.x, Java17설정하기

```
Settings > Build, Execution, Deployment > Gradle 
Gradle JVM : 17 버전 
```

1) Gradle JVM 변경 없다면 다운로드 받아주세요

[##_Image|kage@NTRit/btsiX2TNmqF/irabg6AB8TzE6KkaMeDUuk/img.png|CDM|1.3|{"originWidth":1804,"originHeight":1400,"style":"alignCenter","filename":"242904203-67c9c691-9d14-4455-ab84-049ca4a33275.png"}_##]

2) 프로젝트 설정정보

```
File > Project Structure > Project
```

[##_Image|kage@prSwg/btsi2d1xUJk/DSpRNeT0HLj9pApxt7ALM1/img.png|CDM|1.3|{"originWidth":2552,"originHeight":1390,"style":"alignCenter","filename":"242904742-018c2520-d232-4165-b1b3-fd7f934fe7b2.png"}_##]

3) SDKs Java 변경

[##_Image|kage@ecyzGs/btsiX1lM8G4/ghEBMO6xYiNnGOCXjEoopK/img.png|CDM|1.3|{"originWidth":2558,"originHeight":1398,"style":"alignCenter","filename":"242904797-1c626e84-88fb-49f8-9fa7-61ef84dfd18e.png"}_##]

해당 내용들 변경후 Gradle 다시 갱신하시고 받고나서도 정상적으로 프로젝트 구동이안되신다면 다시 Reload 해보시기 바랍니다.

Springboot 3.x.x버전 변경된 부분중 JPA Entity import가 변경되었습니다.

Springboot 2.x.x JPA Entity

```
import javax.persistence.*;
```

Springboot 3.x.x JPA Entity

```
import jakarta.persistence.*;
```

> 변경사유  
> Java EE의 소유권 이전과 관련이 있습니다.

Java EE는 오라클에서 개발하고 유지 관리하던 기술로, 여기에는 JPA(Java Persistence API)와 같은 여러 API가 포함되어 있습니다.  
그러나 오라클은 2017년에 Java EE의 소유권을 Eclipse 재단에게 넘겼습니다. 그 결과, Eclipse 재단은 이 기술을 Jakarta EE로 개명하고, 관련 패키지 이름을 javax.\*에서 jakarta.\*로 변경하기로 결정했습니다.

이러한 이유로 Spring Boot 2.x.x에서는 javax.persistence.\* 패키지를 사용했지만, Spring Boot 3.x.x 버전에서는 jakarta.persistence.\* 패키지를 사용하게 되었습니다.

기능적으로는 javax.persistence와 jakarta.persistence는 동일합니다. 이는 단지 패키지의 이름만 변경되었을 뿐이지, 내부의 클래스나 인터페이스, 그리고 그들이 제공하는 기능은 동일하게 유지되었습니다. 그러나 이 패키지 이름 변경은 Java EE 및 Jakarta EE 관련 코드를 업데이트해야 하는 작업을 의미합니다.

예를 들어, import javax.persistence.Entity;를 import jakarta.persistence.Entity;로 변경해야 할 수 있습니다.

이와 같은 변화는 전체적인 마이그레이션 프로세스의 일부입니다.


> Springboot 3.x.x 에서 JAVA17을 도입한 이유?

Spring Boot 3.x.x이 Java 17을 필요로 하는 구체적인 이유에 대해서는 다양한 가능성이 있습니다:

1.  **LTS(Long-Term Support)** **버전**: Java 17은 LTS(Long-Term Support) 버전입니다. 이는 Oracle이 제공하는 지원 주기 동안 업데이트와 보안 패치를 받을 수 있다는 것을 의미합니다. LTS 버전은 일반적으로 안정성이 검증되었으며, 향후 몇 년 동안 지원을 받을 수 있기 때문에, 기업 환경에서 널리 사용됩니다.
2.  **새로운** **언어** **기능**: Java 17에는 패턴 매칭, 레코드, 씰드 클래스 등 새로운 언어 기능이 많이 추가되었습니다. 이러한 새로운 기능들은 개발자의 생산성을 향상시키고 코드의 품질을 개선할 수 있습니다.
3.  **성능** **향상** **및** **개선사항**: 새 버전의 Java는 종종 성능 향상, 가비지 컬렉션 개선, 새로운 API 등을 포함하며, 이는 Spring Boot 애플리케이션의 성능과 안정성을 향상시킬 수 있습니다.
4.  **모듈화**: Java 9부터 도입된 Java Platform Module System(JPMS)는 애플리케이션의 크기를 줄이고 성능을 향상시키는 데 도움이 됩니다. 이는 특히 클라우드 기반 애플리케이션에 유용합니다

> 패턴 매칭 (Pattern Matching)

패턴 매칭은 주로 instanceof 연산자와 함께 사용되며, 간결하고 안전한 다운캐스팅을 지원


Java 16 이전

```
if (obj instanceof String) {
    String str = (String) obj;
    // Use str
}
```

Java 17 이후

```
if (obj instanceof String str) {
    // Use str directly
}
```

> 레코드 (Records)

Java 14부터 레코드는 클래스가 주로 데이터를 보유하기 위한 것임을 표시하는 새로운 타입의 클래스를 선언하는 데 사용됩니다. 레코드 클래스는 자동으로 모든 필드에 대한 게터, 생성자, equals(), hashCode(), toString() 메서드를 제공합니다.


예를 들어, 다음과 같은 레코드 클래스를 선언할 수 있습니다

```
record Point(int x, int y) {}
```

이는 불변하며, getX(), getY(), equals(), hashCode(), toString() 메서드가 자동으로 제공됩니다.

> 씰드 클래스 (Sealed Classes)

Java 15부터 씰드 클래스는 클래스 또는 인터페이스가 허용된 하위 타입을 제한하는 데 사용됩니다. 이를 통해 도메인 모델을 더 정확하게 표현하고, 패턴 매칭과 함께 사용하면 더 강력한 타입 검사를 수행할 수 있습니다.

예를 들어, 다음과 같은 씰드 클래스를 선언할 수 있습니다

```
public sealed abstract class Shape
    permits Circle, Rectangle {
}

public final class Circle extends Shape {
    // ...
}

public final class Rectangle extends Shape {
    // ...
}
```

Shape은 Circle 또는 Rectangle만 상속할 수 있습니다. 다른 클래스는 Shape을 상속할 수 없습니다. 이는 패턴 매칭과 결합하여 더 안전한 코드를 작성할 수 있습니다.