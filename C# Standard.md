C# 코딩 스타일
===============

> 이 문서는 [.Net 공식 코드 스타일 문서](https://github.com/dotnet/runtime/blob/main/docs/coding-guidelines/coding-style.md)를 번역 및 보강한 문서입니다.

>이 문서에 서술되지 않은 세부 지침은 [마이크로소프트 공식 홈페이지에 기술된 지침](https://learn.microsoft.com/ko-kr/dotnet/standard/design-guidelines/naming-guidelines)을 따릅니다.  

> C# 표준 코딩 스타일 초심자 혹은 한국어 사용자가 혼동할 수 있는 표현에 대해서는 각주를 포함합니다.

우리가 따르는 일반적인 규칙은 "Visual Studio 기본값 사용"입니다.

1. 우리는 [Allman 중괄호 스타일](http://en.wikipedia.org/wiki/Indent_style#Allman_style)을 사용합니다. 여기서 각 중괄호는 새 라인에서 시작합니다. 단일 라인 구문 블록은 중괄호 없이 사용할 수 있지만 블록의 경우에는 자체적인 라인에서 적절하게 들여쓰기 되어야하며 중괄호를 사용하는 다른 명령문 블록에 중첩되어서는 안됩니다 (자세한 내용은 18번 참고). 한 가지 예외는 중첩된 `using`에 통제되는 블록이 포함된 경우에도 동일한 들여쓰기 수준에서 다음 줄에서 시작하여 `using`문을 다른 `using`문 내에서 중첩시킬 수 있습니다.

    ```C#
    if (condition)
    {
        using fileStream = new FileStream(filename)
        using streamReader = new StreamReader(fileStream)
        {
            if (condition2)
                // ...
        }
    }
    ```
2. 우리는 4칸의 들여쓰기를 사용합니다 (탭 문자 사용 안함).
3. 우리는 internal 및 private 필드에서는 `_camelCase`를 사용하고 가능한 경우 `readonly`를 사용합니다.[^readonly] internal 및 private 인스턴스 필드의 접두사는 `_`, `static` 필드는 `s_` 스레드 static 필드는 `t_`로 시작합니다. `readonly`가 정적 필드에서 사용될 때 `static`뒤에 와야합니다 (예: `readonly static`이 아닌 `static readonly`). public 필드는 드물게 사용해야 하며 만약 사용시에는 접두사 없이 `PascalCase`를 사용합니다.[^public-field]
    ```C#
    private int _privateField;
    private static s_staticField;
    private static readonly s_readonlyStaticField = 0;
    
    public int PublicField;
    ```
4. 우리는 절대적으로 필요한 경우가 아니라면 `this.`를 피합니다.[^this]
    ```C#
    int field = 0;

    // BAD 😠
    void Method(int value)
    {
        this.field = value;
    }

    // NOT BAD 😅
    void Method(int field)
    {
        this.field = field;
    }
    
    // GOOD 😄
    void Method(int value)
    {
        field = value;
    }
    ```
5. 우리는 기본값이더라도 항상 접근지정자를 사용합니다 (예:`string _foo`가 아닌 `private string _foo`).
   접근지정자는 첫번째 수정자여야합니다. (예: `abstract public`가 아닌 `public abstract`)
    ```C#
    // BAD 😠
    abstract public class ExampleClass
    {
        string _foo;
        // ...
    }
    
    // GOOD 😄
    public abstract class ExampleClass
    {
        private string _foo;
    }
    ```
6. 네임스페이스 가져오기는 파일 상단, `namespace`선언의 *외부*에 지정해야합니다. 항상 위에 배치되는 `System.*`네임 스페이스를 제외하고 다른 모든 네임 스페이스들은 알파벳 순으로 정렬되어야합니다.
    ```C#
    // BAD 😠
    using MyNamespace;
    using System.Collections.Generic;
    using System;

    // GOOD 😄
    using System;
    using System.Collections.Generic;
    using MyNamespace;
    ```
7. 한 번에 두 개 이상의 빈 줄을 피합니다. 예를 들어 타입의 멤버 사이에 두 개의 빈 줄이 있으면 안됩니다.
    ```C#
    // BAD 😠
    private string _foo;


    private string _bar;
    // GOOD 😄
    private string _foo;
    
    private string _bar;
    ```
8. 불필요한 빈 공백을 피합니다.
   예를 들어 `if (someVar == 0)...`에서 점들로 표시된 불필요한 빈 공백울 피합니다.
9. 특정 코드 파일의 스타일이 이 지침과 다를 경우 (예: 비공개 멤버의 이름이 `_member`가 아닌 `m_member`임), 해당 파일의 기존 스타일을 우선 사용합니다.
10. 일반적으로 `new` 혹은 명시적 캐스트로 인하여 유형이 오른쪽에 명시적으로 명명된 경우에만 `var`를 사용합니다. 예를 들어 `var stream = OpenStandardInput()`이 아닌 `var stream = new FileStream(...)`입니다.
    - 마찬가지로 `new()`는 변수 정의문 혹은 필드 정의문에서 형식이 왼쪽에 명시된 경우에만 사용할 수 있습니다. 예를 들어 `stream = new()` (타입은 이전행에서 정의됨)가 아닌 `FileStream stream = new(...)`를 사용합니다.
11. 우리는 타입 참조와 메소드 호출 모두에 대해서 BCL 타입 대신 언어 키워드를 사용합니다 (예: `Int32, String, Single, Int32.Parse` 대신 `int, string, float, int.Parse` 등). 예제는 [#13976](https://github.com/dotnet/runtime/issues/13976) 이슈를 참조하세요.
12. 우리는 PascalCase를 사용하여 모든 상수인 로컬 변수와 필드의 이름을 지정합니다. 유일한 예외는 상수 값이 인터럽을 통해 호출하는 코드의 이름 및 값과 정확히 일치해야하는 인터럽 코드의 경우입니다.
    ```C#
    const int ConstantVariable = 0;
    ```
13. 우리는 로컬 함수를 포함해서 모든 메소드 이름에 PascalCase를 사용합니다.
14. 우리는 관련이 있을 경우 가능하면 ```"..."``` 대신에 ```nameof(...)```를 사용합니다.[^nameof]
15. 필드는 타입 선언 내에서 맨 위에 지정되어야합니다.
16. 소스코드에 ASCII가 아닌 문자를 포함할 때 리터럴 문자 대신에 유니코드 이스케이프 시퀀스 (\uXXXX)를 사용합니다. 비 ASCII 리터럴 문자는 때때로 도구나 편집기에 의해 왜곡됩니다.[^non-ascii]
17. 레이블을 사용할 때 (goto의 경우) 레이블을 현재 들여쓰기보다 하나 적게 들여씁니다.
    ```C#
        var helloWorld = "Hello, World!";
    PRINT:
        Console.WriteLine(helloWorld);
    ```
18. 단일 if 문을 사용할 때 다음 규칙을 따릅니다:
    - 단일 라인 폼을 사용하지 않습니다 (예: `if (source == null) throw new ArgumentNullException("source");`).
    - 중괄호 사용은 항상 허용되며 `if`/`else if`/.../`else` 복합문의 블록이 중괄호를 사용하거나 단일 문 본문이 여러 줄에 걸쳐있는 경우 필요합니다.
    - 중괄호는 `if`/`else if`/.../`else` 복합 문에 연결된 *모든* 블록의 본문이 한 줄에 있는 경우에만 생략할 수 있습니다.
19. 파생(상속)이 필요하지 않는 한 모든 internal 및 private 타입을 static 혹은 sealed로 명시합니다. 구현 세부 사항과 마찬가지로 향후 파생이 필요한 경우 변경할 수 있습니다.[^sealed]

[EditorConfig](https://editorconfig.org "EditorConfig homepage") 파일 (`.editorconfig`)이 런타임 저장소에 루트에 제공되어 위 지침에 따라 C# 자동 서식이 활성화됩니다.

또한 [.NET Codeformatter Tool](https://github.com/dotnet/codeformatter)를 사용하여 코드 베이스가 시간이 지남에 따라 일관된 스타일을 유지하도록 하고, 도구가 위에 설명된 지침을 준수하도록 코드 베이스를 자동으로 수정합니다.

### 예제 파일:

``ObservableLinkedList`1.cs:``

```C#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.ComponentModel;
using System.Diagnostics;
using Microsoft.Win32;

namespace System.Collections.Generic
{
    public partial class ObservableLinkedList<T> : INotifyCollectionChanged, INotifyPropertyChanged
    {
        private ObservableLinkedListNode<T> _head;
        private int _count;

        public ObservableLinkedList(IEnumerable<T> items)
        {
            if (items == null)
                throw new ArgumentNullException(nameof(items));

            foreach (T item in items)
            {
                AddLast(item);
            }
        }

        public event NotifyCollectionChangedEventHandler CollectionChanged;

        public int Count
        {
            get { return _count; }
        }

        public ObservableLinkedListNode AddLast(T value)
        {
            var newNode = new LinkedListNode<T>(this, value);

            InsertNodeBefore(_head, node);
        }

        protected virtual void OnCollectionChanged(NotifyCollectionChangedEventArgs e)
        {
            NotifyCollectionChangedEventHandler handler = CollectionChanged;
            if (handler != null)
            {
                handler(this, e);
            }
        }

        private void InsertNodeBefore(LinkedListNode<T> node, LinkedListNode<T> newNode)
        {
            ...
        }

        ...
    }
}
```

``ObservableLinkedList`1.ObservableLinkedListNode.cs:``

```C#
using System;

namespace System.Collections.Generics
{
    partial class ObservableLinkedList<T>
    {
        public class ObservableLinkedListNode
        {
            private readonly ObservableLinkedList<T> _parent;
            private readonly T _value;

            internal ObservableLinkedListNode(ObservableLinkedList<T> parent, T value)
            {
                Debug.Assert(parent != null);

                _parent = parent;
                _value = value;
            }

            public T Value
            {
                get { return _value; }
            }
        }

        ...
    }
}
```

다른 언어의 경우 현재 최고의 지침은 일관성입니다. 파일을 편집할 때, 새 코드와 변경 사항을 파일의 기존 스타일과 일관되게 유지하십시오. 새 파일의 경우 해당 구성 요소의 스타일을 준수해야 합니다. 완전히 새로운 요소가 있다면 합리적으로 널리 받아들여지는 것은 무엇이든 좋습니다.

[^readonly]: `readonly`를 무리하게 사용할 필요는 없습니다. 초기화 이후 값의 변경이 일어나지 않을 것이라고 예측되는 부분에 추가하면됩니다.

[^public-field]: 가급적 public 필드보다는 프로퍼티 사용을 권장합니다.

[^this]: 비주얼스튜디오 및 Rider에서는 불필요한 this 사용시 제거하는 것을 유도합니다.

[^nameof]: 타입명을 문자열로 명시하는 것보다 `nameof(...)`로 서술하는 것이 추후 리팩토링시에 도움이됩니다.

[^non-ascii]: 국제적 개발 환경에서 주로 발생하는 문제로, 일부 환경에서 비 ASCII 문자를 읽지 못하는 문제가 발생할 수 있습니다. UTF-8 에서 대부분 해결되는 문제이지만 국내 한정 개발을 할때 간혹 인코딩이 비 유니코드 인코딩으로 지정되는 경우가 있습니다. 사실 모국어가 영어가 아닌 이상 주석을 영어만으로 작성하기에는 무리가 따르므로 인코딩을 하나로 통일하는 것이 더 중요합니다.

[^sealed]: static 클래스나 sealed 클래스는 .Net 런타임에서 최적화에 긍정적인 영향을 주는 것으로 알려져있습니다.
