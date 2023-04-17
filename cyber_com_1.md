### cyber通信基础

### 1、Node

```cpp
class Node {
  std::string node_name_;
  std::string name_space_;

  std::mutex readers_mutex_;
  std::map<std::string, std::shared_ptr<ReaderBase>> readers_;

  std::unique_ptr<NodeChannelImpl> node_channel_impl_ = nullptr;
  std::unique_ptr<NodeServiceImpl> node_service_impl_ = nullptr;
};
```



```cpp
class NodeChannelImpl
{

  bool is_reality_mode_;
  std::string node_name_;
  proto::RoleAttributes node_attr_;
  NodeManagerPtr node_manager_ = nullptr;			//用于服务发现!
};
```

```cpp
class NodeServiceImpl
{
  std::vector<std::weak_ptr<ServiceBase>> service_list_;
  std::vector<std::weak_ptr<ClientBase>> client_list_;
  std::string node_name_;
  proto::RoleAttributes attr_;
};
```

一个Reader/Writer与固定的信息类型绑定。

```c++
template <typename MessageT>
class Reader: public ReaderBase
{

    
      CallbackFunc<MessageT> reader_func_;
      ReceiverPtr receiver_ = nullptr;
      std::string croutine_name_;
      BlockerPtr blocker_ = nullptr;
      ChangeConnection change_conn_;
      service_discovery::ChannelManagerPtr channel_manager_ = nullptr;
};
```

```c++
template<typename MessageT>
class Writer: public WriterBase
{
    
    
      using TransmitterPtr = std::shared_ptr<transport::Transmitter<MessageT>>;
    
    
      //实际传输实现
      TransmitterPtr transmitter_;
      ChangeConnection change_conn_;
      service_discovery::ChannelManagerPtr channel_manager_;
};
//核心函数
template <typename MessageT>
bool Writer<MessageT>::Write(const std::shared_ptr<MessageT>& msg_ptr) {
  RETURN_VAL_IF(!WriterBase::IsInit(), false);
  return transmitter_->Transmit(msg_ptr);
}
```

