#使struct包含私有struct附属方法公开
今天hack标准库里的debug/elf,里面有一段代码
```
// A Section represents a single section in an ELF file.
type Section struct {
        SectionHeader

        // Embed ReaderAt for ReadAt method.
        // Do not embed SectionReader directly
        // to avoid having Read and Seek.
        // If a client wants Read and Seek it must use
        // Open() to avoid fighting over the seek offset
        // with other clients.
        io.ReaderAt
        sr *io.SectionReader
}
```
这个结构很特别的一点是为了能够只使用SectionReader的ReadAt方法而不使用其他方法防止Seek出现竞争问题把一个interface io.ReaderAt包含了进来。

io.ReaderAt是包含ReadAt方法的interface，以此目的来达到屏蔽其他方法的目的.如果真的要使用Read+Seek需要另外再调用Open函数。

```
// Open returns a new ReadSeeker reading the ELF section.
func (s *Section) Open() io.ReadSeeker { 
	return io.NewSectionReader(s.sr, 0, 1<<63-1) 
}
```
这里可以看到就是new了一个SectionReader出来用，这样就防止了竞争的问题。

实现的基础就是私有结构的方法是不暴露的,而共有借口的方法是可以暴露的,这样怎么做到呢?
我把代码做了一个抽象,写了一个demo如下：
```
package main

import (
    "fmt"
)

type S struct {
    MethodsMask
    // 这里为小写及为私有字段，在外部是无法使用的
    i Implementer
}

func NewS() *S {
    s := new(S)
    // 将i赋值给 MethodsMask 因为i实现了MethodsMask接口
    // 这样来使得可以访问私有字段 i 的方法（因为接口是公开的）
    s.MethodsMask = s.i
    return s
}

type MethodsMask interface {
    F1()
}

type Implementer struct {

}

func (i Implementer) F1() {
    fmt.Println("F1")
}

func (i Implementer) F2() {
    fmt.Println("F2")
}

func main() {
    s := NewS()
    s.F1()
}
```
说明：
这个程序运行的结果就是F1,注意最关键的部分就在于函数NewS。
把实现者赋值给了接口,因为接口是公开的,所以对应的方法就变成共有的了。

而除此之外的方法还是不能被调用.这样就把一个实现者裁剪了。
当然了,这样的裁剪主要还是为了Seek的竞争。
我觉得适用的场景就是两个借口的并集构成了一个结构体的方法,但是这两个借口泾渭分明的时候就可以这样做。
就像标准库里面分成了io.ReaderAt和io.ReadSeeker一样。

