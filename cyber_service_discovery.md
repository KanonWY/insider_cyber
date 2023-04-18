## cyber中的服务发现实现

在apollo的中间件cyber中服务发现是基于`fastrtps`实现的。[fastDDS链接](https://fast-dds.docs.eprosima.com/en/latest/fastdds/discovery/discovery.html)

基于fastDDS的发现机制，只是在cyber中调用了fastdds的接口进行服务发现。

从生成的二进制链接中可以看到使用了`libfastrtps.so.1`和`libfastcdr.so.1`

![image-20230418180014148](/home/kanon/snap/typora/78/.config/Typora/typora-user-images/image-20230418180014148.png)

- `fastcdr`, a C++ library for data serialization according to the [CDR standard](https://www.omg.org/spec/DDSI-RTPS/2.2) (*Section 10.2.1.2 OMG CDR*).
- `fastrtps`, the core library of *eProsima Fast DDS* library.