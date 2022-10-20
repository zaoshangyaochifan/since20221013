---
title: "延迟队列"
weight: 1
# bookFlatSection: false
bookToc: true
# bookHidden: false
# bookCollapseSection: false
bookComments: true
# bookSearchExclude: false
---

# 如kafka实现延迟队列

## 1. 使用延迟topic
- 在发送延迟消息时不直接发送到目标topic，而是发送到一个用于处理延迟消息的topic
- 写一段代码拉取延迟topic中的消息，将满足条件的消息发送到真正的目标主题
- 在轮询kafka拉取消息的时候，它会返回由max.poll.records配置指定的一批消息，但是当程序代码不能在max.poll.interval.ms配置的期望时间内处理这些消息的话，kafka就会认为这个消费者已经挂了，会进行rebalance
- 可以使用暂停和恢复的API函数，调用消费者的暂停方法后就无法再拉取到新的消息，同时长时间不消费kafka也不会认为这个消费者已经挂掉了
  
参考自：[https://zhuanlan.zhihu.com/p/365802989](https://zhuanlan.zhihu.com/p/365802989)