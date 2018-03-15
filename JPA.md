## Spring Data JPA

### JPA
* 持久化技术
  * JDO 模式
  * Active Records
  * CMP 模式
  * ORM - JPA
 
* JPA
  * 官方文档 
 
* what is JPA 
  * JPA 规范
  * JPA 实现方案
 
> JPA(Java Persistence API) 是Sun官方提出的Java持久化规范。它为Java开发人员提供了一种对象/关联映射工具来管理Java应用中的关系数据。他的出现主要是为了简化现有的持久化开发工作和整合ORM技术，结束现在Hibernate，TopLink，JDO等ORM框架各自为营的局面。值得注意的是，JPA是在充分吸收了现有Hibernate，TopLink，JDO等ORM框架的基础上发展而来的，具有易于使用，伸缩性强等优点。
 
 
### Spring Data  
* what is Spring Data
 
> Spring Data JPA 是 Spring 基于 ORM 框架、JPA 规范的基础上封装的一套JPA应用框架，可使开发者用极简的代码即可实现对数据的访问和操作。它提供了包括增删改查等在内的常用功能，且易于扩展！学习并使用 Spring Data JPA 可以极大提高开发效率。

* 简单搭建

> Example : spring data rest

### 基本使用
* 简单查询


```java
public interface UserRepository extends JpaRepository<User, Long> 
{
}
```
``` java
@Test
public void testBaseQuery() throws Exception {
    User user=new User();
    userRepository.findAll();
    userRepository.findOne(1l);
    userRepository.save(user);
    userRepository.delete(user);
    userRepository.count();
    userRepository.exists(1l);
    // ...
}
```

> JpaRepository extends PagingAndSortingRepository which in turn extends CrudRepository.

> * CrudRepository mainly provides CRUD functions.
> * PagingAndSortingRepository provide methods to do pagination and sorting records.
> * JpaRepository provides some JPA related method such as flushing the persistence context and delete record in a batch.

* 自定义查询

![](http://incdn1.b0.upaiyun.com/2017/07/e2496ab9c089190f2334cb179f552161.png)

* 复杂查询 - 分页查询

``` java
Page<User> findALL(Pageable pageable);
     
Page<User> findByUserName(String userName,Pageable pageable);
```
Pageable 是spring封装的分页实现类，使用的时候需要传入页数、每页条数和排序规则.

``` java
@Test
public void testPageQuery() throws Exception {
    int page=1,size=10;
    Sort sort = new Sort(Direction.DESC, "id");
    Pageable pageable = new PageRequest(page, size, sort);
    userRepository.findALL(pageable);
    userRepository.findByUserName("testName", pageable);
}
```

* 复杂查询 - 限制查询

``` java
User findFirstByOrderByLastnameAsc();
 
User findTopByOrderByAgeDesc();
 
Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);
 
List<User> findFirst10ByLastname(String lastname, Sort sort);
 
List<User> findTop10ByLastname(String lastname, Pageable pageable);
```

* 复杂查询 - JPQL 和 Native Sql

在SQL的查询方法上面使用@Query注解，如涉及到删除和修改在需要加上@Modifying.也可以根据需要添加 @Transactional 对事物的支持，查询超时的设置等.

``` java
@Modifying
@Query("update User u set u.userName = ?1 where c.id = ?2")
int modifyByIdAndUserId(String  userName, Long id);
     
@Transactional
@Modifying
@Query("delete from User where id = ?1")
void deleteByUserId(Long id);
   
@Transactional(timeout = 10)
@Query("select u from User u where u.emailAddress = ?1")
    User findByEmailAddress(String emailAddress);
```

* 复杂查询 - 多表查询

第一种是利用hibernate的级联查询来实现，第二种是创建一个结果集的接口来接收连表查询后的结果。

``` java
public interface HotelSummary {
 
    City getCity();
    String getName();
    Double getAverageRating();
    default Integer getAverageRatingRounded() {
        return getAverageRating() == null ? null : (int) Math.round(getAverageRating());
    }
}

@Query("select h.city as city, h.name as name, avg(r.rating) as averageRating "
        - "from Hotel h left outer join h.reviews r where h.city = ?1 group by h")
Page<HotelSummary> findByCity(City city, Pageable pageable);
 
@Query("select h.name as name, avg(r.rating) as averageRating "
        - "from Hotel h left outer join h.reviews r  group by h")
Page<HotelSummary> findByCity(Pageable pageable);

Page<HotelSummary> hotels = this.hotelRepository.findByCity(new PageRequest(0, 10, Direction.ASC, "name"));
for(HotelSummary summay:hotels){
        System.out.println("Name" +summay.getName());
    }
```

* 复杂查询 - 动态查询

* JPA 2.0的Criteria API

  

``` java
 LocalDate today = new LocalDate();
 CriteriaBuilder builder = em.getCriteriaBuilder();
 CriteriaQuery<Customer> query
 builder.createQuery(Customer.class);
 Root<Customer> root = query.from(Customer.class);
 Predicate hasBirthday =   builder.equal(root.get(Customer_.birthday), today);
 Predicate isLongTermCustomer = builder.lessThan(root.get(Customer_.createdAt), today.minusYears(2);
 query.where(builder.and(hasBirthday, isLongTermCustomer));
 em.createQuery(query.select(root)).getResultList();
```

 * Specifications
 
> Eric Evans’ Domain Driven Design 一书中的概念衍生出来

``` java
public interface Specification<T> {
  Predicate toPredicate(Root<T> root, CriteriaQuery query, CriteriaBuilder cb);
}
```

``` java
public CustomerSpecifications {

  public static Specification<Customer> customerHasBirthday() {
    return new Specification<Customer> {
      public Predicate toPredicate(Root<T> root, CriteriaQuery query, CriteriaBuilder cb) {
        return cb.equal(root.get(Customer_.birthday), today);
      }
    };
  }

  public static Specification<Customer> isLongTermCustomer() {
    return new Specification<Customer> {
      public Predicate toPredicate(Root<T> root, CriteriaQuery query, CriteriaBuilder cb) {
        return cb.lessThan(root.get(Customer_.createdAt), new LocalDate.minusYears(2));
      }
    };
  }
}
```
``` java
public interface CustomerRepository extends JpaRepository<Customer>, JpaSpecificationExecutor {
  // Your query methods here
}

customerRepository.findAll(hasBirthday());
customerRepository.findAll(isLongTermCustomer());
customerRepository.findAll(where(customerHasBirthday()).and(isLongTermCustomer()));
```


 * Querydsl
 
 > 开源项目,不仅支持JPA，还支持Hibernate，JDO，Lucene，JDBC甚至是原始集合的查询.
 
 
``` java
 QCustomer customer = QCustomer.customer;
LocalDate today = new LocalDate();
BooleanExpression customerHasBirthday = customer.birthday.eq(today);
BooleanExpression isLongTermCustomer = customer.createdAt.lt(today.minusYears(2));
```

``` java
public interface CustomerRepository extends JpaRepository<Customer>, QueryDslPredicateExecutor {
  // Your query methods here
}

BooleanExpression customerHasBirthday = customer.birthday.eq(today);
BooleanExpression isLongTermCustomer = customer.createdAt.lt(today.minusYears(2));
customerRepository.findAll(customerHasBirthday.and(isLongTermCustomer));
```
 
