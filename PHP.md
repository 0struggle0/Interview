1. isset() 和 empty() 区别

   Isset判断变量是否存在，可以传入多个变量，若其中一个变量不存在则返回假，empty判断变量是否为空为假，只可传一个变量，如果为空为假则返回真。

2.   请说明 PHP 中传值与传引用的区别。什么时候传值什么时候传引用？

   按值传递：函数范围内对值的任何改变在函数外部都会被忽略

   按引用传递：函数范围内对值的任何改变在函数外部也能反映出这些修改

   优缺点：按值传递时，php必须复制值。特别是对于大型的字符串和对象来说，这将会是一个代价很大的操作。按引用传递则不需要复制值，对于性能提高很有好处。

3. 说几个  php数组相关的函数

   ```php
   array_combine()----通过合并两个数组来创建一个新数组
   range()----创建并返回一个包含指定范围的元素的数组
   compact()----建立一个数组
   array_chunk()----将一个数组分割成多个
   array_merge()----把两个或多个数组合并成一个数组
   array_slice()----在数组中根据条件取出一段值
   array_diff()----返回两个数组的差集数组
   array_intersect()----计算数组的交集
   array_search()----在数组中搜索给定的值
   array_splice()----移除数组的一部分且替代它
   array_key_exists()----判断某个数组中是否存在指定的key
   shuffle()----把数组中的元素按随机顺序重新排列
   array_flip()----交换数组中的键和值
   array_reverse()----将原数组中的元素顺序翻转，创建新的数组并返回
   array_unique()----移除数组中重复的值
   ```

4.   echo(),print(),print_r()的区别？

   Echo，print是PHP语句, print_r是函数,

   Print()只能打印出简单类型变量的值(如int,string)，有返回值。

   print_r()可以打印出复杂类型变量的值(如数组,对象)

   echo 输出一个或者多个字符串，无返回值
   
5.  用PHP写出显示客户端IP与服务器IP的代码

   ```
   获取客户端IP：$_SERVER(“REMOTE_ADDR”);
   获取服务器端IP：$_SERVER["SERVER_ADDR"];  
   ```



6. 写一个函数，能够遍历一个文件夹下的所有文件和子文件夹 

   ```php
   function my_scandir($dir){
        $files = array();
        if ( $handle = opendir($dir) ){
            while ( ($file = readdir($handle)) !== false ) {
                if ( $file != ".." && $file != "." ) {
                if ( is_dir($dir . "/" . $file) ) {
                        $files[$file] = scandir($dir . "/" . $file);
                    }else {
                        $files[] = $file;
                    }
                }
            }
            closedir($handle);
            return $files;
        }
   }
   ```



7.   PHP字符串中单引号与双引号的区别?

   单引号不能解释变量，而双引号可以解释变量

   单引号不能转义字符，在双引号中可以转义字符 

8.   php中,模板引擎的目的是什么? 你用过哪些模板引擎?

   使用模板引擎的目的是使程序的逻辑代码和html界面代码分离开，是程序的结构更清晰。

9. 