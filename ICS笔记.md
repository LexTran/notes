# PA1

首先需要将客户程序读入计算机，这一步骤需要使用monitor监控，因此首先初始化monitor，调用init_monitor( )函数

```c
void init_monitor(int argc, char *argv[]){
  parse_args(argc, argv);//语法分析参数
  init_log(log_file);//打开日志文件
  init_mem();//初始化内存
  init_isa();//初始化指令集
  long img_size = load_img();//载入图片到内存，覆盖built-in图
  init_regex();//初始化常规表达式
  init_wp_pool();//初始化watchpoint pool
  init_difftest(diff_so_file, img_size, difftest_port);//
  welcome();//显示welcome信息
}
```

调用的init_isa( )函数会对指令集进行初始化，这个过程中会内置少数几个无意义函数，然后我们还需要在内存中约定一个固定位置用于读入客户程序

此外，init_isa( )函数还需要调用一个restart( )函数对寄存器进行初始化

```c
static const uint32_t img[] = {
  0x800002b7,
  0x0002a023,
  0x0002a503,
  0x0000006b,
};

static void restart(){
  cpu.pc = PEME_BASE + IMAGE_START;//初始化PC寄存器位置
  cpu.gpr[0]._32 = 0;//定义零寄存器常为零
}

void init_isa(){
  memcpy(guest_to_host(IMAGE_START), img, sizeof(img));
  restart();
}
```

初始化寄存器结构

```c
union{
  union{
    uint32_t _32;
    uint16_t _16;
    uint8_t _8[2];
  }gpr[8];//定义八个通用寄存器，这些寄存器本身是32位，但可分为16位和8位的子寄存器，使用union关键词定义为联合体，使他们共享内存
}
```

# PA2

# PA3

# PA4



