# effective java

第五条 避免创建不必要的对象

why : 复用对象，提高效率

1. 避免无意识的类型转换，主要发生在基本类型的自动装箱上,例如int->Interger.
alibaba java规约上约定局部变量一律用基本类型，成员变量用对象。

2. 避免重复创建对象，尤其是在for循环中。
对于不变对象，可多用final修饰。
Interger 的缓存哲学

3. 注意抽取可变对象中重量级的不可变对象。

4. 不要盲目的抽取不可变对象，相信jvm的优化。

第六条 消除过期对象的引用
why : 节约内存，防止内存泄露

1. 从c++带过来的习惯不能丢，java虽然有内存回收，但写代码的时候对对象的声明周期要心中有数。

2. 清空过期引用 object = null;

3. 对自己管理内存的行为，要特别关注此问题。

4. 缓存也需要关注此类问题。

第七条 避免使用终结方法
why : 该方法执行的具有不确定性，不能依赖finalizer来做一些收尾工作。
finalizer 会增加创建和销毁对象的开销

1. 如果需要一些收尾工作，可现实提供finish方法。惯用法

```java
try{
    dosomethine();
}catch(){

}finally{
    finish();
}
```

2. finalizer 不同于c++的析构函数，区别在两点
首先finalizer不保证执行，其次finalizer不具备链式调用属性。
c++中执行子类的析构函数回递归调用其父类的析构函数，调用顺序与构造函数的顺序相反。

3. 补充一点c++析构函数的用法。
RAII（resource acquisition is initialization），意为“资源获取即初始化”。其核心是把资源和对象的生命周期绑定，对象创建获取资源，对象销毁释放资源。在RAII的指导下，C++把底层的资源管理问题提升到了对象生命周期管理的更高层次。


```C++
class SmartPoint{
private:
    Object* m_pObject;

public:
    void set(Objec* pObject){
        m_pObject = pObject
    }
    ~Point(){
        delete m_pObject;
    }
};
SmartPoint smartPoint(new Object);
doSomethine();
```


第八条 覆盖equals遵守通用约定
why : == 判断是否是同一个对象，如果要实现逻辑相等，则需要实重载equals方法。

1. 比较典型的用处是和数字相关的判断，比如写了一个复数类，这个时候重载equals方法就很有必要。

2. x.equals(null) = false.

```java
    "xxx".equals(str);// 有效防止NPE
```
 
  
第九条 覆盖equales时同时需要覆盖 hasCode


第十条 始终覆盖 toString
why : 更好的可视化,debug会很方便
