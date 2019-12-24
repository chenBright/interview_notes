# 为什么vector扩容为2倍

`gcc`中的`vector`实现，每次扩容为原来的**2倍**。其实无论是2倍还是多倍，最后均摊计算得到的时间复杂度都是**O(1)**。

其他编译器实现不是选择扩容为2倍，而是在`(1, 2)`之间中选择一个扩容因子。这么选择的原因是出于重用内存空间的考虑。

[folly](https://github.com/facebook/folly/blob/master/folly/docs/FBVector.md#memory-handling)的文档中提到：

> It is well known that `std::vector` grows exponentially (at a constant factor) in order to avoid quadratic growth performance. The trick is choosing a good factor. Any factor greater than 1 ensures O(1) amortized append complexity towards infinity. But a factor that's too small (say, 1.1) causes frequent vector reallocation, and one that's too large (say, 3 or 4) forces the vector to consume much more memory than needed.
>
> The initial HP implementation by Stepanov used a growth factor of 2; i.e., whenever you'd `push_back` into a vector without there being room, it would double the current capacity. This was not a good choice: it can be mathematically proven that a growth factor of 2 is rigorously the *worst* possible because it never allows the vector to reuse any of its previously-allocated memory. Despite other compilers reducing the growth factor to 1.5, gcc has staunchly maintained its factor of 2. This makes `std::vector` cache- unfriendly and memory manager unfriendly.

懒得翻译了，参考[Google翻译](https://translate.google.cn/#view=home&op=translate&sl=en&tl=zh-CN&text=It%20is%20well%20known%20that%20std%3A%3Avector%20grows%20exponentially%20(at%20a%20constant%20factor)%20in%20order%20to%20avoid%20quadratic%20growth%20performance.%20The%20trick%20is%20choosing%20a%20good%20factor.%20Any%20factor%20greater%20than%201%20ensures%20O(1)%20amortized%20append%20complexity%20towards%20infinity.%20But%20a%20factor%20that's%20too%20small%20(say%2C%201.1)%20causes%20frequent%20vector%20reallocation%2C%20and%20one%20that's%20too%20large%20(say%2C%203%20or%204)%20forces%20the%20vector%20to%20consume%20much%20more%20memory%20than%20needed.%0A%0AThe%20initial%20HP%20implementation%20by%20Stepanov%20used%20a%20growth%20factor%20of%202%3B%20i.e.%2C%20whenever%20you'd%20push_back%20into%20a%20vector%20without%20there%20being%20room%2C%20it%20would%20double%20the%20current%20capacity.%20This%20was%20not%20a%20good%20choice%3A%20it%20can%20be%20mathematically%20proven%20that%20a%20growth%20factor%20of%202%20is%20rigorously%20the%20worst%20possible%20because%20it%20never%20allows%20the%20vector%20to%20reuse%20any%20of%20its%20previously-allocated%20memory.%20Despite%20other%20compilers%20reducing%20the%20growth%20factor%20to%201.5%2C%20gcc%20has%20staunchly%20maintained%20its%20factor%20of%202.%20This%20makes%20std%3A%3Avector%20cache-%20unfriendly%20and%20memory%20manager%20unfriendly.%0A%0ATo%20see%20why%20that's%20the%20case%2C%20consider%20a%20large%20vector%20of%20capacity%20C.%20When%20there's%20a%20request%20to%20grow%20the%20vector%2C%20the%20vector%20(assuming%20no%20in-place%20resizing%2C%20see%20the%20appropriate%20section%20in%20this%20document)%20will%20allocate%20a%20chunk%20of%20memory%20next%20to%20its%20current%20chunk%2C%20copy%20its%20existing%20data%20to%20the%20new%20chunk%2C%20and%20then%20deallocate%20the%20old%20chunk.%20So%20now%20we%20have%20a%20chunk%20of%20size%20C%20followed%20by%20a%20chunk%20of%20size%20k%20*%20C.%20Continuing%20this%20process%20we'll%20then%20have%20a%20chunk%20of%20size%20k%20*%20k%20*%20C%20to%20the%20right%20and%20so%20on.%20That%20leads%20to%20a%20series%20of%20the%20form%20(using%20%5E%5E%20for%20power)%3A%0A%0AThis%20means%20that%20any%20new%20chunk%20requested%20will%20be%20larger%20than%20all%20previously%20used%20chunks%20combined%2C%20so%20the%20vector%20must%20crawl%20forward%20in%20memory%3B%20it%20can't%20move%20back%20to%20its%20deallocated%20chunks.%20But%20any%20number%20smaller%20than%202%20guarantees%20that%20you'll%20at%20some%20point%20be%20able%20to%20reuse%20the%20previous%20chunks.%20For%20instance%2C%20choosing%201.5%20as%20the%20factor%20allows%20memory%20reuse%20after%204%20reallocations%3B%201.45%20allows%20memory%20reuse%20after%203%20reallocations%3B%20and%201.3%20allows%20reuse%20after%20only%202%20reallocations.%0A%0AOf%20course%2C%20the%20above%20makes%20a%20number%20of%20simplifying%20assumptions%20about%20how%20the%20memory%20allocator%20works%2C%20but%20definitely%20you%20don't%20want%20to%20choose%20the%20theoretically%20absolute%20worst%20growth%20factor.%20fbvector%20uses%20a%20growth%20factor%20of%201.5.%20That%20does%20not%20impede%20good%20performance%20at%20small%20sizes%20because%20of%20the%20way%20fbvector%20cooperates%20with%20jemalloc%20(below).)。

[folly](https://github.com/facebook/folly/blob/master/folly/docs/FBVector.md#memory-handling)的实现[FBVector.h](https://github.com/facebook/folly/blob/master/folly/FBVector.h#L1180)。

如果使用扩容因子`k = 2`，每次扩展的新尺寸必然刚好大于之前分配的总和：

![memory](./memory.svg)

也就是说，之前分配的内存空间不可能被使用。这样对于缓存并不友好。最好把增长因子设为`1 < k < 2`。例如 Folly 采用 1.5，RapidJSON 也是跟随采用 1.5。

```text
k = 2, c = 4
0123
    01234567
    		    012345789ABCDEF
    		    							 0123456789ABCDEF0123456789ABCDEF
    		    							 																 012345...

k = 1.5, c = 4
0123
    012345
    			012345678
    			 				 0123456789ABCD
    			 				 							 0123456789ABCDEF0123
0123456789ABCDEF0123456789ABCD
															0123456789ABCDEF0123456789ABCDEF...
```

可以看出，`k = 1.5`时，经过 4 次扩容就能重用原来的内存空间。如果`k = 1.45`，经过 3 次扩容就能重用原来的内存空间。如果`k = 1.3`，经过 4 次扩容就能重用原来的内存空间。