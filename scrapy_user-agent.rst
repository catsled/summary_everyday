在scrapy中设置User-Agent
------------------------

在scrapy中除了在settings中添加User-Agent 之外，还可以这样添加user-agent ::

  1. 在Spider中添加，具体做法是，在生成请求时添加 headers={"User-Agent": "xxxx"}
  2. 在DownloaderMiddleware中添加 具体做法是，在process_request中添加 request.headers['user-agent'] = "xxxx"
  
其中DownloaderMiddleware中的配置将会覆盖之前的所有配置。
