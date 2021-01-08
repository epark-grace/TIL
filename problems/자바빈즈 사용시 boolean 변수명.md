# 자바빈즈 사용시 boolean 변수명

## 문제
-   컨트롤러에서 모델에 담은 객체의 프로퍼티를 EL로 출력하는 중 예외 발생
-   `javax.el.PropertyNotFoundException: 타입 [dev.park.e.dto.Book]에서 프로퍼티 [isFinished]을(를) 찾을 수 없습니다.`

> 기존 코드
```java
public class Book {
    private boolean isFinished;
    private boolean isForAdult;

    public boolean isFinished() {
        return isFinished;
    }

    public void setFinished(boolean finished) {
        isFinished = finished;
    }

    public boolean isForAdult() {
        return isForAdult;
    }

    public void setForAdult(boolean forAdult) {
        isForAdult = forAdult;
    }
}
```
```html
<tbody>
  <c:forEach var="book" items="${bookList}">
    <tr>
      <td>${book.title}</td>
      <td>${book.categoryId}</td>
      <td>${book.author}</td>
      <td>${book.publisher}</td>
      <td>${book.volume}</td>
      <td>${book.shelfName}</td>
      <td>${book.rowNumber}</td>
      <td>${book.isFinished}</td>
      <td>${book.isForAdult}</td>
    </tr>
  </c:forEach>
</tbody>
```

---

## 해결
-   JSTL과 EL에서 객체에 접근할 때는 자바빈즈의 관례에 따른 getter/setter를 통해 프로퍼티를 사용
-   자바빈 boolean 타입의 변수명을 XXX로 지은 경우 gettet/setter 명명 규칙은 isXXX/setXXX
-   위 문제가 발생한 코드의 경우 boolean 변수를 isXXX으로 짓고, EL에서 isXXX으로 사용했기 때문에 isIsXXX getter를 통해 프로퍼티에 접근했지만 작성된 자바빈 코드에는 isIsXXX라는 메소드명의 getter가 없기 때문에 발생하는 문제
-   **boolean 변수명을 isXXX로 지은 경우 intelliJ에서 자동으로 만드는 getter/setter 메소드명은 isIsXXX/setIsXXX가 아닌 isXXX/setXXX이기 때문에 주의 필요**
-   **boolean 변수를 isXXX이 아닌 XXX로 변경**, EL에서 XXX로 사용하니 isXXX getter를 찾아 정상적으로 출력됨
-   getter를 isIsXXX으로 변경하는 방법도 있으나 문법적으로 맞지 않으므로 boolean 변수명을 변경하는 것으로 선택

> 수정 코드
```java
public class Book {
    private boolean finished;
    private boolean forAdult;

    public boolean isFinished() {
        return finished;
    }

    public void setFinished(boolean finished) {
        this.finished = finished;
    }

    public boolean isForAdult() {
        return forAdult;
    }

    public void setForAdult(boolean forAdult) {
        this.forAdult = forAdult;
    }
}
```
```html
<tbody>
  <c:forEach var="book" items="${bookList}">
    <tr>
      <td>${book.title}</td>
      <td>${book.categoryId}</td>
      <td>${book.author}</td>
      <td>${book.publisher}</td>
      <td>${book.volume}</td>
      <td>${book.shelfName}</td>
      <td>${book.rowNumber}</td>
      <td>${book.finished}</td>
      <td>${book.forAdult}</td>
    </tr>
  </c:forEach>
</tbody>
```
