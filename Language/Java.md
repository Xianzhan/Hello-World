[Interview](#interview)

[JDK](#jdk)
  - [Proxy](#proxy)
  - [Thread](#thread)

[JVM](#jvm)
  - [ClassFile](#classfile)
  - [GC](#gc)
  - [Native](#native)
    - [Primitive](#primitive)
    - [Thread](#thread)

[Features](#features)
  
[resources](#resources)

# Interview

[JAVA后端面试100 Q&A之第一篇](https://mp.weixin.qq.com/s/AauQPGo44JrkqND5BLsTQg)<br>

# JDK

## Proxy

[【RPC 专栏】深入理解 RPC 之动态代理篇](https://mp.weixin.qq.com/s/VyX3v3vdMP4VnGHMB46FwA)<br>

## Thread

[Java多线程-线程池ThreadPoolExecutor构造方法和规则](https://blog.csdn.net/qq_25806863/article/details/71126867)<br>

# JVM

[关于Jvm知识看这一篇就够了 - 纯洁的微笑](https://zhuanlan.zhihu.com/p/34426768)<br>
[江南白衣 | 关键系统的JVM参数推荐(2018仲夏版)](https://mp.weixin.qq.com/s/FHY0MelBfmgdRpT4zWF9dQ)<br>
[自己关于VM的帖的目录 - RednaxelaFX](http://rednaxelafx.iteye.com/blog/362738)<br>

## ClassFile

[实例分析JAVA CLASS的文件结构](http://mp.weixin.qq.com/s/WiO97u0cyqpdoEkVOZ6D_Q)

## GC

[G1 垃圾收集器之对象分配过程](https://mp.weixin.qq.com/s/eLbibVWsuqXeNOvMMut0XA)<br>

## Native

[openjdk 源码](https://github.com/dmlloyd/openjdk)

### Primitive

C++ | Java
:---:|:---:
`typedef unsigned char   jboolean`| `boolean`
`typedef unsigned short  jchar`|`char`
`typedef short           jshort`|`short`
`typedef float           jfloat`|`float`
`typedef double          jdouble`|`double`

```h
// jni_md.h
#define JNICALL

typedef int jint;
#ifdef _LP64
typedef long jlong;
#else
typedef long long jlong;
#endif

typedef signed char jbyte;

#endif /* !_JAVASOFT_JNI_MD_H_ */


// jni.h
#ifdef __cplusplus

class _jobject {};
class _jclass : public _jobject {};
class _jthrowable : public _jobject {};
class _jstring : public _jobject {};
class _jarray : public _jobject {};
class _jbooleanArray : public _jarray {};
class _jbyteArray : public _jarray {};
class _jcharArray : public _jarray {};
class _jshortArray : public _jarray {};
class _jintArray : public _jarray {};
class _jlongArray : public _jarray {};
class _jfloatArray : public _jarray {};
class _jdoubleArray : public _jarray {};
class _jobjectArray : public _jarray {};

typedef _jobject *jobject;
typedef _jclass *jclass;
typedef _jthrowable *jthrowable;
typedef _jstring *jstring;
typedef _jarray *jarray;
typedef _jbooleanArray *jbooleanArray;
typedef _jbyteArray *jbyteArray;
typedef _jcharArray *jcharArray;
typedef _jshortArray *jshortArray;
typedef _jintArray *jintArray;
typedef _jlongArray *jlongArray;
typedef _jfloatArray *jfloatArray;
typedef _jdoubleArray *jdoubleArray;
typedef _jobjectArray *jobjectArray;

#else

struct _jobject;

typedef struct _jobject *jobject;
typedef jobject jclass;
typedef jobject jthrowable;
typedef jobject jstring;
typedef jobject jarray;
typedef jarray jbooleanArray;
typedef jarray jbyteArray;
typedef jarray jcharArray;
typedef jarray jshortArray;
typedef jarray jintArray;
typedef jarray jlongArray;
typedef jarray jfloatArray;
typedef jarray jdoubleArray;
typedef jarray jobjectArray;

#endif

/*
 * jboolean constants
 */

#define JNI_FALSE 0
#define JNI_TRUE  1
```

### Thread

[JVM原理与实现——Thread](https://juejin.im/entry/5960852cf265da6c2e0f8a31)<br>
[java线程理解以及openjdk中的实现](https://my.oschina.net/xpbob/blog/1626775)

# Features

[JDK Enhancement-Proposal & Roadmap Process](http://openjdk.java.net/jeps/0)<br>

# resources

[【RPC 专栏】深入理解RPC之序列化篇--总结篇](http://mp.weixin.qq.com/s?__biz=MzUzMTA2NTU2Ng==&mid=2247484483&idx=1&sn=b58e64b13743d3f40b8c9c3ee7ec885e&chksm=fa497bf2cd3ef2e47851b447801ec54bb8be29c81cdf450df2a257b3a2d4d97e264f9ca8586e&scene=0#rd)<br>
[POJO VO DAO BO 等缩写的意义](http://mp.weixin.qq.com/s/8CBlzV2nqbtzVmqA1vKz-g)<br>
[关于Class.getResource和ClassLoader.getResource的路径问题](https://www.cnblogs.com/yejg1212/p/3270152.html)<br>
[Download](http://www.oracle.com/technetwork/java/javase/archive-139210.html)
