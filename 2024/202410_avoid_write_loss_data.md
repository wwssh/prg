write只是保证数据从用户缓冲区拷贝到内核缓冲区，那还是有数据丢失的可能的。所以，内核会尝试最小化延迟写的风险。
一般会有一个内核配置，在`/proc/sys/vm/dirty_expire_centisecs`, 默认是3000，即30秒。也有的设置成300，即3秒。
但是，就算再短，也不能晚班完全避免问题，如果你要确保数据落盘，那就需要调用fsync和放大他sync。但是很奇怪，read，write都不是f开头。
这里的sync却是f开头，因为这里的fsync只会同步一个fd。而sync用有了别的用途，就是sync，它会同步系统中所有的内核缓冲的脏数据到磁盘上。
