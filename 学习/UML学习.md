UML类图术语

- 关联关系：一般以类的属性形式出现在关联类中，在java中关联关系一般使用成员变量来实现。又分为聚合关系和组合关系。
- 单向关联

 ![3057657-0aa3184196f30cf0](/Users/kh/Desktop/3057657-0aa3184196f30cf0.png)

- 双向关联

![3057657-ef441413265cac30](/Users/kh/Desktop/3057657-ef441413265cac30.png)

- 依赖关系：依赖关系表示一个类依赖于另一个类的定义，一般依赖关系在java中提现为局部变量、方法的形参，或者对静态方法的调用，这是一种弱关系。

  - 代码描述

  ```
  class A{
  
  }
  
  class B{
  　　　　public void abc(A a){}//在B类中的没有A的属性，而是以形参的方式引入
  }
  ```

  



![img](https://img-blog.csdn.net/20160516104404230?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

- 聚合关系：类A拥有类B的实例化对象，属性可以独立存在



![聚合](/Users/kh/Desktop/聚合.png)

```
　class A{
　
　}
　　class B{
　　　　A a;//成员变量引入了A类
　　　　B(A a){
　　　　　　this.a = a;
　　　　}
　　}
```



- 组合关系：类A拥有类B的实例化对象，属性无法独立存在

  ![组合](/Users/kh/Desktop/组合.png)

  ```
  　class A{
  　
  　}
  　　class B{
  　　　　A a;//成员变量引入了A类
  　　　　B(){
  　　　　　　this.a = new A();
  　　　　}
  　　}
  ```

  

  