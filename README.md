# file_content_saerch
批量文件内容检索工具，目前支持检索的文件类型：解析表格类型：xlsx、csv、xls、xlsm、xlam、xlsb。解析文本类型：txt、json、sql。


使用方法：

USAGE:

    file_content_saerch [OPTIONS]


OPTIONS:

        --check_key <check_key>...                    检索key（多个值使用,逗号分割）
        
        --check_key_logic <check_key_logic>...        多个检索key查询关系（AND 或 OR）
        
        --check_value <check_value>...                检索值（多个值使用,逗号分割）
        
        --check_value_logic <check_value_logic>...    多个检索值查询关系（AND 或 OR）
        
    -h, --help                                        Print help information
    
    -l, --limit <10>...                               查询条数
    
    -o, --output <>...                                结果导出（如不需要导出就不需要设置）
    
        --offset <0>...                               查询偏移量
        
    -p, --path <path>...                              目录或文件路径
    
    -V, --version                                     Print version information
    




#执行

  file_content_saerch -p 文件夹必须设置 -l 10000（不建议设置的特别大） --check_key 字段名称（一般可不设置） --check_key_logic 多个字段间逻辑关系  --check_value 检索的值（必须设置） --check_value_logic 多个检索值逻辑关系





#文件名定义说明

如文件中包含头部字段参数。

  例如xlsx文件：
  
    第一行：字段名称1，字段名称2，字段名称3
    
    第二行：数据1，数据2，数据3
    
    第三行：数据11，数据22，数据33
    

  当该文件名中带有header字符串时，表示个文件内容包含了字段定义；如文件名中没有header字符串则字段定义行将按文件内容处理。




#执行说明

  path参数表示需要指定包含所有需要被检索的文件的根目录。（工具会去分析该目录内所有文件，进行解析检索内容）
  
  check_key参数表示对定义字段（也就是带有header字符串的文件名中，文件内容的第一行），进行检索。需要检索多个参数时使用逗号进行分割。
  
  check_key_logic参数表示多个check_key参数时，各个参数之前的逻辑关系。
  
  check_value参数表示检索的内容。需要检索多个参数时使用逗号进行分割。
  
  check_value_logic参数表示多个check_value参数时，各个参数之前的逻辑关系。
  
  limit参数表示本次要获取的数量。该值不建议设置的特别大，因为会检索所有文件内容，同时缓存到内存中。（正常情况：例如limit为10000时，当程序获取到10000条数据后将会终止其他并发任务）
  
  offset参数查询结果偏移量。由于时并发执行读取文件内容，该值的设定不会很准确。
  
  output参数表示结果保存文件的名称。如果这顶了该参数那么结果将保存到指定文件中。
  
  在执行才有check_key参数时，将只会对文件名中带有header的文件进行检索





#注意：

  该工具在执行时将会火力全开（并行执行所有文件检索），充分利用cpu性能，因此cpu的使用率会特别的高。
  
  场景一：有很多的文件（例如几千个），当时每个文件的内容大概在几百兆。这种情况，cpu使用率很高，内存估计将会占用2-3GB左右。
  
  场景二：文件数量少，每个文件很大（例如有几个G）。这种情况，总执行时间会相对较长一些。




#实战：

  本地有一个文件夹，总大小为41G，文件数量在500+个。执行时，cpu使用率在90%以上，内存在2G以内，检索所有内容大概执行时间在130秒左右。（win,硬件i9 9700, m2硬盘）




#备注

  如果希望数据内容被格式化且具有关联性。可查看https://github.com/ddv404/mswd （经过实战mswd工具在原文件有几十G，处理到db后会有上百G的文件大小，且在进行模糊查询的时候性能很低。因而诞生了当前项目，该项目。）
  
