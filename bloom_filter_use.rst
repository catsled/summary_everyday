布隆过滤器的使用
--------------

  在执行爬虫任务时，由于忽略了网站的改版问题，导致在数十万的url中有部分url数据抓取失败。
  考虑如何从已抓取到的数据中对应的url字段与全部url进行对比，从而获取到未抓取的url。
  
  由于数据量太大，使用双重for循环将会导致结果处理太慢。
  所以，采用布隆过滤器来完成该任务::
  
      from bloom_filter import BloomFilter
      import json
      import sys
      
      
      def filter_lost_url(url_set_file, data_file, lost_file):
          lost_set = set()
          bloom = BloomFilter(max_elements=1000000, err_rate=0.001)  # 创建bloom过滤器实例, 最大元素100W,允许错误率 0.1%
          
          # 读取已抓取到的url并放入布隆过滤器
          with open(data_file, 'r') as fout:
              for data in fout.readlines():
                  dict_data = json.loads(data.strip())
                  url_field = dict_data.get('url', None')
                  
                  if url_field:
                      url = url_field.strip()
                      try:
                          assert url not in bloom
                          bloom.add(url)
                      except:
                          pass
                          
          with open(url_set_file, 'r') as fout:
              for url in fout.readlines():
                  url = url.strip()
                  try:
                      assert url in bloom
                  except:
                      print('lost :%s' % url)
                      lost_set.add(url)
                    
          with open(lost_filem, 'w') as fin:
              for url in lost_set:
                  fin.write(url + '\n')
     
      
      if __name__ == '__main__':
          if len(sys.argv) != 4:
              print("Usage: url_set_file, data_file, dest_file")
              exit(1)
              
          url_set_file = sys.argv[1]
          data_file = sys.argv[2]
          lost_file = sys.argv[3]
          
          filter_lost_url(url_set_file, data_file, lost_file)
      
      
