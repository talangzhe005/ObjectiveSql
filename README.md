The ObjectiveSql makes it easy to CRUD operations on databases(just define an Annotation). It is a best ActiveRecord pratice in Java, it includes everything about you need to build most SQL for business developing. At the same time, the SQL resused is possible in ObjectiveSql and the SQL abstracted in Java is more clear, more programable. 

### Features

- Defining a domain model with code generating automatically, which carries the query and persistence behavior by itself, no configuration, no empty Interface
- Validating the Java Bean with Jakarta Bean Validation integrated to ObjectiveSql
- Database transaction into an Annotation tagged on a method only
- The relations tagged with Annotation, who will be applied in query as a static field generated automatically

### Defining a DomainModel

```java
@DomainModel
public class Member {
    private String no;
    
    @Queryable
    private String name;
    private Integer gender;
    private String mobile;
    private String otherInfo;

    @Relation(relationType = RelationType.HAS_MANY)
    private List<Order> orders;
}

@DomainModel
public class Order {
    private String no;
    private Integer memberId;
    private Double amount;
    private Double quantity;

    @Relation(relationType = RelationType.BELONGS_TO)
    private Member member;
}
```

### The query methods below

```java
Member member = Member.queryByPrimaryKey(11);
```

```java
Member member = Member.queryFirst("id = ?", 11);
```

```java
List<Member> members = Member.query("id > ?", 8);
```

```java
List<Member> members = Member.queryAll();
```

```java
int count = Member.count("id > ?", 10);
```

```java
...
```

### The validation methods below

```java
try {
    Map<String, String> requestObject = ...; // From the POST request of SpringBoot
    Member rawMember = Member.newInstanceFrom(requestObject, false);
    newMember.validate(); //Skip the validation
} catch(ValidationException ex) {
    ...
}
```

### The persistence methods below

```java
Member newMember = new Member();
// To set the field value for "newMember"
newMember.save(false); //Skip the validation
// newMember.save(true); // Validating the field value before save
```

```java
Map<String, String> requestObject = ...; // From the POST request of SpringBoot
Member rawMember = Member.newInstanceFrom(requestObject, false);
newMember.save(false); //Skip the validation
// newMember.save(true); // Validating the field value before save
```