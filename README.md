#内核对异常应用栈回溯基本方法

1  将 user_stack_unwind.c 编译进内核

2  内核对应用段错误栈回溯实现方法:arm64架构，在 do_page_fault->__do_user_fault 函数中，添加 user_stack_backstrace(regs,current)。mips架构，do_page_fault 函数，if (user_mode(regs))分支里添加，user_stack_backstrace(regs,current)。

3  内核对应用double free 问题栈回溯实现方法: do_send_specific() 函数最后添加，if(SIGABRT == sig) user_stack_backstrace(task_pt_regs(current),current)。arm64架构对double free问题能完整栈回溯，mips架构由于栈回溯原理问题，栈回溯过程会出错返回。

4  其他应用，内核对应用程序所有进程/线程栈回溯，对调试偶然出现的应用程序锁死，实时观察应用程序应用层函数调用流程，有比较大的用处。这个功能目前在开发中。


