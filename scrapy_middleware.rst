Spider Middleware测试
-----------------------

1. process_spider_input

   `Called for each response that goes through the spider middleware and into the spider`
   
   就是说处理每一个要经过该 ``spider middleware`` 并进入 ``spider`` 的响应。
   
   
2. process_spider_output
   
   `Called with the results returned from the Spider, after it has processed the response`
   当从Spider中返回一个已处理过的响应后调用该方法。
   
3. process_start_requests

   Called with the start requests of the spider, and works similarly to the ``process_spider_output()`` method,
   except that it doesn't have a response associated
    
   与 ``process_spider_output`` 类似，只是它并不关联响应，且与Spider中的 ``start_requests`` 相关
   
4. process_spider_exception

   Called when a spider or ``process_spider_input()`` method (from other spider middleware) raise an exception
   
   当一个 ``spider`` 或从 ``process_spider_input()`` 方法中抛出一个异常时调用该方法
   
5. from_crawler

   This method is used by Scrapy to create your spiders.
   
   该方法用来创建类，同时添加一些类属性。
