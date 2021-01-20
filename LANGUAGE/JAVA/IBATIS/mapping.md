# MyBatis 매핑

## 1. selectOne

- SqlSession의 selectOne 메소드를 이용하는 방법
- selectOne 메소드는 하나의 값만 가져올 경우에 사용

- 첫번째 매개변수는 mapper의 namespace . select 태그의 id 값
- 두번째 매개변수는 Object 형식으로 변수(옵션)

```java
String name = session.selectOne("student.selectName", studentNo);
```

```xml
<select id="selectName" parameterType="_int" resultType="string">
    SELECT STUDENT_NAME FROM STUDENT WHERE STUDENT_NO = #{studentNo}
</select>
```

- parameterType은 파라미터의 타입
- resultType 은 반환 값의 타입

## 2. 데이터를 객체로 받기 (as 사용)

```xml
<select id="selectOne" parameterType="_int" resultType="student">
SELECT student_no as studentNo, student_name as studentName, student_tel as studentTel FROM STUDENT WHERE STUDENT_NO = #{studentNo}
</select>
```

- 데이터베이스에서 데이터를 가져올 때 저장할 객체의 속성 이름과 같도록 as 를 이용하여 별칭을 주는 방법
- 객체와 동일하게 적었을 경우 객체로 정보가 알아서 저장
- resultType 에 student 라고 바로 입력할 수 있는 것은 mybatis-config.xml 에 typeAlias 를 넣어주었기 때문(이 글을 참고)

## 3. result map 으로 매핑하기

```xml
<select id="selectOneAjax_" parameterType="_int" resultMap="selectMap">
SELECT \* FROM STUDENT WHERE STUDENT_NO = #{studentNo}
</select>
```

- resultMap 에는 사용할 rersultMap의 id 값

resultMap을 만들어 보자.
mapper 에서 resultMap 태그를 생성한 후
resultMap 태그 안에 result 태그로 데이터베이스의 컬럼과 프로퍼티를 매핑시킨다.
프로퍼티는 객체 속성의 이름을 넣어준다.

```xml
<resultMap type="student" id="selectMap">
  <result column="student_no" property="studentNo"/>
  <result column="student_name" property="studentName"/>
  <result column="student_tel" property="studentTel"/>
</resultMap>
```

- resultMap 의 type 은 클래스 명
- as를 사용하지 않고 객체로 데이터를 받아올 수 있음

## 4. result map 에서 hashMap 으로 가져오기

- 이 방법은 반환 값이 Map

```xml
<select id="selectOneAjax_" parameterType="_int" resultMap="selectMap">
SELECT * FROM STUDENT WHERE STUDENT_NO = #{studentNo}
</select>
```

- select 태그는 동일하지만 resultMap 태그가 단순

```xml
<resultMap type="hashMap" id="selectMap"></resultMap>
```

- resultMap의 type에 hashMap을 넣어주고 id 값
- Map 으로 반환 되는데 key 값이 데이터베이스 컬럼 명과 동일
- 따라서 반환된 값을 jsp 에서 사용할 경우에는 다음과 같이 key 값을 대문자

```html
<p>학생번호: ${result.STUDENT_NO}</p>
<p>학생이름: ${result.STUDENT_NAME}</p>
<p>전화번호: ${result.STUDENT_TEL}</p>
```

## 5. List로 데이터 받기 (selectList)

- SqlSession 의 selectList 메소드를 이용

```java
List<Student> list = session.selectList("student.selectList");
```

mapper에서도 하나만 받을 때와 다를 것 없음

```xml
<select id="selectList" resultType="student">
  SELECT student_no as studentNo, student_name as studentName, student_tel as studentTel, student_email as studentEmail, student_addr as studentAddress, reg_date as redDate FROM STUDENT
</select>
```

## 6. List를 Map으로 받기

- 객체(vo)를 사용하지 않고 데이터를 받을 수 있음

```java
List<Map<String, String>> list = session.selectList("student.selectMap");
```

- 위에서 사용한 resultMap을 이용한다.

```xml
<select id="selectMap" resultMap="selectMapList">
  SELECT * FROM STUDENT
</select>
<resultMap type="map" id="selectMapList">
  <result column="student_no" property="studentNo"/>
  <result column="student_name" property="studentName"/>
  <result column="student_tel" property="studentTel"/>
</resultMap>
```

출처: <https://sinna94.tistory.com/entry/마이바티스-데이터-가져오는-방법>
