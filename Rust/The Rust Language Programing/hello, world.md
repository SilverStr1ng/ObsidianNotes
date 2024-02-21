新建rust项目
```bash
$ cargo new world_hello
$ cd world_hello
```

运行项目
	这个命令下, 会在debug模式下运行项目
```bash
$ cargo run
```

	如果想要高性能的代码,可以使用`--release`

```bash
$ cargo run --release
```

验证代码的正确性
	快速的检查一下代码能否编译通过
```bash
$ cargo check
```