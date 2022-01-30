## 의존성 추가

- 프로젝트를 생성하고 pom.xml 파일에 아래와 같이 의존성을 추가한다.
- Java 9 이상에서는 JAXB API의 의존성도 추가 시켜야한다.
  - 확장자가 xml인 JPA 설정 파일을 클래스로 변경할 때 JAXB가 필요한데 Java 9 이상부터 JAXB API는 표준 Java SE에서 제거되었기 때문에 의존성을 직접 추가 시켜줘야 한다. 

```xml
<dependencies>
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>5.3.10.Final</version>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <version>1.4.200</version>
    </dependency>
</dependencies>
```

## persistence.xml 

아래 사진에 보이는 경로에 persistence.xml 파일을 추가해 준다.

![image-20220130200710456](images\image-20220130200710456.png)

```xml
<persistence-unit name="hello">
    <properties>
        <!-- 필수 속성 -->
        <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
        <property name="javax.persistence.jdbc.user" value="sa"/>
        <property name="javax.persistence.jdbc.password" value=""/>
        <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
        <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>

        <!-- 옵션 -->
        <property name="hibernate.show_sql" value="true"/>
        <property name="hibernate.format_sql" value="true"/>
        <property name="hibernate.use_sql_comments" value="true"/>
        <property name="hibernate.hbm2ddl.auto" value="create" />
    </properties>
</persistence-unit>
```

`<property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>` : 데이터베이스 방언을 설정한다.

## 데이터베이스 방언

- 데이터베이스 방언 : 특정 데이터베이스에만 있는 기능을 사용할 수 있도록 해준다.
- JPA는 특정 데이터베이스에 종속되지 않도록 만들어졌기 때문에 특정 데이터베이스에 있는 기능을 사용하려면 방언을 설정해 줘야 하기 때문에 옵션 같지만 사실상 필수 속성이다.
- 데이터베이스 방언은 아래와 같이 데이터베이스마다 제공하는 SQL 문법과 함수가 조금씩 다른 경우에 사용된다.
  - 가변 문자 : MySQL : varchar, Oracle : varchar2
  - 페이징 : MySQL : LIMIT, Oracle : ROWNUM

## JPA 구동 방식

![image-20220130202939063](images\image-20220130202939063.png)

- persistence.xml 파일의 설정 정보를 읽고 Persistence 클래스를 생성한다.
- Persistence 클래스에서 EntityManagerFactory를 생성한다.
- EntityManagerFactory에서 DB에 접근이 필요할 때마다 EntityManager를 생성한다.   

## JPA 실행

아래와 같이 코드를 작성한 후 실행에 성공한다면 설정이 완료된거다.

```JAVA
public static void main(String[] args) {
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
}
```

만약 위의 코드를 실행했는데 아래 사진과 같이 NoClassDefFoundError가 발생한다면 위에서 얘기한 jaxb-api의 의존성을 추가하면 해결된다.

 ![image-20220130203854916](images\image-20220130203854916.png)

실행에 성공했다면 이제부터는 위에서 얘기한 JPA 구동 방식을 직접 코드로 작성해 보겠다.

```java
public static void main(String[] args) {
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    EntityManager em = emf.createEntityManager();

    em.close();
    emf.close();
}
```

- `Persistence.createEntityManagerFactory("hello")` : persistence.xml에서 생성자에 입력한 문자열과 동일한 persistence-unit 태그의 이름을 찾고 설정 정보를 읽어서 EntityManagerFactory를 생성한다.
- `emf.createEntityManager()`: EntityManagerFactory에서 EntityManager를 생성한다.
- 이제부터 생성한 EntityManager를 통해서 데이터베이스에 접근시킬 수 있는데 그 전에 데이터베이스에 등록할 Entity를 정의해야 한다.

## 정리

- 이번 시간에는 JPA를 실행하기 위해 프로젝트 설정을 하고 데이터베이스 방언, JPA 구동 방식에 대해서 알아봤다.
- 다음 시간에는 Entity를 등록하는 방법을 알아보겠다.

## Reference
[자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)