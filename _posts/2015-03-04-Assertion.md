---
layout: post
title: GTest 3 -- Assertion
category: Google Test
---

> 我们是通过Assertion来断言一个类或函数的行为的

##错误信息
* 当发生错误时，gtest会显示错误信息，信息中自带错误的来源文件，代码第几行，以及失败信息
* 我们通过在Assertoins后面使用 << 符号即可自定义错误信息

{% highlight c++ %}
for (int i = 0; i < x.size(); ++i) {
  EXPECT_EQ(x[i], y[i]) << "Vectors x and y differ at index " << i;
}
{% endhighlight %}

##条件测试


Fatal assertion | Nonfatal assertion | Verifies
--------------- | ------------------ | --------
ASSERT_TRUE(condition); | EXPECT_TRUE(condition); | condition is true
ASSERT_FALSE(condition); | EXPECT_FALSE(condition); | condition is false


##二元比较

1. 比较的两个对象必须支持相应的 **操作符** 运算
2. 两个参数的执行顺序是不确定的，程序不应该依赖于特定顺序
3. C++的string类型应该使用这类宏进行比较


Fatal assertion | Nonfatal assertion | Verifies
--------------- | ------------------ | --------
ASSERT_EQ(expected, actual);|EXPECT_EQ(expected, actual)|expected == actual
ASSERT_NE(val1, val2);|EXPECT_NE(val1, val2);|val1 != val2
ASSERT_LT(val1, val2);|EXPECT_LT(val1, val2);|val1 < val2
ASSERT_LE(val1, val2);|EXPECT_LE(val1, val2);|val1 <= val2
ASSERT_GT(val1, val2);|EXPECT_GT(val1, val2);|val1 > val2
ASSERT_GE(val1, val2);|EXPECT_GE(val1, val2);|val1 >= val2


##C string 比较

Fatal assertion | Nonfatal assertion | Verifies
--------------- | ------------------ | --------
ASSERT_STREQ(expected_str, actual_str);|EXPECT_STREQ(expected_str, actual_str);|the two C strings have the same content
ASSERT_STRNE(str1, str2);|EXPECT_STRNE(str1, str2);|the two C strings have different content
ASSERT_STRCASEEQ(expected_str, actual_str);|EXPECT_STRCASEEQ(expected_str, actual_str);|the two C strings have the same content, ignoring case
ASSERT_STRCASENE(str1, str2);|EXPECT_STRCASENE(str1, str2);|the two C strings have different content, ignoring case

