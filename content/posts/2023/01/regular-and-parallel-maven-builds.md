---
layout: :theme/post
title: "Regular and Parallel Maven Builds"
description: Basic benchmarking on three machines when running tests 
date: "2023-01-16"
tags: "maven"
---

During the Christmas holidays, I came across an [article](https://blog.frankel.ch/faster-maven-builds/1/) of Nicholas Frankel in which he tested the regular and parallel builds of Maven. I remembered that I had also made a similar [test](https://vasouv.wordpress.com/2016/08/30/building-with-maven/) a few years back.

So I decided to test again, since I have three quite different systems and I'd expect the results to be quite different as well. I also used the Hazelcast Code Samples as the build target, only this time I ran the tests three times. Moreover, the project was built beforehand so it already downloaded all dependencies.

The three systems I used are; my Intel NUC with an Intel Celeron J4005 and 8GB RAM, my Lenovo Yoga 530 laptop with an Intel i5-8250U and 8GB RAM, and my desktop with an Intel i5-12400F and 32GB RAM. The JDK I used was the Adoptium JDK 8u352 and Maven 3.8.6.

|  | mvn test | mvn test -T 1C |
| --- | --- | --- |
| Intel NUC | 4.54 min | 3.59 min |
|  | 4.55 min | 4.06 min |
|  | 4.55 min | 4.10 min |
| Lenovo Yoga | 2.03 min | 1.05 min |
|  | 2.03 min | 1.05 min |
|  | 2.02 min | 1.10 min |
| Desktop | 1.10 min | 20.477 sec |
|  | 1.10 min | 20.388 sec |
|  | 1.10 min | 20.554 sec |
