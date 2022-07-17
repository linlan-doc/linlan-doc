---
title: go简介
sidebar_position:  1

keywords: ['go编程','go开发环境搭建','go语言学习','go学习资料']
---

import { VerticalTimeline, VerticalTimelineElement }  from 'react-vertical-timeline-component';
import 'react-vertical-timeline-component/style.min.css';

 _go_ 是 _Google_ 公司开发的一款编程语言，它最早出现于2009年。得益于 _k8s_，_go_ 越来越流行，中国大陆越来越多的公司开始使用 _go_ 进行开发。本文档适用于有一定编程基础的读者。对于没有编程基础的读者，建议先学习 _c/c++_ 或者 _Java_，这样才能更好的理解文档内容。

 互联网上 _go_ 语言的学习资料非常丰富，现在列举一些本文档使用到的，共读者参考。

1.  [A Tour of Go](https://go.dev/tour/welcome/1)
2.  [The Go Programming Language Specification](https://go.dev/ref/spec)
3.  [Effective Go](https://go.dev/doc/effective_go#defer)
4.  [geeksforgeeks](https://www.geeksforgeeks.org/)

 经过十多年的发展，_go_ 的生态越来越完善，下面列举 _go_ 生态里面常见的中间件，有兴趣的读者可以前往这些中间件的主页，获取更多详情。

1.  web服务器：[Gin](https://github.com/gin-gonic/gin)
2.  微服务框架： [go-micro](https://github.com/asim/go-micro)、[go-dubbo](https://github.com/apache/dubbo-go)
3.  ORM: [GORM](https://github.com/go-gorm/gorm)、[beego](https://github.com/beego/beego)
4.  redis: [go-redis](https://github.com/go-redis/redis)

 _go_ 语言时间线

<VerticalTimeline lineColor="grey" animate={false}>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2022-03-15 Go1.18</h3>
    <p>
      泛型
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2021-08-16 Go1.17</h3>
    <p>
      slice转数组指针等
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2021-02-16 Go1.16</h3>
    <p>
      适配不同系统和内核，无重大更新
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2020-08-11 Go1.15</h3>
    <p>
       连接器、小对象分配优化等
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2020-02-25 Go1.14</h3>
    <p>
       overlapping interfaces
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2019-09-03 Go1.13</h3>
    <p>
      支持更多类型的数值常量
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2019-02-25 Go1.12</h3>
    <p>
      适配不同系统和内核，无重大更新
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2018-08-24 Go1.11</h3>
    <p>
      适配不同系统和内核，无重大更新
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2018-02-16 Go1.10</h3>
    <p>
      工具链、运行时优化等
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2017-08-24 Go1.9</h3>
    <p>
      类型别名
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2017-02-16 Go1.8</h3>
    <p>
      结构体转换
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2016-08-15 Go1.7</h3>
    <p>
      terminating statements
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2016-02-17 Go1.6</h3>
    <p>
      运行时、基础库优化
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2015-08-19 Go1.5</h3>
    <p>
      编译器自举、并行垃圾回收等
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2014-12-10 Go1.4</h3>
    <p>
      for range、和 **T方法调用
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2014-06-18 Go1.3</h3>
    <p>
      针对不同操作系统和架构的优化
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2013-12-01 Go1.2</h3>
    <p>
      空指针、Three-index slices等
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2013-05-13 Go1.1</h3>
    <p>
      Unicode优化、方法值等
    </p>
  </VerticalTimelineElement>
  <VerticalTimelineElement
    className="vertical-timeline-element--work"
    contentStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}
    contentArrowStyle={{ borderRight: '7px solid  rgb(249, 180, 45)' }}
    iconStyle={{ background: 'rgb(249, 180, 45)', color: '#fff' }}>
    <h3 className="vertical-timeline-element-title">2012-03-28 Go1</h3>
    <p>
      Go的第一个版本
    </p>
  </VerticalTimelineElement>
</VerticalTimeline>

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
