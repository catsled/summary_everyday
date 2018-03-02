使用hashlib对数据进行去重
------------------------

在对数据进行去重时，有多种方法，比如说，使用bloom filter, 集合去重， 关键字去重。

但是，当我们遇到一个字典数据，且该字典中并没有唯一字段时，由于字典的无序性，我们应该如何进行去重。

解决方法: 我们可以将字典中所有的值取出并放入一个列表中，然后对列表中的内容做MD5求和运算。

验证: 1+2+3 = 3+1+2 = 2+3+1 = 2+1+3(加法交换律)
      
      md5算法对相同的输入会产生相同的结果: x=y --> md5(x) = md5(y)
      
      
代码 ::
  
  import json
  import hashlib
  
  from bloom_filter import BloomFilter
  
  
  def hash_filter(*file_path):
      bloom = BloomFilter(max_elements=1000000, err_rate=0.001)
      data_list = list()
      
      for path in file_path[:-1]:
          with open(path, 'r') as fout:
              for line in fout.readlines():
                  md_list = list()
                  json_data = line.strip()
                  dict_data = json.loads(json_data)
                  
                  for value in dict_data:
                      md_list.append(value)
                      
                  hash_v = sum(map(lambda x: int(haslib.md5(str(x).encode('utf-8')).hexdigest(), 16), md_list))
                  
                  try:
                      assert hash_v not in bloom
                      bloom.add(hash_v)
                      data_list.append(json_data)
                  except:
                      pass
                      
      with open(file_path[-1], 'w') as fin:
          for data in all_list:
              fin.write(data + '\n')
                      
                      
                      
      
      
      
      
      
      
      
      
