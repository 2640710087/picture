[link]: file://O:/Users/QI/Desktop/apache.gif



# apache rewrite

> rewrite 是url路径重写,好用,可以让url看起来更简洁,是apache里面的一个模块  



### 1.加载rewrie模块 

![apache][link]



#### 在httpd.conf文件中配置

> 这是第一种方法, 在httpd.conf中配置

``` 
    #fileName: httpd.conf

    #web 目录权限

    <Directory />      #这里应该是默认配置, 有错请指出        
      Options FollowSymLinks
      AllowOverride None
    </Directory>

    DocumentRoot "O:/Users/Qi/Documents/Project/Website" 
    <Directory "O:/Users/Qi/Documents/Project/Website">
        Options FollowSymLinks         #可配置选项看下个标题
        AllowOverride None             #不使用.htaccess文件
        RewriteEngine on
        RewriteRule ^/(.*)$ $1.php     # 这个是文件重写后缀,比如,/index,会重写为index.php
    </Directory>

    <Files ".ht*">
      Order Allow,Deny
      Deny from alls
    </Files>
    
```



### AllowOverride 指令选项
optsion | description
:-------| :----------
Options | 文件可以为该目录添加没有在Options指令中列出的选项。
FileInfo | .htaccess文件包含修改文档类型信息的指令。
AuthConfig | .htaccess文件可能包含验证指令。
Limit | .htaccess文件可能包含allow、deny、order指令。
Indexes | 控制目录列表方式。
None | 禁止处理.htaccess文件。
All | 表示读取以上所有指令内容。



### Options 指令选项

 optsion | description
 :-------|:-------------
 All |使用除MultiViews外的所有参数
 ExecCGI | 运行CGI脚本
 FollowSymLinks |可以访问链接对应的目标文件
 Includes |使用SSI
 IncludesNOEXEC |使用SSI但是不可以运行CGI脚本
 Indexes |支持目录索引,即如果没有index.html文件,则显示目录下的文件列表
 MultiViews |开启内容协商
 SymLinksOwnerMatch |如果链接和其目标文件属于同一个用户,则允许访问目标文件



#### 使用.htaccess文件中配置

> 这是第二种方法,在目录下新建文件.htaccess,
> 用这个的好处就是可以不重启服务器实验,坏处就是多了性能不好   
> .htaccess 是可以通过 AccessFileName .htaccess 指定的,默认好像就是这个名字, 还要把AllowOverride指令设置为all

```  
    #fileName: httpd.conf
      <Directory "O:/Users/Qi/Documents/Project/Website">
          Options FollowSymLinks         
          AllowOverride all    #这个是限制.htaccess文件能够覆盖的内容
      </Directory>
    #fileName: .htaccess

    RewriteEngine on             #打开rewrite引擎
           ......                #其他规则
    RewriteRule ^/(.*)$ $1.php   #把任何不带后缀的文件重写成后缀为.php的文件,简洁
```



### apache rewrite 规则修正符

 修饰符 | 作用
 ----|:------
   R   | 强制外部重定向
   F   | 禁用URL,返回403状态码
   G   | 强制URL为GONE, 返回410状态码
   P   |强制使用代理转发
   L   |表明当前规则是最后一条规则，停止分析以后规则的重写
   N   |重新从第一条规则开始运行重写过程
   C   |与下一条规则关联
