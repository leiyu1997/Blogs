# MySQL常用字符串函数整理

&emsp;在使用MySQL的时候，经常会碰到处理字符串的情况，我总结了常用的字符串函数，或有不全，请包涵，供大家参考，最好收藏。

&emsp;**注：在MySQL中，字符串的起始位置为1，即`'abc'`这个字符串中，`'b'`的位置为2**

---
## 总结

|函数|意义|
|:--|:--|
|[`CONCAT(str1,str2,...)`](#CONCAT(str1,str2,...))|将多个字符串连接起来，如果其中有一个值为空，那么返回的值便为空|
|[`CONCAT_WS(separator,str1,str2,...)`](#CONCAT_WS(separator,str1,str2,...))|将多个字符串连接起来,中间用separator隔开，如果separator值为null将会返回空值，如果字符串值为null，将会跳过|
|[`LEFT(str,len)`](#LEFT(str,len))|返回字符串左侧长度为len的字符串|
|[`RIGHT(str,len)`](#RIGHT(str,len))|返回字符串右侧长度为len的字符串|
|[`STRCMP(str1,str2)`](#STRCMP(str1,str2))|在当前字符集下，比较str1与str2的大小，若相等返回0，str1小于str2则返回-1，大于则返回1，比如ASCII码下，'a'<'b',比较完一位比下一位|
|[`REPLACE(str,from_str,to_str)`](#REPLACE(str,from_str,to_str))|将str中所有的from_str子串替换为to_str|
|[`SPACE(N)`](#SPACE(N))|返回N个空格构成的字符串|
|[`REPEAT(str,count)`](#REPEAT(str,count))|将str重复count次之后返回|
|[`INSTR(str,substr)`](#INSTR(str,substr))|返回str中第一次出现substr的位置|
|[`SUBSTR(str,pos[,len])`](#SUBSTR(str,pos[,len]))|返回从str的指定pos位置从左到右截取的字符串,可选值len表示截取的长度，如果没有，表示截取到最后,如果pos为负值，表示从右向左|
|[`LENGTH(str)`](#LENGTH(str))|返回当前字符集下str所占的字节数，不同编码格式下，汉字所占字节数是不同的，在utf-8中一个汉字占3个字节，而在gbk中只占2个|
|[`CHAR_LENGTH(str)`](#CHAR_LENGTH(str))|返回字符串中字符的个数，不受字符集的影响|
|[`LOWER(str)`](#LOWER(str))|大写转小写|
|[`UPPER(str)`](#UPPER(str))|小写转大写|

---
## 各函数举例

### CONCAT(str1,str2,...)
- 将多个字符串或列的值连接起来
- 如果其中有一个值为空，那么返回的值便为空

    ```
    mysql> select concat('qwer','tyui');
    +-----------------------+
    | concat('qwer','tyui') |
    +-----------------------+
    | qwertyui              |
    +-----------------------+

    mysql> select concat('|',null,'|');
    +----------------------+
    | concat('|',null,'|') |
    +----------------------+
    | NULL                 |
    +----------------------+
    ```

### CONCAT_WS(separator,str1,str2,...)
- 将多个字符串连接起来,中间用separator隔开
- 如果字符串值为null，将会跳过
- 如果separator值为null将会返回空值

    ```
    mysql> select concat_ws(',','qwer','tyui');
    +------------------------------+
    | concat_ws(',','qwer','tyui') |
    +------------------------------+
    | qwer,tyui                    |
    +------------------------------+

    mysql> select concat_ws(',','123',null,'456');
    +---------------------------------+
    | concat_ws(',','123',null,'456') |
    +---------------------------------+
    | 123,456                         |
    +---------------------------------+

    mysql> select concat_ws(null,'123',null,'456');
    +----------------------------------+
    | concat_ws(null,'123',null,'456') |
    +----------------------------------+
    | NULL                             |
    +----------------------------------+
    ```

### LEFT(str,len)
-  返回字符串左侧长度为len的字符串


    ```
    mysql> select left('123456789',5);
    +---------------------+
    | left('123456789',5) |
    +---------------------+
    | 12345               |
    +---------------------+
    ```

### RIGHT(str,len)
- 返回字符串右侧长度为len的字符串


    ```
    mysql> select right('123456789',5);
    +----------------------+
    | right('123456789',5) |
    +----------------------+
    | 56789                |
    +----------------------+
    ```

### STRCMP(str1,str2)
- 在当前字符集下，比较str1与str2的大小
- 若相等返回0，str1小于str2则返回-1，大于则返回1
- 比如ASCII码下，'a'<'b',比较完一位比下一位


    ```
    mysql> select strcmp('bb','b');
    +------------------+
    | strcmp('bb','b') |
    +------------------+
    |                1 |
    +------------------+
    ```

### REPLACE(str,from_str,to_str)
- 将str中所有的from_str子串替换为to_str


    ```
    mysql> select replace('我爱你','爱','也爱');
    +-------------------------------------+
    | replace('我爱你','爱','也爱')       |
    +-------------------------------------+
    | 我也爱你                            |
    +-------------------------------------+
    ```

### SPACE(N)
- 返回N个空格构成的字符串


    ```
    mysql> select concat('|',space(5),'|');
    +--------------------------+
    | concat('|',space(5),'|') |
    +--------------------------+
    | |     |                  |
    +--------------------------+
    ```

### REPEAT(str,count)
- 将str重复count次之后返回


    ```
    mysql> select repeat('我爱你',3);
    +-----------------------------+
    | repeat('我爱你',3)          |
    +-----------------------------+
    | 我爱你我爱你我爱你          |
    +-----------------------------+
    ```

### INSTR(str,substr)
- 返回str中第一次出现substr的位置


    ```
    mysql> select instr('123456789','5');
    +------------------------+
    | instr('123456789','5') |
    +------------------------+
    |                      5 |
    +------------------------+
    ```

### SUBSTR(str,pos[,len])

- 返回从str的指定pos位置从左到右截取的字符串
- 可选值len表示截取的长度，如果没有，表示截取到最后
- 如果pos为负值，表示从右向左
    ```
    mysql> select substr('ILoveU',2);
    +--------------------+
    | substr('ILoveU',2) |
    +--------------------+
    | LoveU              |
    +--------------------+
    1 row in set (0.00 sec)

    mysql> select substr('ILoveU',2,4);
    +----------------------+
    | substr('ILoveU',2,4) |
    +----------------------+
    | Love                 |
    +----------------------+
    1 row in set (0.00 sec)

    mysql> select substr('ILoveU',-5);
    +---------------------+
    | substr('ILoveU',-5) |
    +---------------------+
    | LoveU               |
    +---------------------+
    1 row in set (0.00 sec)
    ```

### LENGTH(str)
- 返回当前字符集下str所占的字节数
- 不同编码格式下，汉字所占字节数是不同的，在utf-8中一个汉字占3个字节，而在gbk中只占2个


    ```
    mysql> select length('我爱你');
    +---------------------+
    | length('我爱你')    |
    +---------------------+
    |                   9 |
    +---------------------+
    1 row in set (0.00 sec)

    mysql> select length('ILoveU');
    +------------------+
    | length('ILoveU') |
    +------------------+
    |                6 |
    +------------------+
    1 row in set (0.00 sec)
    ```

### CHAR_LENGTH(str)
- 返回字符串中字符的个数
- 不受字符集的影响


    ```
    mysql> select char_length('我爱你');
    +--------------------------+
    | char_length('我爱你')    |
    +--------------------------+
    |                        3 |
    +--------------------------+
    1 row in set (0.00 sec)

    mysql> select char_length('ILoveU');
    +-----------------------+
    | char_length('ILoveU') |
    +-----------------------+
    |                     6 |
    +-----------------------+
    1 row in set (0.00 sec)
    ```

### LOWER(str)
- 大写转小写

    ```
    mysql> select lower('MySQL');
    +----------------+
    | lower('MySQL') |
    +----------------+
    | mysql          |
    +----------------+
    ```

### UPPER(str)
- 小写转大写

    ```
    mysql> select upper('mysql');
    +----------------+
    | upper('mysql') |
    +----------------+
    | MYSQL          |
    +----------------+
    ```