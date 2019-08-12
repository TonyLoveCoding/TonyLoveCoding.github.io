---
title: Lombok入门教程
categories: 教程
tags:
  - Lombok
  - java
abbrlink: 5992
date: 2019-08-12 23:08:26
---
## 0、Lombok是什么

***Lombok***项目是一个自动继承到代码编辑器以及编译工具的Java类库，让你的Java项目更有意思。通过添加注解，我们就不用再编写**get/setter**方法、**equals**方法、**toString**方法，甚至流式编程的**builder模式**也能自动生成，以及自动生成**日志变量log**等等。

## 1、安装使用

以IntelliJ IDEA为例：

1. 点击"File" -> "Settings" -> "Plugins"；
2. 点击"Browse repositories"；
3. 搜索"Lombok Plugin"；
4. 点击"Install plugin"；
5. 重启你的IDEA；

其他IDE安装方法请参考[官方文档](https://projectlombok.org/"Lombok官方文档")。

## 2、常用注解

* **@Data**

  注解在类上，为类起到了`@ToString`, `@EqualsAndHashCode`, 对所有变量的`@Getter` , 对非final变量的`@Setter` , and `@RequiredArgsConstructor`注解的作用。

* **@Builder**

  提供了以Builder模式生成一个对应对象的作用。

* **@Log4j**

  注解在类上, 为类提供一个属性名为 log 的 Log4j的日志对象。

* **@Slf4j**

  注解在类上, 为类提供一个属性名为 log 的 Slf4j的日志对象。

* **@ToString**

  为类提供toString方法，默认为全部参数。

  **@EqualsAndHashCode**

  为类提供Equals，默认为全部参数进行比较，可以在变量标识`@EqualsAndHashCode.Exclude`来排除变量比较；

  为类提供HashCode的实现，可以指定boolean字段callSuper为true以调用父类的HashCode方法。

* **@NoArgsConstructor**

  为类提供无参的构造方法。

* **@RequiredArgsConstructor**

  为类提供包含final变量、标识了`@NonNull`注解的构造方法。

  可以为该注解指定String字段staticName，将生成以staticName为方法名的构造方法。

* **@AllArgsConstructor**

  为类提供全参的构造方法。

* **@NonNull**

  注解在参数上, 如果该类参数为 null , 就会报出异常,  throw new NullPointException(参数名)。

* **@Getter(lazy=true)**

  注解在参数上，以单例模式的双重校验饿汉式获取某一个对象，适合用于创建耗时较长的对象变量，使用AtomicReference来做缓存。

## 3、代码示例

### 3.1、使用`@Data`注解

```java
@Data public class DataExample {
  private final String name;
  @Setter(AccessLevel.PACKAGE) private int age;
  private double score;
  private String[] tags;
  
  @ToString(includeFieldNames=true)
  @Data(staticConstructor="of")
  public static class Exercise<T> {
    private final String name;
    private final T value;
  }
}
```

Java对比：

```java
public class DataExample {
  private final String name;
  private int age;
  private double score;
  private String[] tags;
  
  public DataExample(String name) {
    this.name = name;
  }
  
  public String getName() {
    return this.name;
  }
  
  void setAge(int age) {
    this.age = age;
  }
  
  public int getAge() {
    return this.age;
  }
  
  public void setScore(double score) {
    this.score = score;
  }
  
  public double getScore() {
    return this.score;
  }
  
  public String[] getTags() {
    return this.tags;
  }
  
  public void setTags(String[] tags) {
    this.tags = tags;
  }
  
  @Override public String toString() {
    return "DataExample(" + this.getName() + ", " + this.getAge() + ", " + this.getScore() + ", " + Arrays.deepToString(this.getTags()) + ")";
  }
  
  protected boolean canEqual(Object other) {
    return other instanceof DataExample;
  }
  
  @Override public boolean equals(Object o) {
    if (o == this) return true;
    if (!(o instanceof DataExample)) return false;
    DataExample other = (DataExample) o;
    if (!other.canEqual((Object)this)) return false;
    if (this.getName() == null ? other.getName() != null : !this.getName().equals(other.getName())) return false;
    if (this.getAge() != other.getAge()) return false;
    if (Double.compare(this.getScore(), other.getScore()) != 0) return false;
    if (!Arrays.deepEquals(this.getTags(), other.getTags())) return false;
    return true;
  }
  
  @Override public int hashCode() {
    final int PRIME = 59;
    int result = 1;
    final long temp1 = Double.doubleToLongBits(this.getScore());
    result = (result*PRIME) + (this.getName() == null ? 43 : this.getName().hashCode());
    result = (result*PRIME) + this.getAge();
    result = (result*PRIME) + (int)(temp1 ^ (temp1 >>> 32));
    result = (result*PRIME) + Arrays.deepHashCode(this.getTags());
    return result;
  }
  
  public static class Exercise<T> {
    private final String name;
    private final T value;
    
    private Exercise(String name, T value) {
      this.name = name;
      this.value = value;
    }
    
    public static <T> Exercise<T> of(String name, T value) {
      return new Exercise<T>(name, value);
    }
    
    public String getName() {
      return this.name;
    }
    
    public T getValue() {
      return this.value;
    }
    
    @Override public String toString() {
      return "Exercise(name=" + this.getName() + ", value=" + this.getValue() + ")";
    }
    
    protected boolean canEqual(Object other) {
      return other instanceof Exercise;
    }
    
    @Override public boolean equals(Object o) {
      if (o == this) return true;
      if (!(o instanceof Exercise)) return false;
      Exercise<?> other = (Exercise<?>) o;
      if (!other.canEqual((Object)this)) return false;
      if (this.getName() == null ? other.getValue() != null : !this.getName().equals(other.getName())) return false;
      if (this.getValue() == null ? other.getValue() != null : !this.getValue().equals(other.getValue())) return false;
      return true;
    }
    
    @Override public int hashCode() {
      final int PRIME = 59;
      int result = 1;
      result = (result*PRIME) + (this.getName() == null ? 43 : this.getName().hashCode());
      result = (result*PRIME) + (this.getValue() == null ? 43 : this.getValue().hashCode());
      return result;
    }
  }
}
```



### 3.2、 使用`@Getter(lazy=true)`注解

```java
public class GetterLazyExample {
  @Getter(lazy=true) private final double[] cached = expensive();
  
  private double[] expensive() {
    double[] result = new double[1000000];
    for (int i = 0; i < result.length; i++) {
      result[i] = Math.asin(i);
    }
    return result;
  }
}
```

Java对比：

```java
public class GetterLazyExample {
  private final java.util.concurrent.AtomicReference<java.lang.Object> cached = new java.util.concurrent.AtomicReference<java.lang.Object>();
  
  public double[] getCached() {
    java.lang.Object value = this.cached.get();
    if (value == null) {
      synchronized(this.cached) {
        value = this.cached.get();
        if (value == null) {
          final double[] actualValue = expensive();
          value = actualValue == null ? this.cached : actualValue;
          this.cached.set(value);
        }
      }
    }
    return (double[])(value == this.cached ? null : value);
  }
  
  private double[] expensive() {
    double[] result = new double[1000000];
    for (int i = 0; i < result.length; i++) {
      result[i] = Math.asin(i);
    }
    return result;
  }
}
```



### 3.3、 使用`@Builder`注解

```java
@Builder
public class BuilderExample {
  @Builder.Default private long created = System.currentTimeMillis();
  private String name;
  private int age;
  @Singular private Set<String> occupations;
}
```

Java对比：

```java
public class BuilderExample {
  private long created;
  private String name;
  private int age;
  private Set<String> occupations;
  
  BuilderExample(String name, int age, Set<String> occupations) {
    this.name = name;
    this.age = age;
    this.occupations = occupations;
  }
  
  private static long $default$created() {
    return System.currentTimeMillis();
  }
  
  public static BuilderExampleBuilder builder() {
    return new BuilderExampleBuilder();
  }
  
  public static class BuilderExampleBuilder {
    private long created;
    private boolean created$set;
    private String name;
    private int age;
    private java.util.ArrayList<String> occupations;
    
    BuilderExampleBuilder() {
    }
    
    public BuilderExampleBuilder created(long created) {
      this.created = created;
      this.created$set = true;
      return this;
    }
    
    public BuilderExampleBuilder name(String name) {
      this.name = name;
      return this;
    }
    
    public BuilderExampleBuilder age(int age) {
      this.age = age;
      return this;
    }
    
    public BuilderExampleBuilder occupation(String occupation) {
      if (this.occupations == null) {
        this.occupations = new java.util.ArrayList<String>();
      }
      
      this.occupations.add(occupation);
      return this;
    }
    
    public BuilderExampleBuilder occupations(Collection<? extends String> occupations) {
      if (this.occupations == null) {
        this.occupations = new java.util.ArrayList<String>();
      }

      this.occupations.addAll(occupations);
      return this;
    }
    
    public BuilderExampleBuilder clearOccupations() {
      if (this.occupations != null) {
        this.occupations.clear();
      }
      
      return this;
    }

    public BuilderExample build() {
      // complicated switch statement to produce a compact properly sized immutable set omitted.
      Set<String> occupations = ...;
      return new BuilderExample(created$set ? created : BuilderExample.$default$created(), name, age, occupations);
    }
    
    @java.lang.Override
    public String toString() {
      return "BuilderExample.BuilderExampleBuilder(created = " + this.created + ", name = " + this.name + ", age = " + this.age + ", occupations = " + this.occupations + ")";
    }
  }
}
```

使用方法：

```
BuilderExample example = BuilderExample.builder().name(name).age(age).occupations.(occupations).build（）；
```



## 4、使用lombok遇到的问题

​	目前我发现无法对Lombok的注解进行注解结合，如下代码将报错：

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.SOURCE)
@Data  //Lombok注解
public @interface MyPOJO{

}
```

​	希望知道原因的同学可以在评论区交流一下。



## 5、参考来源

> [Lombok官方文档](https://projectlombok.org"Lombok官方文档)
>
> [Lombok@Builder注解](https://cloud.tencent.com/developer/article/1419097"LomBok@Builder注解")



