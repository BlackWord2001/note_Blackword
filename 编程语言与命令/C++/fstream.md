**fstream**提供了三个类，用来实现c++对文件的操作。（文件的创建、读、写）。
ifstream – 从已有的文件读入
ofstream – 向文件写内容
fstream - 打开文件供读写

+ 文件打开模式    
ios::in 只读    
ios::out 只写   
ios::app 从文件末尾开始写，防止丢失文件中原来就有的内容     
ios::binary 二进制模式  
ios::nocreate 打开一个文件时，如果文件不存在，不创建文件    
ios::noreplace 打开一个文件时，如果文件不存在，创建该文件   
ios::trunc 打开一个文件，然后清空内容   
ios::ate 打开一个文件时，将位置移动到文件尾 

+ 文件指针位置在c++中的用法：  
ios::beg 文件头      
ios::end 文件尾     
ios::cur 当前位置      

