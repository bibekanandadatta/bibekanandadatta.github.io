---
layout: post
title: Relearning Mathematics
date: 2021-01-04 11:59:00-0400
description: A collection of learning resources for mathematics
tags: books courses lecture mathematics
categories: resources
giscus_comments: false
related_posts: true
# toc:
#   sidebar: left
---


## A little background

I started to grow interest in computational mechanics when I was a sophomore in my Mechanics of Solids and Numerical Analysis class. For the next two years in my undergrad, I worked on learning and developing computational methods for linear elasticity problems. I developed a displacement potential-based finite difference method for modeling the interfaces of elastic composites. It was more of a learning experience than developing something novel. During my masters, my research focused more on materials science and less on computational mechanics. Soon I realized I like computational mechanics and developing algorithms and decided if I ever do PhD, I will work in that field. Luckily, I recently started my PhD and my research focuses on computational mechanics. However, within a few days, I started to feel I have lack of knowledge in many essential topics, especially mathematics.

During my undergrad, I took 4 mathematics classes in my freshman and sophomore years which covered from Calculus to Partial Differential Equations, and even Complex Analysis. The organization of the topics was not very standard as I have seen in the syllabus of different US schools and also the old school way of teaching didn't help in understanding the content. To make it worse, I can't recall applying those methods/ techniques in any engineering class to a great extent. So I forgot most of it by the time I started my PhD and have to look up textbooks now and then. I took one recommended math class comprising Linear Algebra, Ordinary, and Partial Differential Equations. The class was fast-paced and poorly organized once again so that didn't really clear the confusion I already had. Other graduate mathematics classes offered by my school were more theory and analysis based while I needed applied classes. So, for once and all I decided that I would brush up all the topics in an organized manner by myself as there are a lot of great books and learning resources available online. I also decided to document all these resources so that I can always go back and look up references. If it helps other people, I would feel humbled.

## What did I need to learn?

The first step to solving any problem is to identify the problem properly. In this case, I was lucky that I wasn't a stranger to the topics that I needed to learn. I was comfortable with Calculus-I and Calculus-II (single and multi-variable, respectively) but still needed to brush up on selected topics. So I picked up James Stewart's Calculus book and read some sections on multivariable calculus and took notes on them. But the main challenge was to learn Linear Algebra and ordinary and Partial Differential Equations in a way that I don't have to look up textbooks to solve standard problems. I also wanted to master Laplace Transform, Fourier Series, and Fourier Transform, in the context of differential equations. Complex Analysis could be another addition to the list, but I kept this aside for the time being. However, I started looking up resources that cover those aforementioned topics in great detail and from an engineering or physical science perspective.

### Resources

A general resource for learning these contents could be Advanced Engineering Mathematics by Erwin Kreyszig (any edition). Old editions are available on Amazon at a cheaper price and I would recommend having one in the book-shelf.

---

## Linear Algebra

Most graduate-level Linear Algebra classes focus on the proof-based analysis of matrix theory. So an undergraduate-level Linear Algebra seems to be sufficient for the time being to understand the core concepts. So I picked up the following resources.

### Essence of Linear Algebra by 3Blue1Brown

A small video lecture series by one of my favorite math YouTubers. His presentation isn't rigorous enough to help you understand the advanced mathematics involved in graduate coursework or research. However, his presentation will develop physical intuition of the subject before diving into advanced topics. I would suggest going over his video series before starting continuum mechanics or even differential equations.
  - [YouTube playlist](https://youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab).

### MIT 18.06 Linear Algebra by Prof. Gilbert Strang:
This is probably the most popular lecture series on this subject. Prof. Strang covered Linear Algebra from scratch to some advanced topics with proper examples and great insights. Homework for this class is assigned from Prof. Strang's book, Introduction to Linear Algebra (4th edition). Solving the homework and exam questions would help understand the subject matter even better.
  - [Course homepage](https://ocw.mit.edu/courses/mathematics/18-06-linear-algebra-spring-2010/).
  - [YouTube playlist](https://youtube.com/playlist?list=PLE7DDD91010BC51F8).

---

## Ordinary Differential Equations

### Learn Differential Equations by Prof. Gilbert Strang (MIT)

This lecture series consists of short lectures from Prof. Strang and Dr. Cleve Moler, founder of MATLAB. Prof. Strang covers analytical solution methods for ODEs while Dr. Moler teaches MATLAB implementation (numerical techniques) to solve ODEs. This class doesn't have any homework or exams; it's rather a self-learning class.
  - [Course homepage](https://ocw.mit.edu/resources/res-18-009-learn-differential-equations-up-close-with-gilbert-strang-and-cleve-moler-fall-2015/index.htm).
  - [YouTube playlist](https://youtube.com/playlist?list=PLUl4u3cNGP63oTpyxCMLKt_JmB0WtSZfG).

### Engineering Mathematics-I by Prof. Steve Brunton (UW)

Prof. Brunton's teaching philosophy revolves around both learning the methods and being able to implement them in available programs. My personal learning and teaching philosophy is coherent with his which made his class lectures enjoyable. Going over the lectures in parallel with Prof. Strang and Dr. Moler's class will make it beneficial to the greatest amount. Old homework and exams are also available on his website. 
  - [Course homepage](http://faculty.washington.edu/sbrunton/me564/).
  - [YouTube playlist](https://youtube.com/playlist?list=PLMrJAkhIeNNR2W2sPWsYxfrxcASrUt_9j).

### Books 

The first book is a widely used textbook for Ordinary Differential Equation and has a modern presentation and explanations. The second one is a classic book on Ordinary Differential Equation and it's cheap.
  - Boyce, William, DiPrima, Richard. Elementary Differential Equations (10th edition). Wiley, 2012.
  - Tenenbaum, Morris, Pollard, Harry. Ordinary Differential Equations. Dover Publications, 1985.

---

## Partial Differential Equations

### Fourier Analysis by Prof. Steven Brunton (UW)

This is my one of absolute favorite lecture series on Fourier Analysis. Starting with Fourier Analysis, Prof. Brunton dives into Fourier Transform and interesting engineering applications using both MATLAB and Python. This class has a similar pattern to Prof. Strang and Dr. Moler's Learn Differential Equations; doesn't have any homework and/ or exam. To appreciate the content of this class, a decent understanding of Linear Algebra and Ordinary Differential Equations is needed.

  - [YouTube playlist](https://youtube.com/playlist?list=PLMrJAkhIeNNT_Xh3Oy0Y4LTj0Oxo8GqsC).

### Engineering Mathematics-II by Prof. Steve Brunton (UW) 

This is a continuation of Engineering Mathematics-I where Prof. Brunton covers PDE. As usual, his lectures are enjoyable and application-based. 
  - [Course homepage](http://faculty.washington.edu/sbrunton/me565/).
  - [YouTube playlist](https://youtube.com/playlist?list=PLMrJAkhIeNNR2W2sPWsYxfrxcASrUt_9j).

### Books 

My first book on PDE was Nakhle Asmar's. So I am a bit biased towards that book. However, Richard Haberman's book is widely used for most PDE classes and has similar content to Asmar. I also enjoyed reading Stanley Farlow's book.
  - Farlow, Stanley. Partial Differential Equations for Scientists and Engineers. Dover Publications, 1993.
  - Haberman, Richard. Applied Partial Differential Equations with Fourier Series and Boundary Value Problems (4th edition). Prentice Hall, 2003.
  - Asmar, Nakhle. Partial Differential Equation with Fourier Series and Boundary Value Problems (2nd edition). Prentice Hall, 2005.
