---
layout: post
title: "HowTo: create and remove Cookies with ASP.NET MVC"
date: 2010-10-19 20:07
author: CI Team
comments: true
categories: [HowTo]
tags: []
language: en
---
{% include JB/setup %}
<img src="{{BASE_PATH}}/assets/wp-images-de/image-thumb737.png" border="0" alt="image" width="135" height="125" align="left" />Cookies are a great way to save files on a client like for example registration files ore other stuff. I´m sure some of you who are using the ASP.NET membership system are already familiar with the "stay declared" function. This "HowTo" teach you an easy way how to create and delete a cookie.




<strong>Structure</strong>



As demo base I use the ASP.NET MVC Framework. That´s what the test page looks like if a cookie is opened:

<img src="{{BASE_PATH}}/assets/wp-images-de/image-thumb738.png" border="0" alt="image" width="268" height="111" />

I also registered the time when the cookie was created. (Yes. I´ve been blogging at 3 a.m. in the morning. My poor girlfriend ;) )

If there is no cookie found than it looks like this:

<img src="{{BASE_PATH}}/assets/wp-images-de/image-thumb739.png" border="0" alt="image" width="244" height="96" />

The output of the index.aspx runs over the Home Controller:

<pre class="c#">
public ActionResult Index()
        {
            ViewData["Message"] = "Welcome to ASP.NET MVC!";
            string cookie = "There is no cookie!";
            if(this.ControllerContext.HttpContext.Request.Cookies.AllKeys.Contains("Cookie"))
            {
                cookie = "Yeah - Cookie: " + this.ControllerContext.HttpContext.Request.Cookies["Cookie"].Value;
            }
            ViewData["Cookie"] = cookie;
            return View();
        }
</pre>

Here it is detected if a cookie exists and if yes than it will be out given.

These two Links guide you to the <strong>CookieController:</strong>

<pre class="c#">
public class CookieController : Controller
    {

        public ActionResult Create()
        {
            HttpCookie cookie = new HttpCookie("Cookie");
            cookie.Value = "Hello Cookie! CreatedOn: " + DateTime.Now.ToShortTimeString();

            this.ControllerContext.HttpContext.Response.Cookies.Add(cookie);
            return RedirectToAction("Index", "Home");
        }

        public ActionResult Remove()
        {
            if (this.ControllerContext.HttpContext.Request.Cookies.AllKeys.Contains("Cookie"))
            {
                HttpCookie cookie = this.ControllerContext.HttpContext.Request.Cookies["Cookie"];
                cookie.Expires = DateTime.Now.AddDays(-1);
                this.ControllerContext.HttpContext.Response.Cookies.Add(cookie);
            }
            return RedirectToAction("Index", "Home");
        }

    }
</pre>

With the create method it´s quite simple to create a Cookie and lay it down into the response and afterwards it turns back to the Index View.

The remove method controls if a cookie exists and if the answer is positive the Cookie will be deleted directly.

<strong>Beware while deleting cookies:</strong>



This way to delete a cookie doesn´t work:

<pre class="c#">
this.ControllerContext.HttpContext.Response.Cookies.Clear();
</pre>

The cookie has to go back to the remove (like it is given in the Cookie Controller) and an <a href="http://msdn.microsoft.com/en-us/library/system.web.httpcookie.expires.aspx">expiry date</a> should be given. I´m going to set it on yesterday so the browser has to refuse it directly.

I had to write this down after I was cursing about this subject for a long time. ;)
