---
layout: post
title:  "Web Basics (2) - What is a Framework?"
date:   2023-09-08 12:40:48 +0900
categories: webdev
---

![Web developer image](https://img.freepik.com/free-vector/web-development-concept-with-programmer-ar_107791-17049.jpg?w=2000)
##### Image from Freepik

If you ever tried developing your own website, you probably have bumped into the term 'Web Framework.' Over time you grasp a vague understanding that framework is "a basic structure of a website that you can build up on." Common frameworks used nowdays include React, Angular, and Vue JS, and if you haven't started your first web application using the traditional HTML and CSS, it is most likely that you're learning React (since it's the most popular framework on 2023).

## So, what actually are web frameworks?

![Framework image](https://parcusgroup.com/blog/wp-content/uploads/2016/04/framework-820x490.jpg)
##### Image from ParcusGroup

Above is the image of a framework, "an essential supporting structure of a building, vehicle, or object", according to the Oxford dictionary. Likewise, a web framework is a pre-prepared library (set of tools) that provides a foundation (a template) on which software developers can build programs. Essentially, frameworks aid developers by offering a structure and set of standards by which applications can be built, making the process faster and efficient.

> Web framework = Template of a web application = Abstraction

&nbsp;

### If you don't use frameworks,

for example, when you're developing a website with pure HTML, CSS, and JavaScript, (called "vanilla" approach), and possibly backend languages like PHP, Python, Ruby, you are going to manually work on these:

1. **Project structure:** You are going to decide and organize the system for your codebase
2. **Routing:** Backend frameworks handle URL routing for you. Withouth these, you'll have to handle URL parsing and direct requests to the correct scripts.
3. **Database Connections:** You have to set up and manage database connection yourself, including query creation, and ensure that your queries would be safe from SQL attacks.
4. **Form Handling:** You'll create your forms, handle data submission, validate use input, and protect agains attacks.
5. **Sessions and Cookies:** You have to find a way to set, retrieve, and delete user sessions, cookies, and other stateful data manually.
6. **Responsive Design:** Without a CSS framework, you'll handle all responsive design breakpoints and grids. You'll set all the color and size changes whenever a user clicks something, hover on something, or change the screen size.
7. **Extensions/Plugins:** Without a community-driven library of extensions or plugins, all added functionality will need to be built from scratch or integrated manually. You're going to write everything about what happens when a user clicks a button.
8. **Error Handling:** You should implement error and exception handling methods for all the cases that might go wrong.
9. **AJAX and Client-side Interactivity:**  Continuing from the above lists, you should handle all client-server communication, and this often involves raw XML or Fetch API for AJAX requests.
10. **And a lot more!**


> The list above shows what frameworks are doing for you

Of course this approach will offer the maximum flexibility to your project, but it will be exhaustive for you to work on all of these. Your web application is likely to have secureity issues since attackers will try _everything_ to cause problems. In other words, if you fill out the template that the framework provides, the framework will automatically convert it into a functional website.

&nbsp;

> ### If you want to make your own chair,
> would you start from chopping the woods or assemble the IKEA product and customize?
> There are pros and cons for everything
> ![Building a chair](https://contentgrid.homedepot-static.com/hdus/en_US/DTCCOMNEW/Articles/how-to-build-a-DIY-adirondack-chair-step-10.jpg)
##### Image from The Home Depot


&nbsp;

## Key aspects of a framework

Here are some key aspects of frameworks that allows easier web development.

1. **Abstraction**

    Frameworks handle many repetitive and common tasks, providing generic functionallity to the website. From the abstract framework, develoers only have to focus on the unique features.

2. **Pre-built Components**

    Frameworks come with a set of pre-built compoents for database access, templating, session management, and more.

3. **MVC Architecture**

    Many modern web frameworks, like Django, Spring, or Ruby on Rails, use the Model-View-Controller (MVC) architecture. This divides the application into three interconnected components, making it more modular and easier to manage the system. This means if you have to update your website, you don't have to fix the whole code. You only have to focus on what you'd like to change

4. **Convention Over Configuration**

    A principle followed by many frameworks, meaning that the environment assumes reasonable defaults in the absence of developer intervention. It reduces the decisions that developers have to make unless they wish to depart from the convention.

5. **Security**

    Frameworks usually come with built-in protection against common web vulnerabilities like SQL injection, Cross-Site Scripting (XSS), and Cross-Site Request Forgery (CSRF).


Aside from these, as mentioned multiple times, frameworks provide better development efficiency, scalability, and comes with community support if you're using popular frameworks.

&nbsp;
&nbsp;

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### Reference & More to read
- [Top 10 Web Development Frameworks, Interviewbit.com](#https://www.interviewbit.com/blog/web-development-frameworks/#:~:text=Ans%3A%20Among%20the%20most%20commonly,for%20back%2Dend%20web%20development.)
- [Framework Abstractions](#https://www.crossingtheruby.com/2021/02/08/framework-abstractions.html)