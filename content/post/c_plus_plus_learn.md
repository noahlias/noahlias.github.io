---
title: "Learn C++ with ChatGPT"
date: 2023-02-22 23:08:02
draft: false
tags: ["ChatGPT"]
categories: ["C++"]
mathjax: true
---

原链接:
[CourseWork](https://github.com/courseworks/AP1401-1-HW3_4)

[个人Repo](https://github.com/noahlias/AP1401-1-HW3_4)

## Outline

这个`c++`作业 有2个部分:

- 第一部分实现一个消息类和服务器类 以及用户类(用于互相传送信息) 这部分你需要了解 **类** ,**继承**, **多态**
- 第二部份需要处理`STL` 函数，你需要了解 匿名类等

## Implement

### Server

这部分的核心实现以及测试和**DEBUG** 都是在通过**ChatGPT**完成的
另外 这里面学到了一个新的function:**std::copy_if**.

这里面readme里面有一些迷惑的行为, 有的地方你需要用引用, 因为拷贝开销是很大的 比如 `sort_msgs`

```cpp
class Server
{
public:
    Server() = default;
    std::vector<User> get_users();
    std::map<std::string, std::string> get_public_keys();
    std::vector<Message*> get_messages();
    User create_user(std::string username);
    bool create_message(Message* msg, std::string signature);
    std::vector<Message*> get_all_messages_from(std::string username);
    std::vector<Message*> get_all_messages_to(std::string username);
    std::vector<Message*> get_chat(std::string user1, std::string user2);
    static void sort_msgs(std::vector<Message*>& msgs);
private:
    std::vector<User> users {};                        // to store our users
    std::map<std::string, std::string> public_keys {}; // map usernames to their publickeys
    std::vector<Message*> messages {};                 // to store all the messages sent by users
};
```

### Message

这部分的实现 一部分我是参考**ChatGPT**给出的实现来做的
比如初始化`时间`和`随机无符号数`

```cpp
std::string Message::current_time() {
  auto now = std::chrono::system_clock::now();
  std::time_t now_c = std::chrono::system_clock::to_time_t(now);
  std::stringstream ss;
  ss << std::put_time(std::localtime(&now_c), "%a %b %d %T %Y");
  return ss.str();
}

VoiceMessage::VoiceMessage(std::string sender, std::string receiver)
    : Message("voice", sender, receiver) {
  voice.resize(5);
  std::random_device rd;
  std::mt19937 gen(rd());
  std::uniform_int_distribution<unsigned int> dis(0, 255);
  for (int i = 0; i < 5; i++) {
    voice[i] = static_cast<unsigned char>(dis(gen));
  }
}

```

这段是消息类的定义等

```cpp
#ifndef MESSAGE_H
#define MESSAGE_H

#include <chrono>
#include <ctime>
#include <iomanip>
#include <iostream>
#include <random>
#include <sstream>
#include <string>
#include <vector>
class Message {
 public:
  Message(std::string type = "", std::string sender = "",
          std::string receiver = "")
      : type(type), sender(sender), receiver(receiver), time(current_time()){};
  std::string current_time();
  std::string get_type() const;
  std::string get_sender() const;
  std::string get_receiver() const;
  std::string get_time() const;
  virtual void print(std::ostream& os) const;
  friend std::ostream& operator<<(std::ostream& os, const Message& msg);
  friend bool operator<(Message& lhs, Message& rhs);
 private:
  std::string type;      // type of the message ("text", "voice", ...)
  std::string sender;    // the username who send this message
  std::string receiver;  // the username whom this message is intended for
  std::string time;      // creation time of the message
};

class TextMessage : public Message {
 public:
  std::string get_text() const;
  TextMessage(std::string text, std::string sender, std::string receiver)
      : Message("text", sender, receiver), text(text){};
  void print(std::ostream& os) const;
  friend std::ostream& operator<<(std::ostream& os, const TextMessage& msg);
 private:
  std::string text;
};

class VoiceMessage : public Message {
 public:
  VoiceMessage(std::string sender, std::string receiver);
  std::vector<unsigned char> get_voice() const;
  void print(std::ostream& os) const;
  friend std::ostream& operator<<(std::ostream& os, const VoiceMessage& msg);
 private:
  std::vector<unsigned char> voice;
};

#endif  // MESSAGE_H
```

### User

这段里面关于发送和认证部分的实现也来自AI给出的建议

```cpp
#ifndef USER_H
#define USER_H

#include <string>

class Server;

class User {
 public:
  User(std::string username, std::string private_key, Server* server)
      : username(username), private_key(private_key), server(server){};
  std::string get_username();
  bool send_text_message(std::string text, std::string receiver);
  bool send_voice_message(std::string receiver);

 private:
  std::string username;     // username of the user
  std::string private_key;  // private key of the user
  Server* const server;     // a pointer to the server for communications
};

#endif  // USER_H
```

## Summary

这个项目用于新手学习**C++**的继承和多态等 包括 运算符重载, 虚函数, 匿名函数,`friend`等 都还不错, 其次学习**STL Function** 也很有帮助

- [`std::count_if`](https://cplusplus.com/reference/algorithm/count_if/)
- [`std::unique`](https://cplusplus.com/reference/algorithm/unique/)
- [`std::accumalate`](https://cplusplus.com/reference/numeric/accumulate/)
- [`std::find_if`](https://cplusplus.com/reference/algorithm/find_if/)
- [`std::count`](https://cplusplus.com/reference/algorithm/count/)

STL的一些用法让我想到了`Haskell`,也让我想起了`Python`的iterator和generator等 也联想到了`Rust`,Functional Language 确实在现代高级语言中很普遍等

Reference:
 [From C to C++ to Rust to Haskell](https://www.youtube.com/watch?v=V9uO3x5l-Dk)
