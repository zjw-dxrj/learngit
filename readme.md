**判断是不是空**

```c
#define ISspace(x) isspace((int)(x))

#用法
while (!ISspace(buf[i]))
```

**读取resource文件，发送回client**

```c
fgets(buf, sizeof(buf), resource);
while (!feof(resource))
{
	send(client, buf, strlen(buf), 0);
	fgets(buf, sizeof(buf), resource);
}
```

**cgi设置环境变量**

```c
sprintf(query_env, "QUERY_STRING=%s", query_string);
putenv(query_env);
```

**最简单的c语言cgi**

```c
#include <stdio.h>

int main()
{
        printf("Content-Type: text/html\n\n");
        printf("Hello World!\n");
        return 0;
}
```

**getopt() 函数**

```c
const char *str = "p:l:m:o:s:t:c:a:";
    while ((opt = getopt(argc, argv, str)) != -1)
    {
        switch (opt)
        {
        case 'p':   //自定义端口号
        {
            PORT = atoi(optarg);
            break;
        }
        case 'l':   //选择日志写入方式，默认同步写入
        {
            LOGWrite = atoi(optarg);
            break;
        }
```

**SO_LINGER(仅仅适用于TCP,SCTP)**

在默认情况下,当调用close关闭socke的使用,close会立即返回,但是,如果send buffer中还有数据,系统会试着先把send buffer中的数据发送出去,然后close才返回.

SO_LINGER选项则是用来修改这种默认操作的.于SO_LINGER相关联的一个结构体如下:

```c
#include <sys/socket.h>
struct linger {
int l_onoff //0=off, nonzero=on(开关)
int l_linger //linger time(延迟时间)
}
```

**当l_onoff被设置为0的时候**,将会关闭SO_LINGER选项,即TCP或则SCTP保持默认操作:close立即返回.l_linger值被忽略

**l_onoff和l_linger都是非0**

在这种情况下，回事的close返回得到延迟。调用close去关闭socket的时候，内核将会延迟。也就是说，如果send buffer中还有数据尚未发送，该进程将会被休眠直到一下任何一种情况发生：

1) send buffer中的所有数据都被发送并且得到对方TCP的应答消息（这种应答并不是意味着对方应用程序已经接收到数据，在后面shutdown将会具体讲道）
2) 延迟时间消耗完。在延迟时间被消耗完之后，send buffer中的所有数据都将会被丢弃。



**解析空白**

```c
static void lept_parse_whitespace(lept_context* c) {
    const char *p = c->json;
    while (*p == ' ' || *p == '\t' || *p == '\n' || *p == '\r')
        p++;
    c->json = p;
}
```
