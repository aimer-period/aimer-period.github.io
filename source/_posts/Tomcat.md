tomcat是一个使用十分广泛的web容器，springboot都将tomcat设为默认的容器

对于tomcat，它有四个重要的参数，分别是threads.min-spare、threads.max、max-connection、accept-count

我们将tomcat比作一个饭店，那么

1. threads.min-spare（最小工作线程数）就是饭店里面的固定厨师，他们会一直在那里工作
2. threads.max（最大工作线程数）就是饭店很忙的时候加上帮厨的人数，固定厨师忙不过来了就要有人去临时帮忙
3. max-connection（最大连接数）就是饭店里面的餐位，能容纳多少人
4. accept-count（最大等待数）是排队的人数

**最大连接数是8192，最大等待数是100，所以Tomcat容器最大容量是8292**