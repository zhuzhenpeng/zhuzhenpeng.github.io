---
layout: post
title:  GTest 5 -- Test Fixtures初探
category: Google Test
---

> 有时我们的多个测试都是对相同的数据进行的，此时我们可以使用测试夹具

##概念
当创建一个测试夹具(test fixture)时，我们需要:  

1. 创建一个测试夹具类，公有继承gtest的testing::Test类，这个测试夹具类的主体必须放在public或protected限定符中  
2. 在类中声明任何需要使用的对象  
------------------------以下仅当需要时--------------------  
3. 在 **SetUp( )** 函数中设置对象测试前的状态  
4. 在 **TearDown( )** 函数中释放任何在测试途中打开的资源  
5. 自定义子程序(subroutine)  

##使用

1. 使用夹具时，应该使用 **TEST_F()** 宏而不是TEST()宏
2. test_case_name必须是刚才定义的夹具类的名字，否则错误

{% highlight c++ %}
TEST_F(test_case_name, test_name) {
 ... test body ...
}
{% endhighlight %}



##夹具与测试的交互过程
对于定义在TEST_F()中的每个测试,gtest都会做以下的事情  

1. 创建一个新的夹具对象
2. 通过SetUp()函数初始化
3. 执行TEST_F()中的测试
4. 通过TearDown()来回收资源
5. 删除此夹具对象

##例子
假设要测试一个FIFO的queue类


{% highlight c++ %}
template <typename E> // E is the element type.
class Queue {
 public:
  Queue();
  void Enqueue(const E& element);
  E* Dequeue(); // Returns NULL if the queue is empty.
  size_t size() const;
  ...
};
{% endhighlight %}


定义一个夹具类


{% highlight c++ %}
class QueueTest : public testing::Test {
 protected:
  virtual void SetUp() {
    q1_.Enqueue(1);
    q2_.Enqueue(2);
    q2_.Enqueue(3);
  }

  // virtual void TearDown() {}

  Queue<int> q0_;
  Queue<int> q1_;
  Queue<int> q2_;
};
{% endhighlight %}

编写TEST_F()测试函数，第一个参数是上面定义的夹具类名

{% highlight c++ %}
TEST_F(QueueTest, IsEmptyInitially) {
  EXPECT_EQ(0, q0_.size());
}

TEST_F(QueueTest, DequeueWorks) {
  int* n = q0_.Dequeue();
  EXPECT_EQ(NULL, n);

  n = q1_.Dequeue();
  ASSERT_TRUE(n != NULL);
  EXPECT_EQ(1, *n);
  EXPECT_EQ(0, q1_.size());
  delete n;

  n = q2_.Dequeue();
  ASSERT_TRUE(n != NULL);
  EXPECT_EQ(2, *n);
  EXPECT_EQ(1, q2_.size());
  delete n;
}
{% endhighlight %}
