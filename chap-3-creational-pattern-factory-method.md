```
팩토리 메서드 (factory method) - 클래스 생성 (Class creational)

의도
객체를 생성하기 위한 인터페이스를 정의하지만, 
어떤 클래스의 인스턴스를 생성할지에 대한 결정은 서브클래스가 내리도록 합니다.
=> 인터페이스와 구현의 분리

다른 이름
가상 생성자

동기
프레임워크는 추상클래스를 사용해 객체 간의 관련성을 정의하고 유지가능
=> 나중에 프레임워크 까볼 때 이런걸 위주로 봐야겠다 싶음
또한 프레임워크는 이들 객체를 생성할 책임을 지님
프레임워크는 이들 추상클래스 간의 상호작용을 책임져서 
전체 시스템의 기본 동작 방식을 정의합니다.
=> 프레임워크가 왜 프레임워크인지 설명해주는 글같음.. 라이브러리와의 차이점이고 또한
어떻게 보면 프레임워크를 공부하고 사용하면서 패턴이나 아키텍처에 대해서 배울수 있게 되지않나 생각
=> 결국은 추상 클래스들의 아키텍쳐와 마찬가지라는 것을 이해..
이들 추상클래스를 상속하는 서브클래스에서 구체적인 행동을 정의함으로써
새로운 응용프로그램을 만듭니다.

사용자에게 다양한 종류의 문서를 표현할 수 있는 응용프로그램 프레임워크가 있다고 간주
이를 위해서는 두 개의 큰 추상화가 필요
하나는 Application 클래스, 다른 하나는 Document 클래스
이 클래스들은 모두 추상클래스
사용자는 특정 응용프로그램에 종속적인 구현을 위해 새로운 서브클래스를 정의

그리기(Drawing) 관련 응용프로그램을 생성하려면 DrawingApplication 클래스와
DrawingDocument 클래스를 정의해야합니다.
Application 클래스는 Document 객체를 관리하는 책임을 맡음
필요에 따라 문서들을 생성하기도 합니다.
=> 위에서 말한 프레임워크가 객체를 생성할 책임
예를 들어,Document 객체의 생성이나 관리는 사용자가 메뉴에서 Open이나 New를 선택할 때 이루어지게 되겠지요.

어떤 응용프로그램을 만드느냐에 따라 인스턴스로 만들어야하는 Document의 서브클래스가 달라지기 때문에,
Application 클래스는 어떤 문서의 인스턴스를 생성해야하는지 미리 예측할 수 없습니다.
Application 클래스는 언제 문서의 인스턴스를 만들어야하는지만 알고 있을 뿐,
어떤 종류의 문서를 생성해야하는지는 알지 못합니다.

이때 문제에 봉착하게 됩니다.
프레임워크는 클래스를 인스턴스로 만들어야 하지만, 추상클래스 밖에 모르는 프레임워크는 클래스의 인스턴스화 작업을 수행할 수 없습니다.
왜냐하면 추상 클래스는 인스턴스를 가질 수 없기 때문입니다.

팩토리 메서드 패턴은 이런 문제에 대한 해법을 제시
이 패턴은 Document의 서브클래스 중 어느 것을 생성해야하는지에 대한 정보를 캡슐화하고,
그것을 프레임워크에서 떼어냅니다.

Application 클래스의 서브클래스는 추상화된 CreateDocument() 연산을 재정의하여 적당한 Document 클래스의 서브클래스를 반환하도록 합니다.
Application 클래스의 서브클래스가 인스턴스화된 응용프로그램에 따른 문서의 인스턴스가 됩니다.
이 때, CreateDocument() 연산을 가리켜 팩토리 메서드라고 하는데, 객체를 "제조하는(manufacture)" 방법을 알기 때문입니다.

활용성
팩토리 메서드는 다음과 같은 상황에 사용합니다.
- 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
- 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 때
- 객체 생성의 책임을 몇 개의 보조 서브클래스 가운데 하나에게 위임하고, 어떤 서브클래스가 위임자인지에 대한 정보를 국소화시키고 싶을 때

참여자
- Product(Document): 팩토리 메서드가 생성하는 객체의 인터페이스를 정의
- ConcreteProduct(MyDocument): Product 클래스에 정의된 인터페이스를 실제로 구현
- Creator(Application): Product 타입의 객체를 반환하는 팩토리 메서드를 선언, Creator 클래스는 팩토리 메서드를 기본적으로 구현하는데,
이 구현에서는 ConcreteProduct 객체를 반환합니다. 또한 Product 객체의 생성을 위해 팩토리 메서드를 호출합니다.
- ConcreteCreator(MyApplication): 팩토리 메서드를 재정의하여 ConcreteProduct의 인스턴스를 반환


협력 방법
- Creator는 자신의 서브클래스를 통해 실제 필요한 팩토리 메서드를 정의하여 적절한 ConcreteProduct의 인스턴스를 반환할 수 있게 합니다.

결과
팩토리 메서드 패턴은 응용프로그램에 국한된 클래스가 여러분의 코드에 종속되지 않도록 해줍니다.
응용프로그램은 Product 클래스에 정의된 인터페이스와만 동작하도록 코드가 만들어지기 때문에,
사용자가 정의한 어떤 ConcreteProduct 클래스와도 동작할 수 있게 됩니다.

구현
팩토리 메서드 패턴을 구현할 때는 다음 사항을 고려해야 합니다.
1. 구현 방법이 크게 두 가지입니다.
팩토리 패턴을 구현하는 두 가지 방식은
(1) Creator 클래스를 추상 클래스로 정의하고, 정의한 팩토리 메서드에 대한 구현은 제공하지 않은 경우와
(2) Creator가 구체클래스이고, 팩토리 메서드에 대한 기본 구현을 제공하는 경우
물론 기본 구현을 일부 정의한 추상 클래스로 정의할 수도 있지만, 흔한 일은 아닙니다.

추상클래스로 정의할 때는 구현을 제공한 서브클래스를 반드시 정의해야 합니다. 이때, 아직 예측할 수 없는 클래스들을 생성해야 하는 문제가 생깁니다.
"객체의 생성은 별도의 분리하여, 이 연산을 서브클래스에서 재정의하게 합니다."
이 규칙을 따르면, 서브클래스 설계자는 부모 클래스가 인스턴스를 만드는 객체의 클래스를 변경가능

2. 팩토리 메서드를 매개변수화합니다.
또 다른 구현 방식으로, 팩토리 메서드를 이용해서 여러 종류의 제품을 생성하는 방법도 있습니다.
팩토리 메서드가 매개변수를 받아서 어떤 종류의 제품을 생성할지 식별하게 만드는 것.

3. 언어마다 구현 방법이 조금 다를 수 있습니다.

4. 템플릿을 사용하여 서브클래싱을 피합니다. (C++)

5. 명명 규칙을 따르는 것도 매우 중요한 일입니다.
팩토리 메서드를 쓴다는 사실을 명확하게 만들어주는 명명 규칙을 따르는 좋은 습관을 들이도록 합시다.

관련 패턴
템플릿 메서드 패턴, 추상 팩토리 패턴은 이 팩토리 메서드를 이용해서 구현할 때가 많습니다.
추상 팩토리 패턴의 '동기'절에서도 팩토리 메서드의 모습을 볼수 있다.

원형 패턴은 Creator 클래스의 상속이 필요하지 않습니다.
그러나 Product 클래스에 정의된 초기화 연산은 필요합니다.
Creator 클래스는 객체의 초기화를 위해 초기화 연산을 사용하지만, 팩토리 메서드는 이런 연산 불필요

```
