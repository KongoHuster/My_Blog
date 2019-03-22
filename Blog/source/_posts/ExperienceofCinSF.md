---
title: Experience of C++ in SF
date: 2018-10-06 16:07:40
categories:
- C++
tags:
- 深信服
---

[TOC]
## 1.Attachment moudule
### (1) perfect using in Attachment 
#### a. Checkout of parameter
```c++
/**
 * set attachment id
 * @param  attachmentId is a string
 */
void Attachment::setAttachmentId(const std::string &attachmentId) {
	//Checkout of parameter
	if (attachmentId.empty())
	{
		SE_LOG_WARN(kAttachmentModule,"set attachmentId get an empty string");
		return;
	}
	m_attachmentId = attachmentId;
	SE_LOG_DEBUG(kAttachmentModule,"setattachmentId success");
}
```
<!-- more -->
#### b.Using get to judge shared_ptr is null  
```c++
shared_ptr<Attachment> attachment;
if(attachment.get() == NULL) {
	return ;
}
```
c.Using empty to judge string is empty 
```
std::string Id; 
// check Id is empty
if (Id.empty()) {
	SE_LOG_ERROR(kAttachmentModule,"getAttachment get an empty Id");
	return finalget;
}
```

### (2) Knowledge in Attachment
#### a.uint64_t
```
uint8_t
uint16_t
uint32_t
uint64_t
 
unsigned integer type with width of exactly 8, 16, 32 and 64 bits respectively 
(provided only if the implementation directly supports the type) 
```

#### b.using of rapidJson
```c++
//define 
StringBuffer strbuf; 
Writer<StringBuffer> writer(strbuf); 

//How to write json
writer.StartObject(); 
writer.Key(F_JSON_ATT_ID);
writer.String(m_id.c_str());

//Write Bool
writer.Key(F_JSON_ATT_IS_Forward); 
writer.Bool(True);

//Write Int
writer.Key(F_JSON_ATT_DOWNLOADDATE); 
writer.Uint64(m_downloadDate); 


//Ending of writing
writer.EndObject(); 

//Return std::string 
return return strbuf.GetString(); 
```

#### c.std::lock_guard

(1)what is std::lock_guard
```
The class lock_guard is a mutex wrapper that provides a convenient RAII-style 
mechanism for owning a mutex for the duration of a scoped block.    
```
(2)how to using std::lock_guard 
```
#include <thread>
#include <mutex>
#include <iostream>
 
int g_i = 0;
std::mutex g_i_mutex;  // protects g_i
 
void safe_increment()
{
    std::lock_guard<std::mutex> lock(g_i_mutex);
    ++g_i;
 
    std::cout << std::this_thread::get_id() << ": " << g_i << '\n';
 
    // g_i_mutex is automatically released when lock
    // goes out of scope
}
 
int main()
{
    std::cout << "main: " << g_i << '\n';
 
    std::thread t1(safe_increment);
    std::thread t2(safe_increment);
 
    t1.join();
    t2.join();
 
    std::cout << "main: " << g_i << '\n';
}
```
(3)output
```c++
main: 0
140641306900224: 1
140641298507520: 2
main: 2
```
#### d.std::thread 


## 2.DataBase Moudul;
