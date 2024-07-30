# Component Introduction

The RT-Thread platform incorporates a multitude of commonly utilized components that, when enabled, can readily facilitate the integration of sophisticated functionality, thereby enabling the development of more advanced embedded systems. The RT-Thread platform encompasses a plethora of pre-integrated components, including C libraries, networking components, FinSh console components, FAL components, ulog components, and numerous others. Concurrently, RT-Thread offers a comprehensive array of software packages that can be expediently incorporated into a project via online means. The subsequent section delineates some of the most frequently utilized components and online packages.

# 1 LIBC library

## brief introduction

The LIBC (C library) provided by RT-Thread consists of three parts: the compiler built-in LIBC leveling layer and the POSIX layer. The layout is shown in the following figure:

![libc_structure](E:/_RISC-V RT-Thread课程/2.相关文件/docs-online-master/rt-thread-version/rt-thread-standard/programming-manual/libc/figures/libc_structure.png)

## 1 LIBC levelling layer

The LIBC levelling layer is responsible for interfacing with the underlying stub functions of the compiler toolchain's built-in LIBC and for balancing the differences in the degree of implementation of the compiler's built-in LIBC APIs. It is designed to provide a functionally unified interface to the upper POSIX layer and is located in the[components/libc/compiler](https://github.com/RT-Thread/rt-thread/tree/master/components/libc/compilers) directory.  The rationale behind the levelling process is that the four compilation toolchains, namely GCC (newlib), Keil-MDK, IAR, and Visual Studio (WIN32), exhibit disparate levels of support for the standard C library functions provided by the built-in LIBC. Consequently, the LIBC levelling layer assumes the responsibility of harmonising the standard C libraries provided by the four distinct compilation toolchains to a uniform level. The LIBC levelling layer does not necessitate manual intervention by the user; instead, it automatically levels the libraries in accordance with the compilation platform and toolchain employed by the user during project compilation. This guarantees that the upper layer is not required to distinguish between LIBCs for the specific compilation platform and toolchain used, and that LIBC-related functions can be referenced using common header files.

## 2 ISO/ANSI C standard

The terms ANSI C, ISO C, and Standard C collectively refer to the standards issued by the American National Standards Institute (ANSI) and the International Organization for Standardization (ISO) for the C language. In its historical context, the designation was employed exclusively to denote the original, and most rigorously validated, iteration of this standard, which was designated C89 or C90. Those engaged in software development utilising the C language were encouraged to adhere to the stipulations set forth by the standard, as this facilitated the utilisation of cross-platform code. The inaugural standard for the C language was published by ANSI. The initial standard for C was published by ANSI. Despite the subsequent adoption of this document by the International Organization for Standardization (ISO) and the subsequent publication of revisions by ISO that have been adopted by ANSI, the designation ANSI C (as opposed to ISO C) remains prevalent in usage. Some software developers employ the designation "ISO C," while others utilize the impartial appellation "Standard C." For instance, the terms "C89" (ANSI X3.159-1989), "C99" (ISO/IEC 9899:1999), and "C11" (ISO/IEC 9899:2011) actually pertain to disparate versions of the standard.

It is typical for this standard to be supported by compiler/toolchain-built-in LIBCs. For example, Keil-MDK and IAR provide built-in LIBCs that fulfil this standard.

## 3 POSIX standard

Portable Operating System Interface (English: Portable Operating System Interface, abbreviated as POSIX) is an umbrella term for a series of interrelated IEEE standards that define APIs for running software on a variety of UNIX operating systems, formally known as IEEE Std 1003, and the international standard name is ISO/IEC 9945. ISO/ANSI C is a subset of POSIX.

### 3.1 IEEE Std 1003.1

For LIBC, the widely used POSIX standard is the 1003.1 standard, known as IEEE Std 1003.1 (abbreviated as POSIX.1.) IEEE Std 1003.1-2001, IEEE Std 1003.1-2008, and so on, are the versions of POSIX.1 that have been updated and released at different times.

### 3.2 IEEE Std. 1003.13

The standard is a POSIX specification for real-time profiles. It is a subset of the International Electrotechnical Commission (IEC) 60870-1 standard, which was derived from the IEEE Std 1003.13 standard, which defines four subsets:

- Minimal: The minimal embedded subset specification (PSE51)
The controller subset specification (PSE52) A further subset specification has been developed for larger embedded systems, designated PSE53.
A second subset specification has been created for large-scale general-purpose systems with real-time requirements, designated PSE54.

### 3.3 POSIX standard and RT-Thread implementation

| File                                                         | API                                                          | PSE51 | PSE52 | PSE53 | PSE54 | RT-Thread |
| ------------------------------------------------------------ | ------------------------------------------------------------ | :---: | :---: | :---: | :---: | :-------: |
| [<ctype.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/ctype.h.html) |                                                              |       |       |       |       |           |
|                                                              | [isalnum()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isalnum.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isalpha()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isalpha.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isblank()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isblank.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [iscntrl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iscntrl.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isdigit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isdigit.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isgraph()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isgraph.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [islower()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/islower.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isprint()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isprint.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [ispunct()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ispunct.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isspace()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isspace.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isupper()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isupper.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [isxdigit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isxdigit.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [tolower()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tolower.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [toupper()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/toupper.html) |   *   |   *   |   *   |   *   |     √     |
| [<errno.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/errno.h.html) |                                                              |       |       |       |       |           |
|                                                              | [errno](https://pubs.opengroup.org/onlinepubs/9699919799/functions/errno.html) |   *   |   *   |   *   |   *   |     √     |
| [<fcntl.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/fcntl.h.html) |                                                              |       |       |       |       |           |
|                                                              | [open()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/open.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [creat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/creat.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [fcntl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fcntl.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [posix_fadvise()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_fadvise.html) |       |       |       |   *   |           |
|                                                              | [posix_fallocate()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_fallocate.html) |       |       |       |   *   |           |
| [<fenv.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/fenv.h.html) |                                                              |       |       |       |       |           |
|                                                              | [feclearexcept()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/feclearexcept.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fegetenv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fegetenv.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fegetexceptflag()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fegetexceptflag.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fegetround()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fegetround.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [feholdexcept()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/feholdexcept.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [feraiseexcept()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/feraiseexcept.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fesetenv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fesetenv.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fesetexceptflag()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fesetexceptflag.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fesetround()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fesetround.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fetestexcept()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fetestexcept.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [feupdateenv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/feupdateenv.html) |   *   |   *   |   *   |   *   |     √     |
| [<inttypes.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/inttypes.h.html) |                                                              |       |       |       |       |           |
|                                                              | [imaxabs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/imaxabs.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [imaxdiv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/imaxdiv.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtoimax()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtoimax.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtoumax()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtoumax.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [wcstoimax()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstoimax.html) |       |       |       |   *   |           |
|                                                              | [wcstoumax()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstoumax.html) |       |       |       |   *   |           |
| [<locale.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/locale.h.html) |                                                              |       |       |       |       |           |
|                                                              | [localeconv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/localeconv.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [setlocale()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setlocale.html) |   *   |   *   |   *   |   *   |     √     |
| [<pthread.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/pthread.h.html) |                                                              |       |       |       |       |           |
|                                                              | [pthread_attr_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_destroy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getdetachstate()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getdetachstate.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getguardsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getguardsize.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getinheritsched()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getinheritsched.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getschedparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getschedparam.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getschedpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getschedpolicy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getscope()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getscope.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getstack()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getstack.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getstackaddr()](https://pubs.opengroup.org/onlinepubs/009696799/functions/pthread_attr_getstackaddr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_getstacksize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_getstacksize.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_init.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setdetachstate()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setdetachstate.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setguardsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setguardsize.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setinheritsched()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setinheritsched.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setschedparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setschedparam.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setschedpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setschedpolicy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setscope()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setscope.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setstack()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setstack.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setstackaddr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setstackaddr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_attr_setstacksize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_attr_setstacksize.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cancel()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cancel.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cleanup_pop()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cleanup_pop.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cleanup_push()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cleanup_push.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cond_broadcast()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_broadcast.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cond_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_destroy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cond_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_init.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cond_signal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_signal.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cond_timedwait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_timedwait.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_cond_wait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_wait.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_condattr_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_condattr_destroy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_condattr_getclock()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_condattr_getclock.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_condattr_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_condattr_init.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_condattr_setclock()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_condattr_setclock.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_create()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_create.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_detach()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_detach.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_equal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_equal.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_exit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_exit.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_getconcurrency()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_getconcurrency.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_getschedparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_getschedparam.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_getspecific()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_getspecific.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_join()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_join.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_key_create()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_key_create.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_key_delete()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_key_delete.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutex_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_destroy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutex_getprioceiling()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_getprioceiling.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutex_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_init.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutex_lock()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_lock.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutex_setprioceiling()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_setprioceiling.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutex_trylock()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_trylock.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutex_unlock()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_unlock.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_destroy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_getprioceiling()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_getprioceiling.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_getprotocol()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_getprotocol.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_gettype()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_gettype.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_init.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_setprioceiling()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_setprioceiling.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_setprotocol()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_setprotocol.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_settype()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_settype.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_once()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_once.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_self()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_self.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_setcancelstate()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_setcancelstate.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_setcanceltype()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_setcanceltype.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_setconcurrency()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_setconcurrency.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_setschedparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_setschedparam.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_setschedprio()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_setschedprio.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_setspecific()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_setspecific.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_testcancel()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_testcancel.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_atfork()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_atfork.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_getcpuclockid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_getcpuclockid.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_condattr_getpshared()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_condattr_getpshared.html) |       |       |   *   |   *   |     √     |
|                                                              | [pthread_condattr_setpshared()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_condattr_setpshared.html) |       |       |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_getpshared()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_getpshared.html) |       |       |   *   |   *   |     √     |
|                                                              | [pthread_mutexattr_setpshared()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutexattr_setpshared.html) |       |       |   *   |   *   |     √     |
| [<sched.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sched.h.html) |                                                              |       |       |       |       |           |
|                                                              | [sched_get_priority_max()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_get_priority_max.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sched_get_priority_min()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_get_priority_min.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sched_rr_get_interval()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_rr_get_interval.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sched_yield()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_yield.html) |       |       |   *   |   *   |     √     |
|                                                              | [sched_getparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_getparam.html) |       |       |   *   |   *   |           |
|                                                              | [sched_getscheduler()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_getscheduler.html) |       |       |   *   |   *   |           |
|                                                              | [sched_setparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_setparam.html) |       |       |   *   |   *   |           |
|                                                              | [sched_setscheduler()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sched_setscheduler.html) |       |       |   *   |   *   |     √     |
| [<semaphore.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/semaphore.h.html) |                                                              |       |       |       |       |           |
|                                                              | [sem_close()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_close.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_destroy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_getvalue()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_getvalue.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_init.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_open()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_open.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_post()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_post.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_timedwait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_timedwait.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_trywait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_trywait.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_unlink()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_unlink.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sem_wait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sem_wait.html) |   *   |   *   |   *   |   *   |     √     |
| [<setjmp.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/setjmp.h.html) |                                                              |       |       |       |       |           |
|                                                              | [longjmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/longjmp.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [setjmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setjmp.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [siglongjmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/siglongjmp.html) |       |       |   *   |   *   |           |
|                                                              | [sigsetjmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigsetjmp.html) |       |       |   *   |   *   |           |
| [<signal.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/signal.h.html) |                                                              |       |       |       |       |           |
|                                                              | [kill()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/kill.html) |   *   |   *   |   *   |       |     √     |
|                                                              | [pthread_kill()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_kill.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pthread_sigmask()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_sigmask.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [raise()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/raise.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigaction()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigaction.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigaddset()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigaddset.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigdelset()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigdelset.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigemptyset()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigemptyset.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigfillset()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigfillset.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigismember()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigismember.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [signal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/signal.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigpending()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigpending.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigprocmask()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigprocmask.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigqueue()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigqueue.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [sigsuspend()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigsuspend.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigtimedwait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigtimedwait.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigwait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigwait.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sigwaitinfo()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sigwaitinfo.html) |   *   |   *   |   *   |   *   |     √     |
| [<stdarg.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/stdarg.h.html) |                                                              |       |       |       |       |           |
|                                                              | [va_arg()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/va_arg.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [va_copy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/va_copy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [va_end()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/va_end.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [va_start()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/va_start.html) |   *   |   *   |   *   |   *   |     √     |
| [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/stdio.h.html) |                                                              |       |       |       |       |           |
|                                                              | [clearerr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clearerr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fclose()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fclose.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fdopen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fdopen.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [feof()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/feof.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [ferror()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ferror.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fflush()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fflush.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fgetc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgetc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fgets()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgets.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fileno()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fileno.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [flockfile()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/flockfile.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [fopen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fopen.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fprintf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fputc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fputc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fputs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fputs.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fread()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fread.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [freopen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/freopen.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fscanf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [ftrylockfile()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ftrylockfile.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [funlockfile()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/funlockfile.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [fwrite()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fwrite.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [getc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [getc_unlocked()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getc_unlocked.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [getchar()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getchar.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [getchar_unlocked()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getchar_unlocked.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [gets()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/gets.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [perror()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/perror.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [printf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/printf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [putc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/putc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [putc_unlocked()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/putc_unlocked.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [putchar()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/putchar.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [putchar_unlocked()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/putchar_unlocked.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [puts()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/puts.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [scanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/scanf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [setbuf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setbuf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [setvbuf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setvbuf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [snprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/snprintf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sprintf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sscanf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [stderr](https://pubs.opengroup.org/onlinepubs/9699919799/functions/stderr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [stdin](https://pubs.opengroup.org/onlinepubs/9699919799/functions/stdin.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [stdout](https://pubs.opengroup.org/onlinepubs/9699919799/functions/stdout.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [ungetc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ungetc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [vfprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vfprintf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [vfscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vfscanf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [vprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vprintf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [vscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vscanf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [vsnprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vsnprintf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [vsprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vsprintf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [vsscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vsscanf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fgetpos()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgetpos.html) |       |   *   |   *   |   *   |           |
|                                                              | [fseek()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fseek.html) |       |   *   |   *   |   *   |           |
|                                                              | [fseeko()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fseeko.html) |       |   *   |   *   |   *   |           |
|                                                              | [fsetpos()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fsetpos.html) |       |   *   |   *   |   *   |           |
|                                                              | [ftell()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ftell.html) |       |   *   |   *   |   *   |           |
|                                                              | [ftello()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ftello.html) |       |   *   |   *   |   *   |           |
|                                                              | [remove()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/remove.html) |       |   *   |   *   |   *   |           |
|                                                              | [rename()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rename.html) |       |   *   |   *   |   *   |           |
|                                                              | [rewind()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rewind.html) |       |   *   |   *   |   *   |           |
|                                                              | [tmpfile()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tmpfile.html) |       |   *   |   *   |   *   |           |
|                                                              | [tmpnam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tmpnam.html) |       |   *   |   *   |   *   |           |
|                                                              | [ctermid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctermid.html) |       |       |       |   *   |           |
|                                                              | [pclose()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pclose.html) |       |       |       |   *   |           |
|                                                              | [popen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/popen.html) |       |       |       |   *   |           |
| [<stdlib.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/stdlib.h.html) |                                                              |       |       |       |       |           |
|                                                              | [abort()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/abort.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [abs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/abs.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [atof()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atof.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [atoi()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atoi.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [atol()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atol.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [atoll()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atoll.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [bsearch()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/bsearch.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [calloc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/calloc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [div()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/div.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [free()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/free.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [getenv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getenv.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [labs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/labs.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [ldiv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ldiv.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [llabs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/llabs.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [lldiv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lldiv.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [malloc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/malloc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [mktime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mktime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [qsort()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/qsort.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [rand()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rand.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [rand_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rand_r.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [realloc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/realloc.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [setenv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setenv.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [srand()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/srand.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtod()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtod.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtof()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtof.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtol()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtol.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtold()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtold.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtoll()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtoll.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtoul()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtoul.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtoull()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtoull.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [unsetenv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/unsetenv.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [_Exit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/_Exit.html) |       |       |   *   |   *   |           |
|                                                              | [atexit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atexit.html) |       |       |   *   |   *   |           |
|                                                              | [exit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/exit.html) |       |       |   *   |   *   |     √     |
|                                                              | [mblen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mblen.html) |       |       |       |   *   |           |
|                                                              | [mbstowcs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mbstowcs.html) |       |       |       |   *   |           |
|                                                              | [mbtowc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mbtowc.html) |       |       |       |   *   |           |
|                                                              | [posix_memalign()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_memalign.html) |       |       |       |   *   |           |
|                                                              | [wcstombs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstombs.html) |       |       |       |   *   |           |
|                                                              | [wctomb()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wctomb.html) |       |       |       |   *   |           |
|                                                              | [system()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/system.html) |       |       |       |   *   |           |
| [<string.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/string.h.html) |                                                              |       |       |       |       |           |
|                                                              | [memchr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/memchr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [memcmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/memcmp.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [memcpy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/memcpy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [memmove()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/memmove.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [memset()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/memset.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strcat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strcat.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strchr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strchr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strcmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strcmp.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strcoll()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strcoll.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strcpy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strcpy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strcspn()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strcspn.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strerror()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strerror.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strerror_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strerror_r.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strlen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strlen.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strncat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strncat.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strncmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strncmp.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strncpy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strncpy.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strpbrk()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strpbrk.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strrchr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strrchr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strspn()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strspn.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strstr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strstr.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtok()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtok.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strtok_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strtok_r.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strxfrm()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strxfrm.html) |   *   |   *   |   *   |   *   |     √     |
| [<sys/mman.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sys_mman.h.html) |                                                              |       |       |       |       |           |
|                                                              | [mlockall()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mlockall.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [mmap()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mmap.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [munlock()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/munlock.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [munmap()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/munmap.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [shm_open()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/shm_open.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [shm_unlink()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/shm_unlink.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [msync()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/msync.html) |       |   *   |   *   |   *   |           |
|                                                              | [mprotect()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mprotect.html) |       |       |   *   |   *   |           |
|                                                              | [posix_madvise()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_madvise.html) |       |       |       |   *   |           |
| [<sys/utsname.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sys_utsname.h.html) |                                                              |       |       |       |       |           |
|                                                              | [uname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/uname.html) |   *   |   *   |   *   |   *   |     √     |
| [<time.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/time.h.html) |                                                              |       |       |       |       |           |
|                                                              | [asctime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asctime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [asctime_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asctime_r.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [clock_getres()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clock_getres.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [clock_gettime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clock_gettime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [clock_nanosleep()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clock_nanosleep.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [clock_settime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clock_settime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [ctime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [ctime_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctime_r.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [difftime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/difftime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [gmtime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/gmtime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [gmtime_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/gmtime_r.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [localtime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/localtime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [localtime_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/localtime_r.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [nanosleep()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nanosleep.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [strftime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strftime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [time()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/time.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [timer_create()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/timer_create.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [timer_delete()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/timer_delete.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [timer_getoverrun()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/timer_getoverrun.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [timer_gettime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/timer_gettime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [timer_settime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/timer_settime.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [tzname](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tzname.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [tzset()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tzset.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [clock()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clock.html) |       |       |   *   |   *   |     √     |
|                                                              | [clock_getcpuclockid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clock_getcpuclockid.html) |       |       |   *   |   *   |           |
| [<unistd.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/unistd.h.html) |                                                              |       |       |       |       |           |
|                                                              | [alarm()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/alarm.html) |   *   |   *   |   *   |       |     √     |
|                                                              | [close()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/close.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [environ](https://pubs.opengroup.org/onlinepubs/9699919799/functions/environ.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [fdatasync()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fdatasync.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [fsync()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fsync.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [pause()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pause.html) |   *   |   *   |   *   |       |     √     |
|                                                              | [read()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/read.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [sysconf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sysconf.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [write()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/write.html) |   *   |   *   |   *   |   *   |     √     |
|                                                              | [confstr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/confstr.html) |   *   |   *   |   *   |   *   |           |
|                                                              | [access()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/access.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [chdir()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/chdir.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [dup()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/dup.html) |       |   *   |   *   |   *   |           |
|                                                              | [dup2()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/dup2.html) |       |   *   |   *   |   *   |           |
|                                                              | [fpathconf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fpathconf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ftruncate()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ftruncate.html) |       |   *   |   *   |   *   |           |
|                                                              | [getcwd()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getcwd.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [link()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/link.html) |       |   *   |   *   |   *   |           |
|                                                              | [lseek()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lseek.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [pathconf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pathconf.html) |       |   *   |   *   |   *   |           |
|                                                              | [rmdir()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rmdir.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [unlink()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/unlink.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [_exit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/_exit.html) |       |       |   *   |   *   |           |
|                                                              | [gethostname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/gethostname.html) |       |       |   *   |   *   |           |
|                                                              | [getpgrp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpgrp.html) |       |       |   *   |   *   |           |
|                                                              | [getpid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpid.html) |       |       |   *   |   *   |     √     |
|                                                              | [getppid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getppid.html) |       |       |   *   |   *   |     √     |
|                                                              | [pipe()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pipe.html) |       |       |   *   |   *   |     √     |
|                                                              | [setsid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setsid.html) |       |       |   *   |   *   |           |
|                                                              | [sleep()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sleep.html) |       |       |   *   |   *   |     √     |
|                                                              | [execl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/execl.html) |       |       |   *   |   *   |           |
|                                                              | [execle()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/execle.html) |       |       |   *   |   *   |           |
|                                                              | [execlp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/execlp.html) |       |       |   *   |   *   |           |
|                                                              | [execv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/execv.html) |       |       |   *   |   *   |           |
|                                                              | [execve()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/execve.html) |       |       |   *   |   *   |           |
|                                                              | [execvp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/execvp.html) |       |       |   *   |   *   |           |
|                                                              | [fork()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fork.html) |       |       |   *   |   *   |           |
|                                                              | [chown()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/chown.html) |       |       |       |   *   |           |
|                                                              | [fchown()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fchown.html) |       |       |       |   *   |           |
|                                                              | [getegid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getegid.html) |       |       |       |   *   |           |
|                                                              | [geteuid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/geteuid.html) |       |       |       |   *   |           |
|                                                              | [getgid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getgid.html) |       |       |       |   *   |           |
|                                                              | [getgroups()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getgroups.html) |       |       |       |   *   |           |
|                                                              | [getlogin()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getlogin.html) |       |       |       |   *   |           |
|                                                              | [getlogin_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getlogin_r.html) |       |       |       |   *   |           |
|                                                              | [getopt()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getopt.html) |       |       |       |   *   |           |
|                                                              | [getuid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getuid.html) |       |       |       |   *   |           |
|                                                              | [isatty()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isatty.html) |       |       |       |   *   |           |
|                                                              | [optarg](https://pubs.opengroup.org/onlinepubs/9699919799/functions/optarg.html) |       |       |       |   *   |           |
|                                                              | [opterr](https://pubs.opengroup.org/onlinepubs/9699919799/functions/opterr.html) |       |       |       |   *   |           |
|                                                              | [optind](https://pubs.opengroup.org/onlinepubs/9699919799/functions/optind.html) |       |       |       |   *   |           |
|                                                              | [optopt](https://pubs.opengroup.org/onlinepubs/9699919799/functions/optopt.html) |       |       |       |   *   |           |
|                                                              | [readlink()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/readlink.html) |       |       |       |   *   |           |
|                                                              | [setegid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setegid.html) |       |       |       |   *   |           |
|                                                              | [seteuid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/seteuid.html) |       |       |       |   *   |           |
|                                                              | [setgid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setgid.html) |       |       |       |   *   |           |
|                                                              | [setpgid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setpgid.html) |       |       |       |   *   |           |
|                                                              | [setuid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setuid.html) |       |       |       |   *   |           |
|                                                              | [symlink()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/symlink.html) |       |       |       |   *   |           |
|                                                              | [tcgetpgrp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcgetpgrp.html) |       |       |       |   *   |           |
|                                                              | [tcsetpgrp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcsetpgrp.html) |       |       |       |   *   |           |
|                                                              | [ttyname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ttyname.html) |       |       |       |   *   |           |
|                                                              | [ttyname_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ttyname_r.html) |       |       |       |   *   |           |
| [<complex.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/complex.h.html) |                                                              |       |       |       |       |           |
|                                                              | [cabs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cabs.html) |       |   *   |   *   |   *   |           |
|                                                              | [cabsf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cabsf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cabsl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cabsl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cacos()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cacos.html) |       |   *   |   *   |   *   |           |
|                                                              | [cacosf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cacosf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cacosh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cacosh.html) |       |   *   |   *   |   *   |           |
|                                                              | [cacoshf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cacoshf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cacoshl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cacoshl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cacosl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cacosl.html) |       |   *   |   *   |   *   |           |
|                                                              | [carg()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/carg.html) |       |   *   |   *   |   *   |           |
|                                                              | [cargf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cargf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cargl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cargl.html) |       |   *   |   *   |   *   |           |
|                                                              | [casin()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/casin.html) |       |   *   |   *   |   *   |           |
|                                                              | [casinf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/casinf.html) |       |   *   |   *   |   *   |           |
|                                                              | [casinh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/casinh.html) |       |   *   |   *   |   *   |           |
|                                                              | [casinhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/casinhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [casinhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/casinhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [casinl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/casinl.html) |       |   *   |   *   |   *   |           |
|                                                              | [catan()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/catan.html) |       |   *   |   *   |   *   |           |
|                                                              | [catanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/catanf.html) |       |   *   |   *   |   *   |           |
|                                                              | [catanh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/catanh.html) |       |   *   |   *   |   *   |           |
|                                                              | [catanhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/catanhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [catanhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/catanhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [catanl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/catanl.html) |       |   *   |   *   |   *   |           |
|                                                              | [ccos()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ccos.html) |       |   *   |   *   |   *   |           |
|                                                              | [ccosf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ccosf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ccosh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ccosh.html) |       |   *   |   *   |   *   |           |
|                                                              | [ccoshf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ccoshf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ccoshl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ccoshl.html) |       |   *   |   *   |   *   |           |
|                                                              | [ccosl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ccosl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cexp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cexp.html) |       |   *   |   *   |   *   |           |
|                                                              | [cexpf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cexpf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cexpl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cexpl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cimag()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cimag.html) |       |   *   |   *   |   *   |           |
|                                                              | [cimagf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cimagf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cimagl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cimagl.html) |       |   *   |   *   |   *   |           |
|                                                              | [clog()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clog.html) |       |   *   |   *   |   *   |           |
|                                                              | [clogf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clogf.html) |       |   *   |   *   |   *   |           |
|                                                              | [clogl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/clogl.html) |       |   *   |   *   |   *   |           |
|                                                              | [conj()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/conj.html) |       |   *   |   *   |   *   |           |
|                                                              | [conjf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/conjf.html) |       |   *   |   *   |   *   |           |
|                                                              | [conjl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/conjl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cpow()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cpow.html) |       |   *   |   *   |   *   |           |
|                                                              | [cpowf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cpowf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cpowl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cpowl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cproj()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cproj.html) |       |   *   |   *   |   *   |           |
|                                                              | [cprojf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cprojf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cprojl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cprojl.html) |       |   *   |   *   |   *   |           |
|                                                              | [creal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/creal.html) |       |   *   |   *   |   *   |           |
|                                                              | [crealf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/crealf.html) |       |   *   |   *   |   *   |           |
|                                                              | [creall()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/creall.html) |       |   *   |   *   |   *   |           |
|                                                              | [csin()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csin.html) |       |   *   |   *   |   *   |           |
|                                                              | [csinf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csinf.html) |       |   *   |   *   |   *   |           |
|                                                              | [csinh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csinh.html) |       |   *   |   *   |   *   |           |
|                                                              | [csinhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csinhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [csinhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csinhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [csinl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csinl.html) |       |   *   |   *   |   *   |           |
|                                                              | [csqrt()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csqrt.html) |       |   *   |   *   |   *   |           |
|                                                              | [csqrtf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csqrtf.html) |       |   *   |   *   |   *   |           |
|                                                              | [csqrtl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/csqrtl.html) |       |   *   |   *   |   *   |           |
|                                                              | [ctan()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctan.html) |       |   *   |   *   |   *   |           |
|                                                              | [ctanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctanf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ctanh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctanh.html) |       |   *   |   *   |   *   |           |
|                                                              | [ctanhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctanhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ctanhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctanhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [ctanl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ctanl.html) |       |   *   |   *   |   *   |           |
| [<dirent.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/dirent.h.html) |                                                              |       |       |       |       |           |
|                                                              | [closedir()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/closedir.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [opendir()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/opendir.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [readdir()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/readdir.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [readdir_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/readdir_r.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [rewinddir()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rewinddir.html) |       |   *   |   *   |   *   |     √     |
| [<math.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/math.h.html) |                                                              |       |       |       |       |           |
|                                                              | [acos()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/acos.html) |       |   *   |   *   |   *   |           |
|                                                              | [acosf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/acosf.html) |       |   *   |   *   |   *   |           |
|                                                              | [acosh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/acosh.html) |       |   *   |   *   |   *   |           |
|                                                              | [acoshf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/acoshf.html) |       |   *   |   *   |   *   |           |
|                                                              | [acoshl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/acoshl.html) |       |   *   |   *   |   *   |           |
|                                                              | [acosl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/acosl.html) |       |   *   |   *   |   *   |           |
|                                                              | [asin()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asin.html) |       |   *   |   *   |   *   |           |
|                                                              | [asinf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asinf.html) |       |   *   |   *   |   *   |           |
|                                                              | [asinh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asinh.html) |       |   *   |   *   |   *   |           |
|                                                              | [asinhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asinhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [asinhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asinhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [asinl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/asinl.html) |       |   *   |   *   |   *   |           |
|                                                              | [atan()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atan.html) |       |   *   |   *   |   *   |           |
|                                                              | [atan2()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atan2.html) |       |   *   |   *   |   *   |           |
|                                                              | [atan2f()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atan2f.html) |       |   *   |   *   |   *   |           |
|                                                              | [atan2l()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atan2l.html) |       |   *   |   *   |   *   |           |
|                                                              | [atanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atanf.html) |       |   *   |   *   |   *   |           |
|                                                              | [atanh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atanh.html) |       |   *   |   *   |   *   |           |
|                                                              | [atanhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atanhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [atanhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atanhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [atanl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/atanl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cbrt()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cbrt.html) |       |   *   |   *   |   *   |           |
|                                                              | [cbrtf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cbrtf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cbrtl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cbrtl.html) |       |   *   |   *   |   *   |           |
|                                                              | [ceil()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ceil.html) |       |   *   |   *   |   *   |           |
|                                                              | [ceilf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ceilf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ceill()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ceill.html) |       |   *   |   *   |   *   |           |
|                                                              | [copysign()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/copysign.html) |       |   *   |   *   |   *   |           |
|                                                              | [copysignf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/copysignf.html) |       |   *   |   *   |   *   |           |
|                                                              | [copysignl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/copysignl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cos()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cos.html) |       |   *   |   *   |   *   |           |
|                                                              | [cosf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cosf.html) |       |   *   |   *   |   *   |           |
|                                                              | [cosh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cosh.html) |       |   *   |   *   |   *   |           |
|                                                              | [coshf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/coshf.html) |       |   *   |   *   |   *   |           |
|                                                              | [coshl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/coshl.html) |       |   *   |   *   |   *   |           |
|                                                              | [cosl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cosl.html) |       |   *   |   *   |   *   |           |
|                                                              | [erf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/erf.html) |       |   *   |   *   |   *   |           |
|                                                              | [erfc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/erfc.html) |       |   *   |   *   |   *   |           |
|                                                              | [erfcf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/erfcf.html) |       |   *   |   *   |   *   |           |
|                                                              | [erfcl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/erfcl.html) |       |   *   |   *   |   *   |           |
|                                                              | [erff()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/erff.html) |       |   *   |   *   |   *   |           |
|                                                              | [erfl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/erfl.html) |       |   *   |   *   |   *   |           |
|                                                              | [exp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/exp.html) |       |   *   |   *   |   *   |           |
|                                                              | [exp2()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/exp2.html) |       |   *   |   *   |   *   |           |
|                                                              | [exp2f()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/exp2f.html) |       |   *   |   *   |   *   |           |
|                                                              | [exp2l()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/exp2l.html) |       |   *   |   *   |   *   |           |
|                                                              | [expf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/expf.html) |       |   *   |   *   |   *   |           |
|                                                              | [expl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/expl.html) |       |   *   |   *   |   *   |           |
|                                                              | [expm1()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/expm1.html) |       |   *   |   *   |   *   |           |
|                                                              | [expm1f()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/expm1f.html) |       |   *   |   *   |   *   |           |
|                                                              | [expm1l()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/expm1l.html) |       |   *   |   *   |   *   |           |
|                                                              | [fabs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fabs.html) |       |   *   |   *   |   *   |           |
|                                                              | [fabsf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fabsf.html) |       |   *   |   *   |   *   |           |
|                                                              | [fabsl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fabsl.html) |       |   *   |   *   |   *   |           |
|                                                              | [fdim()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fdim.html) |       |   *   |   *   |   *   |           |
|                                                              | [fdimf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fdimf.html) |       |   *   |   *   |   *   |           |
|                                                              | [fdiml()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fdiml.html) |       |   *   |   *   |   *   |           |
|                                                              | [floor()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/floor.html) |       |   *   |   *   |   *   |           |
|                                                              | [floorf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/floorf.html) |       |   *   |   *   |   *   |           |
|                                                              | [floorl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/floorl.html) |       |   *   |   *   |   *   |           |
|                                                              | [fma()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fma.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmaf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmaf.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmal.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmax()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmax.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmaxf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmaxf.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmaxl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmaxl.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmin()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmin.html) |       |   *   |   *   |   *   |           |
|                                                              | [fminf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fminf.html) |       |   *   |   *   |   *   |           |
|                                                              | [fminl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fminl.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmod()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmod.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmodf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmodf.html) |       |   *   |   *   |   *   |           |
|                                                              | [fmodl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fmodl.html) |       |   *   |   *   |   *   |           |
|                                                              | [fpclassify()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fpclassify.html) |       |   *   |   *   |   *   |           |
|                                                              | [frexp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/frexp.html) |       |   *   |   *   |   *   |           |
|                                                              | [frexpf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/frexpf.html) |       |   *   |   *   |   *   |           |
|                                                              | [frexpl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/frexpl.html) |       |   *   |   *   |   *   |           |
|                                                              | [hypot()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/hypot.html) |       |   *   |   *   |   *   |           |
|                                                              | [hypotf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/hypotf.html) |       |   *   |   *   |   *   |           |
|                                                              | [hypotl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/hypotl.html) |       |   *   |   *   |   *   |           |
|                                                              | [ilogb()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ilogb.html) |       |   *   |   *   |   *   |           |
|                                                              | [ilogbf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ilogbf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ilogbl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ilogbl.html) |       |   *   |   *   |   *   |           |
|                                                              | [isfinite()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isfinite.html) |       |   *   |   *   |   *   |           |
|                                                              | [isgreater()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isgreater.html) |       |   *   |   *   |   *   |           |
|                                                              | [isgreaterequal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isgreaterequal.html) |       |   *   |   *   |   *   |           |
|                                                              | [isinf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isinf.html) |       |   *   |   *   |   *   |           |
|                                                              | [isless()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isless.html) |       |   *   |   *   |   *   |           |
|                                                              | [islessequal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/islessequal.html) |       |   *   |   *   |   *   |           |
|                                                              | [islessgreater()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/islessgreater.html) |       |   *   |   *   |   *   |           |
|                                                              | [isnan()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isnan.html) |       |   *   |   *   |   *   |           |
|                                                              | [isnormal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isnormal.html) |       |   *   |   *   |   *   |           |
|                                                              | [isunordered()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/isunordered.html) |       |   *   |   *   |   *   |           |
|                                                              | [ldexp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ldexp.html) |       |   *   |   *   |   *   |           |
|                                                              | [ldexpf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ldexpf.html) |       |   *   |   *   |   *   |           |
|                                                              | [ldexpl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ldexpl.html) |       |   *   |   *   |   *   |           |
|                                                              | [lgamma()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lgamma.html) |       |   *   |   *   |   *   |           |
|                                                              | [lgammaf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lgammaf.html) |       |   *   |   *   |   *   |           |
|                                                              | [lgammal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lgammal.html) |       |   *   |   *   |   *   |           |
|                                                              | [llrint()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/llrint.html) |       |   *   |   *   |   *   |           |
|                                                              | [llrintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/llrintf.html) |       |   *   |   *   |   *   |           |
|                                                              | [llrintl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/llrintl.html) |       |   *   |   *   |   *   |           |
|                                                              | [llround()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/llround.html) |       |   *   |   *   |   *   |           |
|                                                              | [llroundf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/llroundf.html) |       |   *   |   *   |   *   |           |
|                                                              | [llroundl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/llroundl.html) |       |   *   |   *   |   *   |           |
|                                                              | [log()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log.html) |       |   *   |   *   |   *   |           |
|                                                              | [log10()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log10.html) |       |   *   |   *   |   *   |           |
|                                                              | [log10f()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log10f.html) |       |   *   |   *   |   *   |           |
|                                                              | [log10l()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log10l.html) |       |   *   |   *   |   *   |           |
|                                                              | [log1p()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log1p.html) |       |   *   |   *   |   *   |           |
|                                                              | [log1pf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log1pf.html) |       |   *   |   *   |   *   |           |
|                                                              | [log1pl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log1pl.html) |       |   *   |   *   |   *   |           |
|                                                              | [log2()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log2.html) |       |   *   |   *   |   *   |           |
|                                                              | [log2f()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log2f.html) |       |   *   |   *   |   *   |           |
|                                                              | [log2l()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/log2l.html) |       |   *   |   *   |   *   |           |
|                                                              | [logb()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/logb.html) |       |   *   |   *   |   *   |           |
|                                                              | [logbf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/logbf.html) |       |   *   |   *   |   *   |           |
|                                                              | [logbl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/logbl.html) |       |   *   |   *   |   *   |           |
|                                                              | [logf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/logf.html) |       |   *   |   *   |   *   |           |
|                                                              | [logl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/logl.html) |       |   *   |   *   |   *   |           |
|                                                              | [lrint()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lrint.html) |       |   *   |   *   |   *   |           |
|                                                              | [lrintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lrintf.html) |       |   *   |   *   |   *   |           |
|                                                              | [lrintl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lrintl.html) |       |   *   |   *   |   *   |           |
|                                                              | [lround()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lround.html) |       |   *   |   *   |   *   |           |
|                                                              | [lroundf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lroundf.html) |       |   *   |   *   |   *   |           |
|                                                              | [lroundl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lroundl.html) |       |   *   |   *   |   *   |           |
|                                                              | [modf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/modf.html) |       |   *   |   *   |   *   |           |
|                                                              | [modff()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/modff.html) |       |   *   |   *   |   *   |           |
|                                                              | [modfl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/modfl.html) |       |   *   |   *   |   *   |           |
|                                                              | [nan()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nan.html) |       |   *   |   *   |   *   |           |
|                                                              | [nanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nanf.html) |       |   *   |   *   |   *   |           |
|                                                              | [nanl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nanl.html) |       |   *   |   *   |   *   |           |
|                                                              | [nearbyint()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nearbyint.html) |       |   *   |   *   |   *   |           |
|                                                              | [nearbyintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nearbyintf.html) |       |   *   |   *   |   *   |           |
|                                                              | [nearbyintl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nearbyintl.html) |       |   *   |   *   |   *   |           |
|                                                              | [nextafter()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nextafter.html) |       |   *   |   *   |   *   |           |
|                                                              | [nextafterf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nextafterf.html) |       |   *   |   *   |   *   |           |
|                                                              | [nextafterl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nextafterl.html) |       |   *   |   *   |   *   |           |
|                                                              | [nexttoward()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nexttoward.html) |       |   *   |   *   |   *   |           |
|                                                              | [nexttowardl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nexttowardl.html) |       |   *   |   *   |   *   |           |
|                                                              | [pow()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pow.html) |       |   *   |   *   |   *   |           |
|                                                              | [powf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/powf.html) |       |   *   |   *   |   *   |           |
|                                                              | [powl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/powl.html) |       |   *   |   *   |   *   |           |
|                                                              | [remainder()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/remainder.html) |       |   *   |   *   |   *   |           |
|                                                              | [remainderf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/remainderf.html) |       |   *   |   *   |   *   |           |
|                                                              | [remainderl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/remainderl.html) |       |   *   |   *   |   *   |           |
|                                                              | [remquo()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/remquo.html) |       |   *   |   *   |   *   |           |
|                                                              | [remquof()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/remquof.html) |       |   *   |   *   |   *   |           |
|                                                              | [remquol()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/remquol.html) |       |   *   |   *   |   *   |           |
|                                                              | [rint()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rint.html) |       |   *   |   *   |   *   |           |
|                                                              | [rintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rintf.html) |       |   *   |   *   |   *   |           |
|                                                              | [rintl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/rintl.html) |       |   *   |   *   |   *   |           |
|                                                              | [round()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/round.html) |       |   *   |   *   |   *   |           |
|                                                              | [roundf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/roundf.html) |       |   *   |   *   |   *   |           |
|                                                              | [roundl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/roundl.html) |       |   *   |   *   |   *   |           |
|                                                              | [scalbln()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/scalbln.html) |       |   *   |   *   |   *   |           |
|                                                              | [scalblnf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/scalblnf.html) |       |   *   |   *   |   *   |           |
|                                                              | [scalblnl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/scalblnl.html) |       |   *   |   *   |   *   |           |
|                                                              | [scalbn()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/scalbn.html) |       |   *   |   *   |   *   |           |
|                                                              | [scalbnf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/scalbnf.html) |       |   *   |   *   |   *   |           |
|                                                              | [scalbnl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/scalbnl.html) |       |   *   |   *   |   *   |           |
|                                                              | [sin()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sin.html) |       |   *   |   *   |   *   |           |
|                                                              | [sinf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sinf.html) |       |   *   |   *   |   *   |           |
|                                                              | [sinh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sinh.html) |       |   *   |   *   |   *   |           |
|                                                              | [sinhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sinhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [sinhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sinhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [sinl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sinl.html) |       |   *   |   *   |   *   |           |
|                                                              | [sqrt()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sqrt.html) |       |   *   |   *   |   *   |           |
|                                                              | [sqrtf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sqrtf.html) |       |   *   |   *   |   *   |           |
|                                                              | [sqrtl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sqrtl.html) |       |   *   |   *   |   *   |           |
|                                                              | [tan()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tan.html) |       |   *   |   *   |   *   |           |
|                                                              | [tanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tanf.html) |       |   *   |   *   |   *   |           |
|                                                              | [tanh()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tanh.html) |       |   *   |   *   |   *   |           |
|                                                              | [tanhf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tanhf.html) |       |   *   |   *   |   *   |           |
|                                                              | [tanhl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tanhl.html) |       |   *   |   *   |   *   |           |
|                                                              | [tanl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tanl.html) |       |   *   |   *   |   *   |           |
|                                                              | [tgamma()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tgamma.html) |       |   *   |   *   |   *   |           |
|                                                              | [tgammaf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tgammaf.html) |       |   *   |   *   |   *   |           |
|                                                              | [tgammal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tgammal.html) |       |   *   |   *   |   *   |           |
|                                                              | [trunc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/trunc.html) |       |   *   |   *   |   *   |           |
|                                                              | [truncf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/truncf.html) |       |   *   |   *   |   *   |           |
|                                                              | [truncl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/truncl.html) |       |   *   |   *   |   *   |           |
|                                                              | [nexttowardf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/nexttowardf.html) |       |   *   |   *   |   *   |           |
|                                                              | [signbit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/signbit.html) |       |   *   |   *   |   *   |           |
| [<mqueue.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/mqueue.h.html) |                                                              |       |       |       |       |           |
|                                                              | [mq_close()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_close.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [mq_getattr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_getattr.html) |       |   *   |   *   |   *   |           |
|                                                              | [mq_notify()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_notify.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [mq_open()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_open.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [mq_receive()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_receive.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [mq_send()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_send.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [mq_setattr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_setattr.html) |       |   *   |   *   |   *   |           |
|                                                              | [mq_timedreceive()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_timedreceive.html) |       |   *   |   *   |   *   |           |
|                                                              | [mq_timedsend()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_timedsend.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [mq_unlink()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mq_unlink.html) |       |   *   |   *   |   *   |     √     |
| [<sys/stat.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sys_stat.h.html) |                                                              |       |       |       |       |           |
|                                                              | [fstat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fstat.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [mkdir()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mkdir.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [stat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/stat.html) |       |   *   |   *   |   *   |     √     |
|                                                              | [chmod()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/chmod.html) |       |       |       |   *   |           |
|                                                              | [fchmod()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fchmod.html) |       |       |       |   *   |           |
|                                                              | [lstat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lstat.html) |       |       |       |   *   |           |
|                                                              | [mkfifo()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mkfifo.html) |       |       |       |   *   |           |
|                                                              | [umask()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/umask.html) |       |       |       |   *   |           |
| [<trace.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/trace.h.html) |                                                              |       |       |       |       |           |
|                                                              | [posix_trace_attr_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_destroy.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getclockres()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getclockres.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getcreatetime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getcreatetime.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getgenversion()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getgenversion.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getinherited()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getinherited.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getlogfullpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getlogfullpolicy.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getlogsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getlogsize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getmaxdatasize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getmaxdatasize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getmaxsystemeventsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getmaxsystemeventsize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getmaxusereventsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getmaxusereventsize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getname.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getstreamfullpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getstreamfullpolicy.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_getstreamsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_getstreamsize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_init.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_setinherited()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_setinherited.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_setlogfullpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_setlogfullpolicy.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_setlogsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_setlogsize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_setmaxdatasize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_setmaxdatasize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_setname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_setname.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_setstreamfullpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_setstreamfullpolicy.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_attr_setstreamsize()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_attr_setstreamsize.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_clear()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_clear.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_close()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_close.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_create()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_create.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_create_withlog()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_create_withlog.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_event()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_event.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventid_equal()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventid_equal.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventid_get_name()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventid_get_name.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventid_open()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventid_open.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventset_add()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventset_add.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventset_del()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventset_del.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventset_empty()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventset_empty.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventset_fill()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventset_fill.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventset_ismember()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventset_ismember.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventtypelist_getnext_id()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventtypelist_getnext_id.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_eventtypelist_rewind()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_eventtypelist_rewind.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_flush()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_flush.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_get_attr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_get_attr.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_get_filter()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_get_filter.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_get_status()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_get_status.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_getnext_event()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_getnext_event.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_open()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_open.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_rewind()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_rewind.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_set_filter()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_set_filter.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_shutdown()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_shutdown.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_start()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_start.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_stop()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_stop.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_timedgetnext_event()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_timedgetnext_event.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_trid_eventid_open()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_trid_eventid_open.html) |       |   *   |   *   |   *   |           |
|                                                              | [posix_trace_trygetnext_event()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_trace_trygetnext_event.html) |       |   *   |   *   |   *   |           |
| [<utime.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/utime.h.html) |                                                              |       |       |       |       |           |
|                                                              | [utime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/utime.html) |       |   *   |   *   |   *   |           |
| [<aio.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/aio.h.html) |                                                              |       |       |       |       |           |
|                                                              | [aio_cancel()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/aio_cancel.html) |       |       |   *   |   *   |     √     |
|                                                              | [aio_error()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/aio_error.html) |       |       |   *   |   *   |     √     |
|                                                              | [aio_fsync()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/aio_fsync.html) |       |       |   *   |   *   |     √     |
|                                                              | [aio_read()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/aio_read.html) |       |       |   *   |   *   |     √     |
|                                                              | [aio_return()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/aio_return.html) |       |       |   *   |   *   |     √     |
|                                                              | [aio_write()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/aio_write.html) |       |       |   *   |   *   |     √     |
|                                                              | [aio_suspend()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/aio_suspend.html) |       |       |   *   |   *   |     √     |
|                                                              | [lio_listio()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/lio_listio.html) |       |       |   *   |   *   |     √     |
| [<arpa/inet.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/arpa_inet.h.html) |                                                              |       |       |       |       |           |
|                                                              | [htonl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/htonl.html) |       |       |   *   |   *   |     √     |
|                                                              | [htons()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/htons.html) |       |       |   *   |   *   |     √     |
|                                                              | [inet_addr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/inet_addr.html) |       |       |   *   |   *   |     √     |
|                                                              | [inet_ntoa()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/inet_ntoa.html) |       |       |   *   |   *   |     √     |
|                                                              | [inet_ntop()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/inet_ntop.html) |       |       |   *   |   *   |     √     |
|                                                              | [inet_pton()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/inet_pton.html) |       |       |   *   |   *   |     √     |
|                                                              | [ntohl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ntohl.html) |       |       |   *   |   *   |     √     |
|                                                              | [ntohs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ntohs.html) |       |       |   *   |   *   |     √     |
| [<assert.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/assert.h.html) |                                                              |       |       |       |       |           |
|                                                              | [assert()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/assert.html) |       |       |   *   |   *   |     √     |
| [<net/if.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/net_if.h.html) |                                                              |       |       |       |       |           |
|                                                              | [if_freenameindex()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/if_freenameindex.html) |       |       |   *   |   *   |     √     |
|                                                              | [if_indextoname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/if_indextoname.html) |       |       |   *   |   *   |     √     |
|                                                              | [if_nameindex()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/if_nameindex.html) |       |       |   *   |   *   |     √     |
|                                                              | [if_nametoindex()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/if_nametoindex.html) |       |       |   *   |   *   |     √     |
| [<netdb.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/netdb.h.html) |                                                              |       |       |       |       |           |
|                                                              | [endhostent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/endhostent.html) |       |       |   *   |   *   |           |
|                                                              | [endnetent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/endnetent.html) |       |       |   *   |   *   |           |
|                                                              | [endprotoent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/endprotoent.html) |       |       |   *   |   *   |           |
|                                                              | [endservent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/endservent.html) |       |       |   *   |   *   |           |
|                                                              | [freeaddrinfo()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/freeaddrinfo.html) |       |       |   *   |   *   |     √     |
|                                                              | [gai_strerror()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/gai_strerror.html) |       |       |   *   |   *   |     √     |
|                                                              | [getaddrinfo()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getaddrinfo.html) |       |       |   *   |   *   |     √     |
|                                                              | [gethostent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/gethostent.html) |       |       |   *   |   *   |     √     |
|                                                              | [getnameinfo()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getnameinfo.html) |       |       |   *   |   *   |     √     |
|                                                              | [getnetbyaddr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getnetbyaddr.html) |       |       |   *   |   *   |     √     |
|                                                              | [getnetbyname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getnetbyname.html) |       |       |   *   |   *   |           |
|                                                              | [getnetent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getnetent.html) |       |       |   *   |   *   |           |
|                                                              | [getprotobyname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getprotobyname.html) |       |       |   *   |   *   |           |
|                                                              | [getprotobynumber()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getprotobynumber.html) |       |       |   *   |   *   |           |
|                                                              | [getprotoent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getprotoent.html) |       |       |   *   |   *   |           |
|                                                              | [getservbyname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getservbyname.html) |       |       |   *   |   *   |           |
|                                                              | [getservbyport()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getservbyport.html) |       |       |   *   |   *   |           |
|                                                              | [getservent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getservent.html) |       |       |   *   |   *   |           |
|                                                              | [sethostent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sethostent.html) |       |       |   *   |   *   |           |
|                                                              | [setnetent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setnetent.html) |       |       |   *   |   *   |           |
|                                                              | [setprotoent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setprotoent.html) |       |       |   *   |   *   |           |
|                                                              | [setservent()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setservent.html) |       |       |   *   |   *   |           |
| [<spawn.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/spawn.h.html) |                                                              |       |       |       |       |           |
|                                                              | [posix_spawn()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawn.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawn_file_actions_addclose()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawn_file_actions_addclose.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawn_file_actions_adddup2()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawn_file_actions_adddup2.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawn_file_actions_addopen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawn_file_actions_addopen.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawn_file_actions_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawn_file_actions_destroy.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawn_file_actions_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawn_file_actions_init.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_destroy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_destroy.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_getflags()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_getflags.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_getpgroup()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_getpgroup.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_getschedparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_getschedparam.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_getschedpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_getschedpolicy.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_getsigdefault()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_getsigdefault.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_getsigmask()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_getsigmask.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_init()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_init.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_setflags()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_setflags.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_setpgroup()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_setpgroup.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_setschedparam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_setschedparam.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_setschedpolicy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_setschedpolicy.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_setsigdefault()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_setsigdefault.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnattr_setsigmask()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnattr_setsigmask.html) |       |       |   *   |   *   |           |
|                                                              | [posix_spawnp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/posix_spawnp.html) |       |       |   *   |   *   |           |
| [<sys/select.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sys_select.h.html) |                                                              |       |       |       |       |           |
|                                                              | [FD_CLR()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/FD_CLR.html) |       |       |   *   |   *   |     √     |
|                                                              | [FD_ISSET()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/FD_ISSET.html) |       |       |   *   |   *   |     √     |
|                                                              | [FD_SET()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/FD_SET.html) |       |       |   *   |   *   |     √     |
|                                                              | [FD_ZERO()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/FD_ZERO.html) |       |       |   *   |   *   |     √     |
|                                                              | [select()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/select.html) |       |       |   *   |   *   |     √     |
|                                                              | [pselect()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pselect.html) |       |       |   *   |   *   |           |
| [<sys/socket.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sys_socket.h.html) |                                                              |       |       |       |       |           |
|                                                              | [accept()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/accept.html) |       |       |   *   |   *   |     √     |
|                                                              | [bind()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/bind.html) |       |       |   *   |   *   |     √     |
|                                                              | [connect()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/connect.html) |       |       |   *   |   *   |     √     |
|                                                              | [getpeername()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpeername.html) |       |       |   *   |   *   |     √     |
|                                                              | [getsockname()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getsockname.html) |       |       |   *   |   *   |     √     |
|                                                              | [getsockopt()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getsockopt.html) |       |       |   *   |   *   |     √     |
|                                                              | [listen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/listen.html) |       |       |   *   |   *   |     √     |
|                                                              | [recv()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/recv.html) |       |       |   *   |   *   |     √     |
|                                                              | [recvfrom()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/recvfrom.html) |       |       |   *   |   *   |     √     |
|                                                              | [recvmsg()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/recvmsg.html) |       |       |   *   |   *   |           |
|                                                              | [send()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/send.html) |       |       |   *   |   *   |     √     |
|                                                              | [sendmsg()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sendmsg.html) |       |       |   *   |   *   |           |
|                                                              | [sendto()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sendto.html) |       |       |   *   |   *   |     √     |
|                                                              | [setsockopt()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setsockopt.html) |       |       |   *   |   *   |     √     |
|                                                              | [shutdown()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/shutdown.html) |       |       |   *   |   *   |     √     |
|                                                              | [socket()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/socket.html) |       |       |   *   |   *   |     √     |
|                                                              | [socketpair()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/socketpair.html) |       |       |   *   |   *   |           |
|                                                              | [sockatmark()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/sockatmark.html) |       |       |   *   |   *   |           |
| [<sys/time.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sys_time.h.html) |                                                              |       |       |       |       |           |
|                                                              | [times()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/times.html) |       |       |   *   |   *   |           |
|                                                              | [utimes()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/utimes.html) |       |       |   *   |   *   |           |
| [<sys/wait.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/sys_wait.h.html) |                                                              |       |       |       |       |           |
|                                                              | [wait()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wait.html) |       |       |   *   |   *   |           |
| [<dlfcn.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/dlfcn.h.html) |                                                              |       |       |       |       |           |
|                                                              | [dlclose()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/dlclose.html) |       |       |       |   *   |           |
|                                                              | [dlerror()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/dlerror.html) |       |       |       |   *   |           |
|                                                              | [dlopen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/dlopen.html) |       |       |       |   *   |           |
|                                                              | [dlsym()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/dlsym.html) |       |       |       |   *   |           |
| [<fnmatch.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/fnmatch.h.html) |                                                              |       |       |       |       |           |
|                                                              | [fnmatch()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fnmatch.html) |       |       |       |   *   |           |
| [<glob.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/glob.h.html) |                                                              |       |       |       |       |           |
|                                                              | [glob()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/glob.html) |       |       |       |   *   |           |
|                                                              | [globfree()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/globfree.html) |       |       |       |   *   |           |
| [<grp.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/grp.h.html) |                                                              |       |       |       |       |           |
|                                                              | [getgrgid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getgrgid.html) |       |       |       |   *   |           |
|                                                              | [getgrgid_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getgrgid_r.html) |       |       |       |   *   |           |
|                                                              | [getgrnam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getgrnam.html) |       |       |       |   *   |           |
|                                                              | [getgrnam_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getgrnam_r.html) |       |       |       |   *   |           |
| [<pwd.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/pwd.h.html) |                                                              |       |       |       |       |           |
|                                                              | [getpwnam()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpwnam.html) |       |       |       |   *   |           |
|                                                              | [getpwnam_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpwnam_r.html) |       |       |       |   *   |           |
|                                                              | [getpwuid()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpwuid.html) |       |       |       |   *   |           |
|                                                              | [getpwuid_r()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getpwuid_r.html) |       |       |       |   *   |           |
| [<regex.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/regex.h.html) |                                                              |       |       |       |       |           |
|                                                              | [regcomp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/regcomp.html) |       |       |       |   *   |           |
|                                                              | [regerror()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/regerror.html) |       |       |       |   *   |           |
|                                                              | [regexec()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/regexec.html) |       |       |       |   *   |           |
|                                                              | [regfree()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/regfree.html) |       |       |       |   *   |           |
| [<syslog.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/syslog.h.html) |                                                              |       |       |       |       |           |
|                                                              | [closelog()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/closelog.html) |       |       |       |   *   |           |
|                                                              | [openlog()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/openlog.html) |       |       |       |   *   |           |
|                                                              | [setlogmask()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/setlogmask.html) |       |       |       |   *   |           |
|                                                              | [syslog()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/syslog.html) |       |       |       |   *   |           |
| [<termios.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/termios.h.html) |                                                              |       |       |       |       |           |
|                                                              | [cfgetispeed()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cfgetispeed.html) |       |       |       |   *   |           |
|                                                              | [cfgetospeed()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cfgetospeed.html) |       |       |       |   *   |           |
|                                                              | [cfsetispeed()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cfsetispeed.html) |       |       |       |   *   |           |
|                                                              | [cfsetospeed()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/cfsetospeed.html) |       |       |       |   *   |           |
|                                                              | [tcdrain()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcdrain.html) |       |       |       |   *   |           |
|                                                              | [tcflow()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcflow.html) |       |       |       |   *   |           |
|                                                              | [tcflush()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcflush.html) |       |       |       |   *   |           |
|                                                              | [tcgetattr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcgetattr.html) |       |       |       |   *   |           |
|                                                              | [tcsendbreak()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcsendbreak.html) |       |       |       |   *   |           |
|                                                              | [tcsetattr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/tcsetattr.html) |       |       |       |   *   |           |
| [<wchar.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/wchar.h.html) |                                                              |       |       |       |       |           |
|                                                              | [btowc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/btowc.html) |       |       |       |   *   |           |
|                                                              | [fgetwc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgetwc.html) |       |       |       |   *   |           |
|                                                              | [fgetws()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fgetws.html) |       |       |       |   *   |           |
|                                                              | [fputwc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fputwc.html) |       |       |       |   *   |           |
|                                                              | [fputws()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fputws.html) |       |       |       |   *   |           |
|                                                              | [fwide()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fwide.html) |       |       |       |   *   |           |
|                                                              | [fwprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fwprintf.html) |       |       |       |   *   |           |
|                                                              | [fwscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fwscanf.html) |       |       |       |   *   |           |
|                                                              | [getwc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getwc.html) |       |       |       |   *   |           |
|                                                              | [getwchar()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/getwchar.html) |       |       |       |   *   |           |
|                                                              | [mbrlen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mbrlen.html) |       |       |       |   *   |           |
|                                                              | [mbrtowc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mbrtowc.html) |       |       |       |   *   |           |
|                                                              | [mbsinit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mbsinit.html) |       |       |       |   *   |           |
|                                                              | [mbsrtowcs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/mbsrtowcs.html) |       |       |       |   *   |           |
|                                                              | [putwc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/putwc.html) |       |       |       |   *   |           |
|                                                              | [putwchar()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/putwchar.html) |       |       |       |   *   |           |
|                                                              | [swprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/swprintf.html) |       |       |       |   *   |           |
|                                                              | [swscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/swscanf.html) |       |       |       |   *   |           |
|                                                              | [ungetwc()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/ungetwc.html) |       |       |       |   *   |           |
|                                                              | [vfwprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vfwprintf.html) |       |       |       |   *   |           |
|                                                              | [vfwscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vfwscanf.html) |       |       |       |   *   |           |
|                                                              | [vswprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vswprintf.html) |       |       |       |   *   |           |
|                                                              | [vswscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vswscanf.html) |       |       |       |   *   |           |
|                                                              | [vwprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vwprintf.html) |       |       |       |   *   |           |
|                                                              | [vwscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/vwscanf.html) |       |       |       |   *   |           |
|                                                              | [wcrtomb()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcrtomb.html) |       |       |       |   *   |           |
|                                                              | [wcscat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcscat.html) |       |       |       |   *   |           |
|                                                              | [wcschr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcschr.html) |       |       |       |   *   |           |
|                                                              | [wcscmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcscmp.html) |       |       |       |   *   |           |
|                                                              | [wcscoll()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcscoll.html) |       |       |       |   *   |           |
|                                                              | [wcscpy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcscpy.html) |       |       |       |   *   |           |
|                                                              | [wcscspn()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcscspn.html) |       |       |       |   *   |           |
|                                                              | [wcsftime()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsftime.html) |       |       |       |   *   |           |
|                                                              | [wcslen()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcslen.html) |       |       |       |   *   |           |
|                                                              | [wcsncat()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsncat.html) |       |       |       |   *   |           |
|                                                              | [wcsncmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsncmp.html) |       |       |       |   *   |           |
|                                                              | [wcsncpy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsncpy.html) |       |       |       |   *   |           |
|                                                              | [wcspbrk()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcspbrk.html) |       |       |       |   *   |           |
|                                                              | [wcsrchr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsrchr.html) |       |       |       |   *   |           |
|                                                              | [wcsrtombs()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsrtombs.html) |       |       |       |   *   |           |
|                                                              | [wcsspn()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsspn.html) |       |       |       |   *   |           |
|                                                              | [wcsstr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsstr.html) |       |       |       |   *   |           |
|                                                              | [wcstod()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstod.html) |       |       |       |   *   |           |
|                                                              | [wcstof()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstof.html) |       |       |       |   *   |           |
|                                                              | [wcstok()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstok.html) |       |       |       |   *   |           |
|                                                              | [wcstol()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstol.html) |       |       |       |   *   |           |
|                                                              | [wcstold()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstold.html) |       |       |       |   *   |           |
|                                                              | [wcstoll()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstoll.html) |       |       |       |   *   |           |
|                                                              | [wcstoul()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstoul.html) |       |       |       |   *   |           |
|                                                              | [wcstoull()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcstoull.html) |       |       |       |   *   |           |
|                                                              | [wcsxfrm()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcsxfrm.html) |       |       |       |   *   |           |
|                                                              | [wctob()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wctob.html) |       |       |       |   *   |           |
|                                                              | [wmemchr()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wmemchr.html) |       |       |       |   *   |           |
|                                                              | [wmemcmp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wmemcmp.html) |       |       |       |   *   |           |
|                                                              | [wmemcpy()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wmemcpy.html) |       |       |       |   *   |           |
|                                                              | [wmemmove()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wmemmove.html) |       |       |       |   *   |           |
|                                                              | [wmemset()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wmemset.html) |       |       |       |   *   |           |
|                                                              | [wprintf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wprintf.html) |       |       |       |   *   |           |
|                                                              | [wscanf()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wscanf.html) |       |       |       |   *   |           |
| [<wctype.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/wctype.h.html) |                                                              |       |       |       |       |           |
|                                                              | [iswalnum()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswalnum.html) |       |       |       |   *   |           |
|                                                              | [iswalpha()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswalpha.html) |       |       |       |   *   |           |
|                                                              | [iswblank()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswblank.html) |       |       |       |   *   |           |
|                                                              | [iswcntrl()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswcntrl.html) |       |       |       |   *   |           |
|                                                              | [iswctype()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswctype.html) |       |       |       |   *   |           |
|                                                              | [iswdigit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswdigit.html) |       |       |       |   *   |           |
|                                                              | [iswgraph()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswgraph.html) |       |       |       |   *   |           |
|                                                              | [iswlower()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswlower.html) |       |       |       |   *   |           |
|                                                              | [iswprint()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswprint.html) |       |       |       |   *   |           |
|                                                              | [iswpunct()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswpunct.html) |       |       |       |   *   |           |
|                                                              | [iswspace()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswspace.html) |       |       |       |   *   |           |
|                                                              | [iswupper()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswupper.html) |       |       |       |   *   |           |
|                                                              | [iswxdigit()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/iswxdigit.html) |       |       |       |   *   |           |
|                                                              | [towctrans()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/towctrans.html) |       |       |       |   *   |           |
|                                                              | [towlower()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/towlower.html) |       |       |       |   *   |           |
|                                                              | [towupper()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/towupper.html) |       |       |       |   *   |           |
|                                                              | [wctrans()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wctrans.html) |       |       |       |   *   |           |
|                                                              | [wctype()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wctype.html) |       |       |       |   *   |           |
| [<wordexp.h>](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/wordexp.h.html) |                                                              |       |       |       |       |           |
|                                                              | [wordexp()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wordexp.html) |       |       |       |   *   |           |
|                                                              | [wordfree()](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wordfree.html) |       |       |       |   *   |           |



# Network Framework

With the popularity of the Internet, people's lives are increasingly dependent on the application of the network. More and more products need to connect to the Internet, and device networking has become a trend. To achieve the connection between the device and the network, you need to follow the TCP/IP protocol, you can run the network protocol stack on the device to connect to the network, or you can use devices (chips with hardware network protocol stack interfaces) to connect to the Internet.

When the device is connected to the network, it is like plugging in the wings. You can use the network to upload data in real time. The user can see the current running status and collected data of the device in a hundred thousand miles, and remotely control the device to complete specific tasks. You can also play online music, make online calls, and act as a LAN storage server through your device.

This chapter will explain the related content of the RT-Thread network framework, and introduce you to the concept, function and usage of the network framework. After reading this chapter, you will be familiar with the concept and implementation principle of the RT-Thread network framework and familiar with  network programming using Socket API.

## TCP/IP Introduction to Network Protocols

TCP/IP is short for Transmission Control Protocol/Internet Protocol. It is not a single protocol, but a general term for a protocol family. It includes IP protocol, ICMP protocol, TCP protocol, and http and ftp, pop3, https protocol, etc., which define how electronic devices connect to the Internet and the standards by which data is transferred between them.

### OSI Reference Model

OSI (Open System Interconnect), which is an open system interconnection. Generally referred to as the OSI reference model, it is a network interconnection model studied by the ISO (International Organization for Standardization) in 1985.  The architecture standard defines a seven-layer framework for the network interconnection (physical layer, data link layer, network layer, transport layer, session layer, presentation layer, and application layer), that is, the ISO open system interconnection reference model. The first to third layers belong to the lower three layers of the OSI Reference Model and are responsible for creating links for network communication connections; the fourth to seventh layers are the upper four layers of the OSI reference model and is responsible for end-to-end data communication. The capabilities of each layer are further detailed in this framework to achieve interconnectivity, interoperability, and application portability in an open system environment.

### TCP/IP Reference Model

The TCP/IP communication protocol uses a four-layer hierarchical structure, and each layer calls the network provided by its next layer to fulfill its own needs. The four layers are:

* **Application layer**: Different types of network applications have different communication rules, so the application layer protocols are various, such as Simple Mail Transfer Protocol (SMTP), File Transfer Protocol (FTP), and network remote access protocol (Telnet).
* **Transport layer**: In this layer, it provides data transfer services between nodes, such as Transmission Control Protocol (TCP), User Datagram Protocol (UDP), etc. TCP and UDP add data to the data packet and transmit it to the next layer, this layer is responsible for transmitting data and determining that the data has been delivered and received.
* **Network layer**: responsible for providing basic data packet transfer functions, so that each packet can reach the destination host (but not check whether it is received correctly), such as Internet Protocol (IP).
* **Network interface layer**: Management of actual network media, defining how to use actual networks (such as Ethernet, Serial Line, etc.) to transmit data.

### Difference between TCP/IP Reference Model and OSI Reference Model

The following figure shows the TCP/IP reference model and the OSI reference model diagram:

![TCP/IP Reference Model and OSI Reference Model](figures/net-osi.png)

Both the OSI reference model and the TCP/IP reference model are hierarchical, based on the concept of a separate protocol stack. The OSI reference model has 7 layers, while the TCP/IP reference model has only 4 layers, that is, the TCP/IP reference model has no presentation layer and session layer, and the data link layer and physical layer are merged into a network interface layer. However, there is a certain correspondence between the two layers. Due to the complexity of the OSI system and the design prior to implementation, many designs are too ideal and not very convenient for software implementation. Therefore, there are not many systems that fully implement the OSI reference model, and the scope of application is limited. The TCP/IP reference model was first implemented in a computer system. It has a stable implementation on UNIX and Windows platforms, and provides a simple and convenient programming interface (API) on which a wide range of applications are developed. The TCP/IP reference model has become the international standard and industry standard for Internet connectivity.

### IP Address

The IP address refers to the Internet Protocol Address (also translated as the Internet Protocol Address) and is a uniform address format that assigns a logical address to each network and each host on the Internet to mask physical address differences provided by  Internet Protocol. The common LAN IP address is 192.168.X.X.

### Subnet Mask

Subnet mask (also called netmask, address mask), which is used to indicate which bits of an IP address identify the subnet where the host is located, and which bits are identified as the bit mask of the host. The subnet mask cannot exist alone, it must be used in conjunction with an IP address. Subnet mask has only one effect, which is to divide an IP address into two parts: network address and host address. The subnet mask is the bit of 1, the IP address is the network address, the subnet mask is the bit of 0, and the IP address is the host address. Taking the IP address 192.168.1.10 and the subnet mask 255.255.255.0 as an example, the first 24 bits of the subnet mask (converting decimal to binary) is 1, so the first 24 bits of the IP address 192.168.1 represent the network address. The remaining 0 is the host address.

### MAC Address

MAC (figures Access Control or Medium Access Control) address, which is translated as media access control, or physical address, hardware address, used to define the location of  network devices. In OSI model, the third layer network Layer is responsible for IP address, the second layer data link layer is responsible for the MAC address. A host will have at least one MAC address.

## Introduction to the Network Framework of RT-Thread

In order to support various network protocol stacks, RT-Thread has developed a **SAL** component, the full name of the **Socket abstraction layer**. RT-Thread can seamlessly access various protocol stacks, including several commonly used TCP/IP protocol stack, such as the LwIP protocol stack commonly used in embedded development and the AT Socket protocol stack component developed by RT-Thread, which complete the conversion of data from the network layer to the transport layer.

The main features of the RT-Thread network framework are as follows:

* Support for standard network sockets BSD Socket API, support for poll/select
* Abstract, unified multiple network protocol stack interfaces
* Support various physical network cards, network communication module hardware
* The resource occupancy of SAL socket abstraction layer component is small: ROM 2.8K and RAM 0.6K.

RT-Thread's network framework adopts a layered design with four layers, each layer has different responsibilities. The following figure shows the RT-Thread network framework structure:

![RT-Thread network framework structure](figures/net-layer.png)

The network framework provides a standard BSD Socket interface to user applications. Developers use the BSD Socket interface to operate without worrying about how the underlying network is implemented, and no need to care which network protocol stack the network data passes through. The socket abstraction layer provides the upper application layer. The interfaces are: `accept`, `connect`, `send`, `recv`, etc.

Below the SAL layer is the protocol stack layer. The main protocol stacks supported in the current network framework are as follows:

* **LwIP** is an open source TCP/IP protocol stack implementation that reduces RAM usage while maintaining the main functionality of the TCP/IP protocol, making the LwIP protocol stack ideal for use in embedded systems.
* **AT Socket** is a component for modules that support AT instructions. The AT command uses a standard serial port for data transmission and reception, and converts complex device communication methods into simple serial port programming, which greatly simplifies the hardware design and software development costs of the product, which makes almost all network modules such as GPRS, 3G/4G, NB-IoT, Bluetooth, WiFi, GPS and other modules are very convenient to access the RT-Thread network framework, develop network applications through the standard BSD Socket method, greatly simplifying the development of upper-layer applications.
* **Socket CAN** is a way of programming CAN, it is easy to use and easy to program. By accessing the SAL layer, developers can implement Socket CAN programming on RT-Thread.

Below the protocol stack layer is an abstract device layer that is connected to various network protocol stacks by abstracting hardware devices into Ethernet devices or AT devices.

The bottom layer is a variety of network chips or modules (for example: Ethernet chips with built-in protocol stack such as W5500/CH395, WiFi module with AT command, GPRS module, NB-IoT module, etc.). These hardware modules are the carrier that truly performs the network communication function and is responsible for communicating with various physical networks.

In general, the RT-Thread network framework allows developers to only care about and use the standard BSD Socket network interface for network application development, without concern for the underlying specific network protocol stack type and implementation, greatly improving system compatibility and convenience. Developers have completed the development of network-related applications, and have greatly improved the compatibility of RT-Thread in different areas of the Internet of Things.

In addition, based on the network framework, RT-Thread provides a large number of network software packages, which are various network applications based on the SAL layer, such as **Paho MQTT**, **WebClient**, **cJSON**, **netutils**, etc., which can be obtained from the online package management center. These software packages are web application tools. Using them can greatly simplify the development of network applications and shorten the network application development cycle. At present, there are more than a dozen network software packages. The following table lists some of the network software packages currently supported by RT-Thread, and the number of software packages is still increasing.

| **Package Name** | **Description**                                              |
| ---------------- | ------------------------------------------------------------ |
| Paho MQTT        | Based on Eclipse open source Paho MQTT, it has done a lot of functions and performance optimization, such as: increased automatic reconnection after disconnection, pipe model, support for non-blocking API, support for TLS encrypted transmission, etc. |
| WebClient        | Easy-to-use HTTP client with support for HTTP GET/POST and other common request functions, support for HTTPS, breakpoint retransmission, etc. |
| mongoose         | Embedded Web server network library, similar to Nginx in the embedded world. Licensing is not friendly enough, business needs to be charged |
| WebTerminal      | Access Finsh/MSH Shell packages in the browser or on the mobile |
| cJSON            | Ultra-lightweight JSON parsing library                       |
| ljson            | Json to struct parsing, output library                       |
| ezXML            | XML file parsing library, currently does not support parsing XML data |
| nanopb           | Protocol Buffers format data parsing library, Protocol Buffers format takes up less resources than JSON, XML format resources |
| GAgent           | Software package for accessing Gizwits Cloud Platform        |
| Marvell WiFi     | Marvell WiFi driver                                          |
| Wiced WiFi       | WiFi driver for Wiced interface                              |
| CoAP             | Porting libcoap's CoAP communication package                 |
| nopoll           | Ported open source WebSocket communication package           |
| netutils         | A collection of useful network debugging gadgets, including: ping, TFTP, iperf, NetIO, NTP, Telnet, etc. |
| OneNet           | Software for accessing China Mobile OneNet Cloud             |

## Network Framework Workflow

Using the RT-Thread network framework, you first need to initialize the SAL, then register various network protocol clusters to ensure that the application can communicate using the socket network socket interface. This section mainly uses LwIP as an example.

### Register the Network Protocol Cluster

First use the `sal_init()` interface to initialize resources such as mutex locks used in the component. The interface looks like this:

```c
int sal_init(void);
```

After the SAL is initialized, then use the  the `sal_proto_family_register()` interface to register network protocol cluster, for example, the LwIP network protocol cluster is registered to the SAL. The sample code is as follows:

```c
static const struct proto_family LwIP_inet_family_ops = {
    "LwIP",
    AF_INET,
    AF_INET,
    inet_create,
    LwIP_gethostbyname,
    LwIP_gethostbyname_r,
    LwIP_freeaddrinfo,
    LwIP_getaddrinfo,
};

int LwIP_inet_init(void)
{
    sal_proto_family_register(&LwIP_inet_family_ops);

    return 0;
}
```

`AF_INET` stands for IPv4 address, for example 127.0.0.1; `AF` is short for "Address Family" and `INET` is short for "Internet".

The `sal_proto_family_register()` interface is defined as follows:

```
int sal_proto_family_register(const struct proto_family *pf)；
```

| **Parameters** | **Description**                    |
| -------------- | ---------------------------------- |
| pf             | Protocol cluster structure pointer |
| **Return**     | **——**                             |
| 0              | registration success               |
| -1             | registration failed                |

### Network Data Receiving Process

After the LwIP is registered to the SAL, the application can send and receive network data through the network socket interface. In LwIP, several main threads are created, and they are `tcpip` thread, `erx` receiving thread and `etx` sending thread. The network data receiving process is as shown in the following picture. The application receives data by calling the standard socket interface `recv()` with blocking mode. When the Ethernet hardware device receives the network data packet, it stores the packet in the receiving buffer, and then sends an email to notify the `erx` thread that the data arrives through the Ethernet interrupt program. The `erx` thread applies for the `pbuf` memory block according to the received data length and put the data into the pbuf's `payload` data, then send the `pbuf` memory block to the `tcpip` thread via mailbox, and the `tcpip` thread returns the data to the application that is blocking the receiving data.

![Data receiving function call flow chart](figures/net-recv.png)

### Network Data Sending Process

The network data sending process is shown in the figure below. When there is data to send, the application calls the standard network socket interface `send()` to hand the data to the `tcpip` thread. The `tcpip` thread sends a message to wake up the `etx` thread. The `etx` thread first determines if the Ethernet is sending data. If data is not being sent, it will put the data to be sent into the send buffer, and then send the data through the Ethernet device. If data is being sent, the `etx` thread suspends itself until the Ethernet device is idle before sending the data out.

![Data sending function call flow chart](figures/net-send.png)

## Network Socket Programming

The application uses Socket (network socket) interface programming to implement network communication functions. Socket is a set of application program interface (API), which shields the communication details of each protocol, so that the application does not need to pay attention to the protocol itself, directly using the interfaces provide by socket to communicate between different hosts interconnected.

### TCP socket Communication Process

TCP(Tranfer Control Protocol) is a connection-oriented protocol to ensure reliable data transmission. Through the TCP protocol transmission, a sequential error-free data stream is obtained. The TCP-based socket programming flow diagram is shown in the following figure. A connection must be established between the sender and the receiver's two sockets in order to communicate on the basis of the TCP protocol. When a socket (usually a server socket) waits for a connection to be established. Another socket can request a connection. Once the two sockets are connected, they can perform two-way data transmission, and both sides can send or receive data. A TCP connection is a reliable connection that guarantees that packets arrive in order, and if a packet loss occurs, the packet is automatically resent.

For example, TCP is equivalent to calling in life. When you call the other party, you must wait for the other party to answer. Only when the other party answers your call  can he/she establish a connection with you. The two parties can talk and pass information to each other. Of course, the information passed at this time is reliable, because the other party can't hear what you said and can ask you to repeat the content again. When either party on the call wants to end the call, they will bid farewell to the other party and wait until the other party bids farewell to them before they hang up and end the call.

![TCP-based socket programming flow chart](figures/net-tcp.png)

### UDP socket Communication Process

UDP is short for User Datagram Protocol. It is a connectionless protocol. Each datagram is a separate information, including the complete source address and destination address. It is transmitted to the destination on the network in any possible path. Therefore, whether the destination can be reached, the time to reach the destination, and the correctness of the content cannot be guaranteed. The UDP-based socket programming flow is shown in the following figure.

![UDP-based socket programming flow](figures/net-udp.png)

For example, UDP is equivalent to the walkie-talkie communication in life. After you set up the channel, you can directly say the information you want to express. The data is sent out by the walkie-talkie, but you don't know if your message has been received by others. By the way, unless someone else responds to you with a walkie-talkie. So this method is not reliable.

### Create a Socket

Before communicating, the communicating parties first use the `socket()` interface to create a socket, assigning a socket descriptor and its resources based on the specified address family, data type, and protocol. The interface is as follows:

```c
int socket(int domain, int type, int protocol);
```

| **Parameters** | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| domain         | Protocol family                                              |
| type           | Specify the communication type, including values SOCK_STREAM and SOCK_DGRAM. |
| protocol       | Protocol allows to specify a protocol for the socket, which is set to 0 by default. |
| **Return**     | **——**                                                       |
| >=0            | Successful, returns an integer representing the socket descriptor |
| -1             | Fail                                                         |

**Communication types** include SOCK_STREAM and SOCK_DGRAM.

**SOCK_STREAM** indicates connection-oriented TCP data transfer. Data can arrive at another computer without any errors. If it is damaged or lost, it can be resent, but it is relatively slow.

**SOCK_DGRAM** indicates the connectionless UDP data transfer method. The computer only transmits data and does not perform data verification. If the data is damaged during transmission or does not reach another computer, there is no way to remedy it. In other words, if the data is wrong, it is wrong and cannot be retransmitted. Because SOCK_DGRAM does less validation work, it is more efficient than SOCK_STREAM.

The sample code for creating a TCP type socket is as follows:

```c
 /* Create a socket, type is SOCKET_STREAM，TCP type */
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        /* failed to create socket*/
        rt_kprintf("Socket error\n");

        return;
    }
```

### Binding Socket

A binding socket is used to bind a port number and an IP address to a specified socket. When using socket() to create a socket, only the protocol family is given, and no address is assigned. Before the socket receives a connection from another host, it must bind it with an address and port number using bind(). The interface is as follows:

```c
int bind(int s, const struct sockaddr *name, socklen_t namelen);
```

| **Parameters** | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| S              | Socket descriptor                                            |
| name           | Pointer to the sockaddr structure representing the address to be bound |
| namelen        | Length of sockaddr structure                                 |
| **Return**     | **——**                                                       |
| 0              | Successful                                                   |
| -1             | Fail                                                         |

### Establishing a TCP Connection

For server-side programs, after using `bind()` to bind the socket, you also need to use the `listen()` function to make the socket enter the passive listening state, and then call the `accept()` function to respond to the client at any time.

#### Listening Socket

The listening socket is used by the TCP server to listen for the specified socket connection. The interface is as follows:

```c
int listen(int s, int backlog);
```

| **Parameters** | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| s              | Socket descriptor                                            |
| backlog        | Indicates the maximum number of connections that can be waited at a time |
| **Return**     | **——**                                                       |
| 0              | Successful                                                   |
| -1             | Fail                                                         |

#### Accept the Connection

When the application listens for connections from other clients, the connection must be initialized with the `accept()` function, which creates a new socket for each connection and removes the connection from the listen queue. The interface is as follows:

```c
int accept(int s, struct sockaddr *addr, socklen_t *addrlen);
```

| **Parameters** | **Description**                                        |
| -------------- | ------------------------------------------------------ |
| s              | Socket descriptor                                      |
| addr           | Client device address information                      |
| addrlen        | Client device address structure length                 |
| **Return**     | **——**                                                 |
| >=0            | Successful, return the newly created socket descriptor |
| -1             | Fail                                                   |

#### Establish Connection

Used by the client to establish a connection with the specified server. The interface is as follows:

```
int connect(int s, const struct sockaddr *name, socklen_t namelen);
```

| **Parameters** | **Description**                 |
| -------------- | ------------------------------- |
| s              | Socket descriptor               |
| name           | Server address information      |
| namelen        | Server address structure length |
| **Return**     | **——**                          |
| 0              | Successful                      |
| -1             | Fail                            |

When the client connects to the server, first set the server address and then use the `connect()` function to connect. The sample code is as follows:

```c
struct sockaddr_in server_addr;
/* Initialize the pre-connected server address */
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(port);
server_addr.sin_addr = *((struct in_addr *)host->h_addr);
rt_memset(&(server_addr.sin_zero), 0, sizeof(server_addr.sin_zero));

/* Connect to the server */
if (connect(sock, (struct sockaddr *)&server_addr, sizeof(struct sockaddr)) == -1)
{
    /* Connection failed */
    closesocket(sock);

    return;
}
```

### Data Transmission

TCP and UDP have different data transmission methods. TCP needs to establish a connection before data transmission, use `send()` function for data transmission, use `recv()` function for data reception, and UDP does not need to establish connection. It uses `sendto()` function sends data and receives data using the `recvfrom()` function.

#### TCP Data Transmission

After the TCP connection is established, the data is sent using the `send()` function. The interface is as follows:

```c
int send(int s, const void *dataptr, size_t size, int flags);
```

| **Parameters** | **Description**                                |
| -------------- | ---------------------------------------------- |
| s              | Socket descriptor                              |
| dataptr        | The data pointer to send                       |
| size           | Length of data sent                            |
| flags          | Flag, generally 0                              |
| **Return**     | **——**                                         |
| >0             | Successful, return the length of the sent data |
| <=0            | Failed                                         |

#### TCP Data Reception

After the TCP connection is established, use `recv()` to receive the data. The interface is as follows:

```c
int recv(int s, void *mem, size_t len, int flags);
```

| **Parameters** | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| s              | Socket descriptor                                            |
| mem            | Received data pointer                                        |
| len            | Received data length                                         |
| flags          | Flag, generally 0                                            |
| **Return**     | **Description**                                              |
| >0             | Successful, return the length of the received data           |
| =0             | The destination address has been transferred and the connection is closed |
| <0             | Fail                                                         |

#### UDP Data transmission

In the case where a connection is not established, you can use the `sendto()` function to send UDP data to the specified destination address, as shown below:

```c
int sendto(int s, const void *dataptr, size_t size, int flags,
           const struct sockaddr *to, socklen_t tolen);
```

| **Parameters** | **Description**                                |
| -------------- | ---------------------------------------------- |
| s              | Socket descriptor                              |
| dataptr        | Data pointer sent                              |
| size           | Length of data sent                            |
| flags          | Flag, generally 0                              |
| to             | Target address structure pointer               |
| tolen          | Target address structure length                |
| **Return**     | **——**                                         |
| >0             | Successful, return the length of the sent data |
| <=0            | Fail                                           |

#### UDP Data Reception

To receive UDP data, use the `recvfrom()` function, and the interface is:

```c
int recvfrom(int s, void *mem, size_t len, int flags,
             struct sockaddr *from, socklen_t *fromlen);
```

| **Parameters** | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| s              | Socket descriptor                                            |
| mem            | Received data pointer                                        |
| len            | Received data length                                         |
| flags          | Flag, generally 0                                            |
| from           | Receive address structure pointer                            |
| fromlen        | Receive address structure length                             |
| **Return**     | **——**                                                       |
| >0             | Successful, return the length of the received data           |
| 0              | The receiving address has been transferred and the connection is closed |
| <0             | Fail                                                         |

### Close Network Connection

After the network communication is over, you need to close the network connection. There are two ways to use `closesocket()` and `shutdown()`.

The `closesocket()` interface is used to close an existing socket connection, release the socket resource, clear the socket descriptor from memory, and then the socket could not be used again. The connection and cache associated with the socket are also lost the meaning, the TCP protocol will automatically close the connection. The interface is as follows:

```c
int closesocket(int s);
```

| **Parameters** | **Description**   |
| -------------- | ----------------- |
| s              | Socket descriptor |
| **Return**     | **——**            |
| 0              | Successful        |
| -1             | Fail              |

Network connections can also be turned off using the `shutdown()` function. The TCP connection is full-duplex. You can use the `shutdown()` function to implement a half-close. It can close the read or write operation of the connection, or both ends, but it does not release the socket resource. The interface is as follows:

```c
int shutdown(int s, int how);
```

| **Parameters** | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| s              | Socket descriptor                                            |
| how            | SHUT_RD closes the receiving end of the connection and no longer receives data. <br />SHUT_WR closes the connected sender and no longer sends data. <br />SHUT_RDWR is closed at both ends. |
| **Return**     | **——**                                                       |
| 0              | Successful                                                   |
| -1             | Fail                                                         |

## Network Function Configuration

The main functional configuration options of the network framework are shown in the following table, which can be configured according to different functional requirements:

SAL component configuration options:

| **Macro Definition**   | **Value Type** | **Description**                                   |
| ---------------------- | -------------- | ------------------------------------------------- |
| RT_USING_SAL           | Boolean        | Enable SAL                                        |
| SAL_USING_LWIP         | Boolean        | Enable LwIP component                             |
| SAL_USING_AT           | Boolean        | Enable the AT component                           |
| SAL_USING_POSIX        | Boolean        | Enable POSIX interface                            |
| SAL_PROTO_FAMILIES_NUM | Integer        | the maximum number of protocol families supported |

LwIP Configuration options:

| **Macro Definition**        | **Value Type** | **Description**                              |
| --------------------------- | -------------- | -------------------------------------------- |
| RT_USING_LWIP               | Boolean        | Enable  LwIP protocol                        |
| RT_USING_LWIP_IPV6          | Boolean        | Enable IPV6 protocol                         |
| RT_LWIP_IGMP                | Boolean        | Enable the IGMP protocol                     |
| RT_LWIP_ICMP                | Boolean        | Enable the ICMP protocol                     |
| RT_LWIP_SNMP                | Boolean        | Enable the SNMP protocol                     |
| RT_LWIP_DNS                 | Boolean        | Enable DHCP function                         |
| RT_LWIP_DHCP                | Boolean        | Enable DHCP function                         |
| IP_SOF_BROADCAST            | Integer        | filtering roadcasting Packets Sended by IP   |
| IP_SOF_BROADCAST_RECV       | Integer        | filtering roadcasting Packets received by IP |
| RT_LWIP_IPADDR              | String         | IP address                                   |
| RT_LWIP_GWADDR              | String         | Gateway address                              |
| RT_LWIP_MSKADDR             | String         | Subnet mask                                  |
| RT_LWIP_UDP                 | Boolean        | Enable UDP protocol                          |
| RT_LWIP_TCP                 | Boolean        | Enable TCP protocol                          |
| RT_LWIP_RAW                 | Boolean        | Enable RAW API                               |
| RT_MEMP_NUM_NETCONN         | Integer        | Support Numbers of network links             |
| RT_LWIP_PBUF_NUM            | Integer        | pbuf number of memory blocks                 |
| RT_LWIP_RAW_PCB_NUM         | Integer        | Maximum number of connections for RAW        |
| RT_LWIP_UDP_PCB_NUM         | Integer        | Maximum number of connections for UDP        |
| RT_LWIP_TCP_PCB_NUM         | Integer        | Maximum number of connections for TCP        |
| RT_LWIP_TCP_SND_BUF         | Integer        | TCP send buffer size                         |
| RT_LWIP_TCP_WND             | Integer        | TCP sliding window size                      |
| RT_LWIP_TCPTHREAD_PRIORITY  | Integer        | TCP thread priority                          |
| RT_LWIP_TCPTHREAD_MBOX_SIZE | Integer        | TCP thread mailbox size                      |
| RT_LWIP_TCPTHREAD_STACKSIZE | Integer        | TCP thread stack size                        |
| RT_LWIP_ETHTHREAD_PRIORITY  | Integer        | Receive/send thread's priority               |
| RT_LWIP_ETHTHREAD_STACKSIZE | Integer        | Receive/send thread's stack size             |
| RT_LwIP_ETHTHREAD_MBOX_SIZE | Integer        | Receive/send thread's mailbox size           |

## Network Application Example

### View IP Address

In the console, you can use the ifconfig command to check the network status. The IP address is 192.168.12.26, and the FLAGS status is LINK_UP, indicating that the network is configured:

```c
msh >ifconfig
network interface: e0 (Default)
MTU: 1500
MAC: 00 04 a3 12 34 56
FLAGS: UP LINK_UP ETHARP BROADCAST IGMP
ip address: 192.168.12.26
gw address: 192.168.10.1
net mask  : 255.255.0.0·
dns server #0: 192.168.10.1
dns server #1: 223.5.5.5
```

### Ping Network Test

Use the ping command for network testing:

```c
msh />ping rt-thread.org
60 bytes from 116.62.244.242 icmp_seq=0 ttl=49 time=11 ticks
60 bytes from 116.62.244.242 icmp_seq=1 ttl=49 time=10 ticks
60 bytes from 116.62.244.242 icmp_seq=2 ttl=49 time=12 ticks
60 bytes from 116.62.244.242 icmp_seq=3 ttl=49 time=10 ticks
msh />ping 192.168.10.12
60 bytes from 192.168.10.12 icmp_seq=0 ttl=64 time=5 ticks
60 bytes from 192.168.10.12 icmp_seq=1 ttl=64 time=1 ticks
60 bytes from 192.168.10.12 icmp_seq=2 ttl=64 time=2 ticks
60 bytes from 192.168.10.12 icmp_seq=3 ttl=64 time=3 ticks
msh />
```

Getting the above output indicates that the connection network is successful!

### TCP Client Example

After the network is successfully connected, you can run the network example, first run the TCP client example. This example will open a TCP server on the PC, open a TCP client on the IoT Board, and both parties will communicate on the network.

In the example project, there is already a TCP client program `tcpclient_sample.c`. The function is to implement a TCP client that can receive and display the information sent from the server. If it receives the information starting with 'q' or 'Q', then exit the program directly and close the TCP client. The program exports the tcpclient command to the FinSH console. The command  format is `tcpclient URL PORT`, where URL is the server address and PORT is the port number. The sample code is as follows:

```c
/*
 * Program list: tcp client
 *
 * This is a tcp client routine
 * Export the tcpclient command to MSH
 * Command call format: tcpclient URL PORT
 * URL: server address PORT:: port number
 * Program function: Receive and display the information sent from the server, and receive the information that starts with 'q' or 'Q' to exit the program.
*/

#include <rtthread.h>
#include <sys/socket.h> /* To use BSD socket, you need to include the socket.h header file. */
#include <netdb.h>
#include <string.h>
#include <finsh.h>

#define BUFSZ   1024

static const char send_data[] = "This is TCP Client from RT-Thread."; /* Sending used data */
void tcpclient(int argc, char**argv)
{
    int ret;
    char *recv_data;
    struct hostent *host;
    int sock, bytes_received;
    struct sockaddr_in server_addr;
    const char *url;
    int port;

    /* Received less than 3 parameters */
    if (argc < 3)
    {
        rt_kprintf("Usage: tcpclient URL PORT\n");
        rt_kprintf("Like: tcpclient 192.168.12.44 5000\n");
        return ;
    }

    url = argv[1];
    port = strtoul(argv[2], 0, 10);

    /* Get the host address through the function entry parameter url (if it is a domain name, it will do domain name resolution) */
    host = gethostbyname(url);

    /* Allocate buffers for storing received data */
    recv_data = rt_malloc(BUFSZ);
    if (recv_data == RT_NULL)
    {
        rt_kprintf("No memory\n");
        return;
    }

    /* Create a socket of type SOCKET_STREAM, TCP type */
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        /* Failed to create socket */
        rt_kprintf("Socket error\n");

        /* Release receive buffer */
        rt_free(recv_data);
        return;
    }

    /* Initialize the pre-connected server address */
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(port);
    server_addr.sin_addr = *((struct in_addr *)host->h_addr);
    rt_memset(&(server_addr.sin_zero), 0, sizeof(server_addr.sin_zero));

    /* Connect to the server */
    if (connect(sock, (struct sockaddr *)&server_addr, sizeof(struct sockaddr)) == -1)
    {
        /* Connection failed */
        rt_kprintf("Connect fail!\n");
        closesocket(sock);

        /* Release receive buffer */
        rt_free(recv_data);
        return;
    }

    while (1)
    {
        /* Receive maximum BUFSZ-1 byte data from a sock connection */
        bytes_received = recv(sock, recv_data, BUFSZ - 1, 0);
        if (bytes_received < 0)
        {
            /* Receive failed, close this connection */
            closesocket(sock);
            rt_kprintf("\nreceived error,close the socket.\r\n");

            /* Release receive buffer */
            rt_free(recv_data);
            break;
        }
        else if (bytes_received == 0)
        {
            /* Print the recv function returns a warning message with a value of 0 */
            rt_kprintf("\nReceived warning,recv function return 0.\r\n");

            continue;
        }

        /* Received data, clear the end */
        recv_data[bytes_received] = '\0';

        if (strncmp(recv_data, "q", 1) == 0 || strncmp(recv_data, "Q", 1) == 0)
        {
            /* If the initial letter is q or Q, close this connection */
            closesocket(sock);
            rt_kprintf("\n got a'q'or'Q',close the socket.\r\n");

            /* Release receive buffer */
            rt_free(recv_data);
            break;
        }
        else
        {
            /* Display the received data at the control terminal */
            rt_kprintf("\nReceived data = %s", recv_data);
        }

        /* Send data to sock connection */
        ret = send(sock, send_data, strlen(send_data), 0);
        if (ret < 0)
        {
            /* Receive failed, close this connection */
            closesocket(sock);
            rt_kprintf("\nsend error,close the socket.\r\n");

            rt_free(recv_data);
            break;
        }
        else if (ret == 0)
        {
            /* Print the send function returns a warning message with a value of 0 */
            rt_kprintf("\n Send warning,send function return 0.\r\n");
        }
    }
    return;
}
MSH_CMD_EXPORT(tcpclient, a tcp client sample);
```

When running the example, first open a network debugging assistant on your computer and open a TCP server. Select the protocol type as TCP
Server, fill in the local IP address and port 5000, as shown below.

![Network debugging tool interface](figures/net-tcp-s.png)

Then start the TCP client to connect to the TCP server by entering the following command in the FinSH console:

```C
msh />tcpclient 192.168.12.45 5000  // Input according to actual situation
Connect successful
```

When the console outputs the log message "Connect successful", it indicates that the TCP connection was successfully established. Next, you can perform data communication. In the network debugging tool window, send Hello RT-Thread!, which means that a data is sent from the TCP server to the TCP client, as shown in the following figure:

![网络调试工具界面](figures/net-hello.png)

After receiving the data on the FinSH console, the corresponding log information will be output, you can see:

```c
msh >tcpclient 192.168.12.130 5000
Connect successful
Received data = hello world
Received data = hello world
Received data = hello world
Received data = hello world
Received data = hello world
 got a 'q' or 'Q',close the socket.
msh >
```

The above information indicates that the TCP client received 5 "hello world" data sent from the server. Finally, the exit command 'q' was received from the TCP server, and the TCP client program exited the operation and returned to the FinSH console.

### UDP Client Example

This is an example of a UDP client. This example will open a UDP server on the PC and open a UDP client on the IoT Board for network communication. A UDP client program has been implemented in the sample project. The function is to send data to the server. The sample code is as follows:

```c
/*
 * Program list: udp client
 *
 * This is a udp client routine
 * Export the udpclient command to the msh
 * Command call format: udpclient URL PORT [COUNT = 10]
 * URL: Server Address PORT: Port Number COUNT: Optional Parameter Default is 10
 * Program function: send COUNT datas to the remote end of the service
*/

#include <rtthread.h>
#include <sys/socket.h> /* To use BSD socket, you need to include the sockets.h header file. */
#include <netdb.h>
#include <string.h>
#include <finsh.h>

const char send_data[] = "This is UDP Client from RT-Thread.\n"; /* data */

void udpclient(int argc, char**argv)
{
    int sock, port, count;
    struct hostent *host;
    struct sockaddr_in server_addr;
    const char *url;

    /* Received less than 3 parameters */
    if (argc < 3)
    {
        rt_kprintf("Usage: udpclient URL PORT [COUNT = 10]\n");
        rt_kprintf("Like: udpclient 192.168.12.44 5000\n");
        return ;
    }

    url = argv[1];
    port = strtoul(argv[2], 0, 10);

    if (argc> 3)
        count = strtoul(argv[3], 0, 10);
    else
        count = 10;

    /* Get the host address through the function entry parameter url (if it is a domain name, it will do domain name resolution) */
    host = (struct hostent *) gethostbyname(url);

    /* Create a socket of type SOCK_DGRAM, UDP type */
    if ((sock = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {
        rt_kprintf("Socket error\n");
        return;
    }

    /* Initialize the pre-connected server address */
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(port);
    server_addr.sin_addr = *((struct in_addr *)host->h_addr);
    rt_memset(&(server_addr.sin_zero), 0, sizeof(server_addr.sin_zero));

    /* Send count data in total */
    while (count)
    {
        /* Send data to the remote end of the service */
        sendto(sock, send_data, strlen(send_data), 0,
               (struct sockaddr *)&server_addr, sizeof(struct sockaddr));

        /* Thread sleep for a while */
        rt_thread_delay(50);

        /* Count value minus one */
        count --;
    }

    /* Turn off this socket */
    closesocket(sock);
}
```

When running the example, first open a network debugging assistant on your computer and open a UDP server. Select the protocol type as UDP and fill in the local IP address and port 5000, as shown in the figure below.

![网络调试工具界面](figures/net-udp-server.png)

Then you can enter the following command in the FinSH console to send data to the UDP server.

```c
msh />udpclient 192.168.12.45 1001          // Need to enter according to the real situation
```

The server will receive 10 messages from This is UDP Client from RT-Thread., as shown below:

![网络调试工具界面](figures/net-udp-client.png)



# FinSH Console

In the early days of computer development, before the advent of graphics systems, there was no mouse or even a keyboard. How did people interact with computers at the time? The earliest computers used a punched note to enter commands into the computer and write the program. Later, with the continuous development of computers, monitors and keyboards became the standard configuration of computers, but the operating system at this time did not support the graphical interface. Computer pioneers developed a software that accepts commands entered by the user, and after interpretation, passes it to The operating system and return the results of the operating system execution to the user. This program wraps around the operating system like a layer of shell, so it's called a shell.

Embedded devices usually need to connect the development board to the PC for communication. Common connections include: serial port, USB, Ethernet, Wi-Fi, etc. A flexible shell should also support working on multiple connection methods. With the shell, the developer can easily get the system running and control the operation of the system through commands. Especially in the debugging phase, with the shell, in addition to being able to locate the problem more quickly, the developer can also use the shell to call the test function, change the parameters of the test function, reduce the number of times the code is downloaded, and shorten the development time of the project.

FinSH is the command line component (shell) of RT-Thread. It is based on the above considerations. FinSH is pronounced [ˈfɪnʃ]. After reading this chapter, we will have a deeper understanding of how FinSH works and how to export your own commands to FinSH.

## Introduction of FinSH

FinSH is the command line component of RT-Thread. It provides a set of operation interfaces for users to call from the command line. It is mainly used to debug or view system information. It can communicate with a PC using serial/Ethernet/USB, etc. The hardware topology is shown below:

![FinSH Hardware connection diagram](figures/finsh-hd.png)

The user inputs a command in the control terminal, and the control terminal transmits the command to the FinSH in the device through the serial port, USB, network, etc., FinSH will read the device input command, parse and automatically scan the internal function table, find the corresponding function name, and execute the function. The response is output, the response is returned through the original path, and the result is displayed on the control terminal.

When using a serial port to connect a device to a control terminal, the execution flow of the FinSH command is as follows:

![FinSH FinSH Command execution flow chart](figures/finsh-run.png)

FinSH supports the rights verification function. After the system is started, the system will perform the rights verification. Only when the rights verification is passed, the FinSH function will be enabled. This improves the security of system input.

FinSH supports auto-completion, and viewing history commands, etc. These functions can be easily accessed through the keys on the keyboard. The keys supported by FinSH are shown in the following table:

| Keys          | **Functional Description**                                   |
| ------------- | ------------------------------------------------------------ |
| Tab key       | Pressing the Tab key when no characters are entered will print all commands supported by the current system. If you press the Tab key when you have entered some characters, it will find the matching command, and will also complete the file name according to the file system's current directory, and you can continue to input, multiple completions. |
| ↑↓ key        | Scroll up and down the recently entered history command      |
| Backspace key | Delete character                                             |
| ←→ key        | Move the cursor left or right                                |

FinSH supports two input modes, the traditional command line mode and the C language interpreter mode.

### Traditional Command Line Mode

This mode is also known as msh(module shell). In msh mode, FinSH is implemented in the same way as the traditional shell (dos/bash). For example, you can switch directories to the root directory with the `cd /` command.

MSH can parse commands into parameters and parameters separated by spaces. Its command execution format is as follows:

```
command [arg1] [arg2] [...]
```

The command can be either a built-in command in RT-Thread or an executable file.

### C Language Interpreter Mode

This mode is also known as C-Style mode. In C language interpreter mode, FinSH can solve and parse most C language expressions, and use functions like C to access functions and global variables in the system. In addition, it can create variables through the command line. In this mode, the command entered must be similar to the function call in C language, that is, you must carry the `()` symbol. For example, to output all current threads and their status in the system, type `list_thread()` in FinSH to print out the required information. The output of the FinSH command is the return value of this function. For some functions that do not have a return value (void return value), this printout has no meaning.

Initially FinSH only supported C-Style mode. Later, with the development of RT-Thread, C-Style mode is not convenient when running scripts or programs, and it is more convenient to use traditional shell method. In addition, in C-Style mode, FinSH takes up a lot of volume. For these reasons, the msh mode has been added to RT-Thread. The msh mode is small and easy to use. It is recommended that you use the msh mode.

If both modes are enabled in the RT-Thread, they can be dynamically switched. Enter the `exit` in msh mode and press `Enter` to switch to C-Style mode. Enter `msh()` in C-Style mode and press `Enter` to enter msh mode. The commands of the two modes are not common, and the msh command cannot be used in C-Style mode, and vice versa.

## FinSH Built-in Commands

Some FinSH commands are built in by default in RT-Thread. You can print all commands supported by the current system by entering help in FinSH and pressing Enter or directly pressing Tab. The built-in commands in C-Style and msh mode are basically the same, so msh is taken as an example here.

In msh mode, you can list all currently supported commands by pressing the Tab key. The number of default commands is not fixed, and the various components of RT-Thread will output some commands to FinSH. For example, when the DFS component is opened, commands such as `ls`, `cp`, and `cd` are added to FinSH for developers to debug.

The following are all currently supported commands that display RT-Thread kernel status information after pressing the Tab key. The command name is on the left and the description of the command on the right:

```c
RT-Thread shell commands:
version         - show RT-Thread version information
list_thread     - list thread
list_sem        - list semaphore in system
list_event      - list event in system
list_mutex      - list mutex in system
list_mailbox    - list mail box in system
list_msgqueue   - list message queue in system
list_timer      - list timer in system
list_device     - list device in system
exit            - return to RT-Thread shell mode.
help            - RT-Thread shell help.
ps              - List threads in the system.
time            - Execute command with time.
free            - Show the memory usage in the system.
```

Here lists the field information returned after entering the common commands, so that the developer can understand the content of the returned information.

### Display Thread Status

Use the `ps` or `list_thread` command to list all thread information in the system, including thread priority, state, maximum stack usage, and more.

```c
msh />list_thread
thread   pri  status      sp     stack size max used left tick  error
-------- ---  ------- ---------- ----------  ------  ---------- ---
tshell    20  ready   0x00000118 0x00001000    29%   0x00000009 000
tidle     31  ready   0x0000005c 0x00000200    28%   0x00000005 000
timer      4  suspend 0x00000078 0x00000400    11%   0x00000009 000
```

list_thread Return field description:

| **Field**  | **Description**                                   |
| ---------- | ------------------------------------------------- |
| thread     | Thread name                                       |
| pri        | Thread priority                                   |
| status     | The current state of the thread                   |
| sp         | The current stack position of the thread          |
| stack size | Thread stack size                                 |
| max used   | The maximum stack position used in thread history |
| left tick  | The number of remaining ticks of the thread       |
| error      | Thread error code                                 |

### Display Semaphore Status

Use the `list_sem` command to display all semaphore information in the system, including the name of the semaphore, the value of the semaphore, and the number of threads waiting for this semaphore.

```c
msh />list_sem
semaphore v   suspend thread
-------- --- --------------
shrx     000 0
e0       000 0
```

list_sem  Return field description:

| **Field**      | **Description**                                  |
| -------------- | ------------------------------------------------ |
| semaphore      | Semaphore name                                   |
| v              | The current value of semaphore                   |
| suspend thread | The number of threads waiting for this semaphore |

### Display Event Status

Use the `list_event` command to display all event information in the system, including the event name, the value of the event, and the number of threads waiting for this event.

```c
msh />list_event
event      set    suspend thread
-----  ---------- --------------
```

list_event Return field description:

| Field          | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| event          | Event set name                                               |
| set            | The current event in the event set                           |
| suspend thread | The number of threads waiting for an event in this event set |

### Display Mutex Status

Use the `list_mutex` command to display all mutex information in the system, including the mutex name, the owner of the mutex, and the number of nestings the owner holds on the mutex.

```c
msh />list_mutex
mutex    owner    hold suspend thread
-------- -------- ---- --------------
fat0     (NULL)   0000 0
sal_lock (NULL)   0000 0
```

list_mutex Return field description:

| **Field**      | **Description**                                        |
| -------------- | ------------------------------------------------------ |
| mutxe          | Mutex name                                             |
| owner          | The thread currently holding the mutex                 |
| hold           | The number of times the holder is nested on this mutex |
| suspend thread | The number of threads waiting for this mutex           |

### Display Mailbox Status

Use the `list_mailbox` command to display all mailbox information in the system, including the mailbox name, the number of messages in the mailbox, and the maximum number of messages the mailbox can hold.

```c
msh />list_mailbox
mailbox  entry size suspend thread
-------- ----  ---- --------------
etxmb    0000  0008 1:etx
erxmb    0000  0008 1:erx
```

list_mailbox Return field description:

| Field          | **Description**                                   |
| -------------- | ------------------------------------------------- |
| mailbox        | Mailbox name                                      |
| entry          | The number of messages included in the mailbox    |
| size           | The maximum number of messages a mailbox can hold |
| suspend thread | The number of threads waiting for this mailbox    |

### Display Message Queue Status

Use the `list_msgqueue` command to display all message queue information in the system, including the name of the message queue, the number of messages it contains, and the number of threads waiting for this message queue.

```c
msh />list_msgqueue
msgqueue entry suspend thread
-------- ----  --------------
```

list_msgqueue Return field description:

| Field          | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| msgqueue       | Message queue name                                           |
| entry          | The number of messages currently included in the message queue |
| suspend thread | Number of threads waiting for this message queue             |

### Display Memory Pool Status

Use the `list_mempool` command to display all the memory pool information in the system, including the name of the memory pool, the size of the memory pool, and the maximum memory size used.

```c
msh />list_mempool
mempool block total free suspend thread
------- ----  ----  ---- --------------
signal  0012  0032  0032 0
```

list_mempool Return field description:

| Field          | **Description**                                    |
| -------------- | -------------------------------------------------- |
| mempool        | Memory pool name                                   |
| block          | Memory block size                                  |
| total          | Total memory block                                 |
| free           | Free memory block                                  |
| suspend thread | The number of threads waiting for this memory pool |

### Display Timer Status

Use the `list_timer` command to display all the timer information in the system, including the name of the timer, whether it is the periodic timer, and the number of beats of the timer timeout.

```c
msh />list_timer
timer     periodic   timeout       flag
-------- ---------- ---------- -----------
tshell   0x00000000 0x00000000 deactivated
tidle    0x00000000 0x00000000 deactivated
timer    0x00000000 0x00000000 deactivated
```

list_timer Return field description:

| Field    | **Description**                                              |
| -------- | ------------------------------------------------------------ |
| timer    | Timer name                                                   |
| periodic | Whether the timer is periodic                                |
| timeout  | The number of beats when the timer expires                   |
| flag     | The state of the timer, activated indicates active, and deactivated indicates inactive |

### Display Device Status

Use the `list_device` command to display all device information in the system, including the device name, device type, and the number of times the device was opened.

```c
msh />list_device
device         type      ref count
------ ----------------- ----------
e0     Network Interface 0
uart0  Character Device  2
```

list_device Return field description:

| Field     | Description                               |
| --------- | ----------------------------------------- |
| device    | Device name                               |
| type      | Device type                               |
| ref count | The number of times the device was opened |

### Display Dynamic Memory Status

Use the `free` command to display all memory information in the system.

```c
msh />free
total memory: 7669836
used memory : 15240
maximum allocated memory: 18520
```

free Return field description:

| Field                    | Description              |
| ------------------------ | ------------------------ |
| total memory             | Total memory size        |
| used memory              | Used memory size         |
| maximum allocated memory | Maximum allocated memory |

## Custom FinSH Command

In addition to the commands that come with FinSH, FinSH also provides multiple macro interfaces to export custom commands. The exported commands can be executed directly in FinSH.

### Custom msh Command

The custom msh command can be run in msh mode. To export a command to msh mode, you can use the following macro interface：

```
MSH_CMD_EXPORT(name, desc);
```

| **Parameter** | **Description**                   |
| ------------- | --------------------------------- |
| name          | The command to export             |
| desc          | Description of the export command |

This command can export commands with parameters, or export commands without parameters. When exporting a parameterless command, the input parameter of the function is void. The example is as follows：

```c
void hello(void)
{
    rt_kprintf("hello RT-Thread!\n");
}

MSH_CMD_EXPORT(hello , say hello to RT-Thread);
```

When exporting a command with parameters, the function's input parameters are `int argc` and `char**argv`. Argc represents the number of arguments, and argv represents a pointer to a command-line argument string pointer array. An example of exporting a parameter command is as follows:

```c
static void atcmd(int argc, char**argv)
{
    ……
}

MSH_CMD_EXPORT(atcmd, atcmd sample: atcmd <server|client>);
```

### Custom C-Style Commands and Variables

Export custom commands to C-Style mode can use the following interface：

```
FINSH_FUNCTION_EXPORT(name, desc);
```

| **Parameter** | **Description**                   |
| ------------- | --------------------------------- |
| name          | The command to export             |
| desc          | Description of the export command |

The following example defines a `hello` function and exports it as a command in C-Style mode：

```c
void hello(void)
{
    rt_kprintf("hello RT-Thread!\n");
}

FINSH_FUNCTION_EXPORT(hello , say hello to RT-Thread);
```

In a similar way, you can also export a variable that can be accessed through the following interface：

```
FINSH_VAR_EXPORT(name, type, desc);
```

| Parameter | **Description**                      |
| --------- | ------------------------------------ |
| name      | The variable to be exported          |
| type      | Type of variable                     |
| desc      | Description of the exported variable |

The following example defines a `dummy` variable and exports it to a variable command in C-Style mode.：

```c
static int dummy = 0;
FINSH_VAR_EXPORT(dummy, finsh_type_int, dummy variable for finsh)
```

### Custom Command Rename

The function name length of FinSH is limited. It is controlled by the macro definition `FINSH_NAME_MAX` in `finsh.h`. The default is 16 bytes, which means that the FinSH command will not exceed 16 bytes in length. There is a potential problem here: when a function name is longer than FINSH_NAME_MAX, after using FINSH_FUNCTION_EXPORT to export the function to the command table, the full function name is seen in the FinSH symbol table, but a full node execution will result in a *null node* error. This is because although the full function name is displayed, in fact FinSH saves the first 16 bytes as a command. Too many inputs will result in the command not being found correctly. In this case, you can use `FINSH_FUNCTION_EXPORT_ALIAS` to re-export the command name.

```
FINSH_FUNCTION_EXPORT_ALIAS(name, alias, desc);
```

| Parameter | Description                                        |
| --------- | -------------------------------------------------- |
| name      | The command to export                              |
| alias     | The name that is displayed when exporting to FinSH |
| desc      | Description of the export command                  |

The command can be exported to msh mode by adding `__cmd_` to the renamed command name. Otherwise, the command will be exported to C-Style mode. The following example defines a `hello` function and renames it to `ho` and exports it to a command in C-Style mode.

```c
void hello(void)
{
    rt_kprintf("hello RT-Thread!\n");
}

FINSH_FUNCTION_EXPORT_ALIAS(hello , ho, say hello to RT-Thread);
```

## FinSH Function Configuration

The FinSH function can be cropped, and the macro configuration options are defined in the rtconfig.h file. The specific configuration items are shown in the following table.

| **Macro Definition**            | **Value Type** | Description                                                | Default  |
| ------------------------------- | -------------- | ---------------------------------------------------------- | -------- |
| #define RT_USING_FINSH          | None           | Enable FinSH                                               | on       |
| #define FINSH_THREAD_NAME       | String         | FinSH thread name                                          | "tshell" |
| #define FINSH_USING_HISTORY     | None           | Turn on historical traceback                               | on       |
| #define FINSH_HISTORY_LINES     | Integer type   | Number of historical command lines that can be traced back | 5        |
| #define FINSH_USING_SYMTAB      | None           | Symbol table can be used in FinSH                          | on       |
| #define FINSH_USING_DESCRIPTION | None           | Add a description to each FinSH symbol                     | on       |
| #define FINSH_USING_MSH         | None           | Enable msh mode                                            | on       |
| #define FINSH_USING_MSH_ONLY    | None           | Use only msh mode                                          | on       |
| #define FINSH_ARG_MAX           | Integer type   | Maximum number of input parameters                         | 10       |
| #define FINSH_USING_AUTH        | None           | Enable permission verification                             | off      |
| #define FINSH_DEFAULT_PASSWORD  | String         | Authority verification password                            | off      |

The reference configuration example in rtconfig.h is as follows, and can be configured according to actual functional requirements.

```c
/* Open FinSH */
#define RT_USING_FINSH

/* Define the thread name as tshell */
#define FINSH_THREAD_NAME "tshell"

/* Open history command */
#define FINSH_USING_HISTORY
/* Record 5 lines of history commands */
#define FINSH_HISTORY_LINES 5

/* Enable the use of the Tab key */
#define FINSH_USING_SYMTAB
/* Turn on description */
#define FINSH_USING_DESCRIPTION

/* Define FinSH thread priority to 20 */
#define FINSH_THREAD_PRIORITY 20
/* Define the stack size of the FinSH thread to be 4KB */
#define FINSH_THREAD_STACK_SIZE 4096
/* Define the command character length to 80 bytes */
#define FINSH_CMD_SIZE 80

/* Open msh function */
#define FINSH_USING_MSH
/* Use msh function by default */
#define FINSH_USING_MSH_DEFAULT
/* The maximum number of input parameters is 10 */
#define FINSH_ARG_MAX 10
```

## FinSH Application Examples

### Examples of msh Command without Arguments

This section demonstrates how to export a custom command to msh. The sample code is as follows, the hello function is created in the code, and the `hello` function can be exported to the FinSH command list via the `MSH_CMD_EXPORT` command.

```c
#include <rtthread.h>

void hello(void)
{
    rt_kprintf("hello RT-Thread!\n");
}

MSH_CMD_EXPORT(hello , say hello to RT-Thread);
```

Once the system is up and running, press the tab key in the FinSH console to see the exported command:

```c
msh />
RT-Thread shell commands:
hello             - say hello to RT-Thread
version           - show RT-Thread version information
list_thread       - list thread
……
```

Run the `hello` command and the results are as follows:

```c
msh />hello
hello RT_Thread!
msh />
```

### Example of msh Command with Parameters

This section demonstrates how to export a custom command with parameters to FinSH. The sample code is as follows, the `atcmd()` function is created in the code, and the `atcmd()` function can be exported to the msh command list via the MSH_CMD_EXPORT command.

```c
#include <rtthread.h>

static void atcmd(int argc, char**argv)
{
    if (argc < 2)
    {
        rt_kprintf("Please input'atcmd <server|client>'\n");
        return;
    }

    if (!rt_strcmp(argv[1], "server"))
    {
        rt_kprintf("AT server!\n");
    }
    else if (!rt_strcmp(argv[1], "client"))
    {
        rt_kprintf("AT client!\n");
    }
    else
    {
        rt_kprintf("Please input'atcmd <server|client>'\n");
    }
}

MSH_CMD_EXPORT(atcmd, atcmd sample: atcmd <server|client>);
```

Once the system is running, press the Tab key in the FinSH console to see the exported command:

```c
msh />
RT-Thread shell commands:
hello             - say hello to RT-Thread
atcmd             - atcmd sample: atcmd <server|client>
version           - show RT-Thread version information
list_thread       - list thread
……
```

Run the `atcmd` command and the result is as follows:

```c
msh />atcmd
Please input 'atcmd <server|client>'
msh />
```

Run the `atcmd server` command and the result is as follows:

```c
msh />atcmd server
AT server!
msh />
```

Run the `atcmd client` command and the result is as follows:

```c
msh />atcmd client
AT client!
msh />
```

## FinSH Porting

FinSH is written entirely in ANSI C and has excellent portability; it has a small memory footprint, and FinSH will not dynamically request memory if you do not use the functions described in the previous section to dynamically add symbols to FinSH. The FinSH source is located in the `components/finsh` directory. Porting FinSH requires attention to the following aspects:

* FinSH thread：

Each command execution is done in the context of a FinSH thread (that is, a tshell thread). When the RT_USING_FINSH macro is defined, the FinSH thread can be initialized by calling `finsh_system_init()` in the initialization thread. In RT-Thread 1.2.0 and later, you don't have to use the `finsh_set_device(const char* device_name)` function to explicitly specify the device to be used. Instead, the `rt_console_get_device()` function is called automatically to use the console device (The `finsh_set_device(const char* device_name)` must be used in 1.1.x and below to specify the device used by FinSH. The FinSH thread is created in the function `finsh_system_init()` function, which will wait for the rx_sem semaphore.

* FinSH output：

The output of FinSH depends on the output of the system and relies on the `rt_kprintf()` output in RT-Thread. In the startup function `rt_hw_board_init()`, the `rt_console_set_device(const char* name)` function sets the FinSH printout device.

* FinSH input：

After the rin_sem semaphore is obtained, the FinSH thread calls the `rt_device_read()` function to obtain a character from the device (select serial device) and then process it. So the migration of FinSH requires the implementation of the `rt_device_read()` function. The release of the rx_sem semaphore completes the input notification to the FinSH thread by calling the `rx_indicate()` function. The usual process is that when the serial port receive interrupt occurs (that is, the serial port has input character), the interrupt service routine calls the `rx_indicate()` function to notify the FinSH thread that there is input, and then the FinSH thread obtains the serial port input and finally performs the corresponding command processing.



# Virtual File System

In early days, the amount of data to be stored in embedded systems was relatively small and data types were relatively simple.
The data were stored by directly writing to a specific address in storage devices. However, with today modern technology, embedded device's functions are getting complicated and required more data storage. Therefore, we need new data management methods to simplify and organize the data storage.

A file system is made up of abstract data types and also a mechanism for providing data access, retrieve, implements, and store them in hierarchical structure. A folder contains multiple files and a file contains multiple organized data on the file system. This chapter explains about the RT-Thread file system, architecture, features and usage of virtual file system in RT-Thread OS.

## An Introduction to DFS

Device File System (DFS) is a virtual file system component and name structure is similar to UNIX files and folders. Following is the files and folders structure:

The root directory is represented by "/". For example, if users want to access to f1.bin file under root directory, it can be accessed by "/f1.bin". If users want to access to f1.bin file under /2019 folder, it can be accessed by "/data/2019/f1.bin" according to their folder paths as in UNIX/Linux unlike Windows System.

### The Architecture of DFS

The main features of the RT-Thread DFS component are:

- Provides a unified POSIX file and directory operations interface for applications: read, write, poll/select, and more.
- Supports multiple types of file systems, such as FatFS, RomFS, DevFS, etc., and provides management of common files, device files, and network file descriptors.
- Supports multiple types of storage devices such as SD Card, SPI Flash, Nand Flash, etc.

The hierarchical structure of DFS is shown in the following figure, which is mainly divided into POSIX interface layer, virtual file system layer and device abstraction layer.

![The hierarchical structure of DFS](figures/fs-layer.png)

### POSIX Interface Layer

POSIX stands for Portable Operating System Interface of UNIX (POSIX). The POSIX standard defines the interface standard that the operating system should provide for applications. It is a general term for a series of API standards defined by IEEE for software to run on various UNIX operating systems.

The POSIX standard is intended to achieve software portability at the source code level. In other words, a program written for a POSIX-compatible operating system should be able to compile and execute on any other POSIX operating system (even from another vendor). RT-Thread supports the POSIX standard interface, so it is easy to port Linux/Unix programs to the RT-Thread operating system.

On UNIX-like systems, normal files, device files, and network file descriptors are the same. In the RT-Thread operating system, DFS is used to achieve this uniformity. With the uniformity of such file descriptors, we can use the `poll/select` interface to uniformly poll these descriptors and bring convenience to the implement of the  program functions.

Using the `poll/select` interface to block and simultaneously detect whether a group of  I/O devices which support non-blocking have events (such as readable, writable, high-priority error output, errors, etc.) until a device trigger the event was or exceed the specified wait time. This mechanism can help callers find devices that are currently ready, reducing the complexity of programming.

### Virtual File System Layer

Users can register specific file systems to DFS, such as FatFS, RomFS, DevFS, etc. Here are some common file system types:

* FatFS is a Microsoft FAT format compatible file system developed for small embedded devices. It is written in ANSI C and has good hardware independence and portability. It is the most commonly used file system type in RT-Thread.
* The traditional RomFS file system is a simple, compact, read-only file system that does not support dynamic erasing and saving or storing data in order, thus it supports applications to run in XIP (execute In Place) method and save RAM space while the system is running.
* The Jffs2 file system is a log flash file system. It is mainly used for NOR flash memory, based on MTD driver layer, featuring: readable and writable, supporting data compression, Hash table based log file system, and providing crash/power failure security protection, write balance support, etc..
* DevFS is the device file system. After the function is enabled in the RT-Thread operating system, the devices in the system can be virtualized into files in the `/dev` folder, so that the device can use the interfaces such as `read` and `write` according to the operation mode of the file to operate.
* NFS (Network File System) is a technology for sharing files over a network between different machines and different operating systems. In the development and debugging phase of the operating system, this technology can be used to build an NFS-based root file system on the host and mount it on the embedded device, which can easily modify the contents of the root file system.
* UFFS is short for Ultra-low-cost Flash File System. It is an open source file system developed by Chinese people and used for running Nand Flash in small memory environments such as embedded devices. Compared with the Yaffs file system which often used in embedded devices, it has the advantages of less resource consumption, faster startup speed and free.

### Device Abstraction Layer

The device abstraction layer abstracts physical devices such as SD Card, SPI Flash, and Nand Flash into devices that are accessible to the file system. For example, the FAT file system requires that the storage device be a block device type.

Different file system types are implemented independently of the storage device driver, so the file system function can be correctly used after the drive interface of the underlying storage device is docked with the file system.

## Mount Management

The initialization process of the file system is generally divided into the following steps:

1. Initialize the DFS component.
2. Initialize a specific type of file system.
3. Create a block device on the memory.
4. Format the block device.
5. Mount the block device to the DFS directory.
6. When the file system is no longer in use, you can unmount it.

### Initialize the DFS Component

The initialization of the DFS component is done by the dfs_init() function. The dfs_init() function initializes the relevant resources required by DFS and creates key data structures that allow DFS to find a specific file system in the system and get a way to manipulate files within a particular storage device. This function will be called automatically if auto-initialization is turned on (enabled by default).

### Registered File System

After the DFS component is initialized, you also need to initialize the specific type of file system used, that is, register a specific type of file system into DFS. The interface to register the file system is as follows:

```c
int dfs_register(const struct dfs_filesystem_ops *ops);
```

| **Parameter** | **Description**                                        |
| ------------- | ------------------------------------------------------ |
| ops           | a collection of operation functions of the file system |
| **return**    | **——**                                                 |
| 0             | file registered successfully                           |
| -1            | file fail to register                                  |

This function does not require user calls, it will be called by the initialization function of different file systems, such as the elm-FAT file system's initialization function `elm_init()`. After the corresponding file system is enabled, if automatic initialization is enabled (enabled by default), the file system initialization function will also be called automatically.

The `elm_init()` function initializes the elm-FAT file system, which calls the `dfs_register(`) function to register the elm-FAT file system with DFS. The file system registration process is shown below:

![Register file system](figures/fs-reg.png)

### Register a Storage Device as a Block Device

Only block devices can be mounted to the file system,  so you need to create the required block devices on the storage device. If the storage device is SPI Flash, you can use the "Serial Flash Universal Driver Library SFUD" component, which provides various SPI Flash drivers, and abstracts the SPI Flash into a block device for mounting. The  process of registering block device is shown as follows:

![The timing diagram of registering block device](figures/fs-reg-block.png)

### Format the file system

After registering a block device, you also need to create a file system of the specified type on the block device, that is, format the file system. You can use the `dfs_mkfs()` function to format the specified storage device and create a file system. The interface to format the file system is as follows：

```c
int dfs_mkfs(const char * fs_name, const char * device_name);
```

| **Parameter** | **Description**                    |
| ------------- | ---------------------------------- |
| fs_name       | type of the file system            |
| device_name   | name of the block device           |
| **return**    | **——**                             |
| 0             | file system formatted successfully |
| -1            | fail to format the file system     |

The file system type (fs_name) possible values and the corresponding file system is shown in the following table:

| **Value** | **File System Type**               |
| --------- | ---------------------------------- |
| elm       | elm-FAT file system                |
| jffs2     | jffs2 journaling flash file system |
| nfs       | NFS network file system            |
| ram       | RamFS file system                  |
| rom       | RomFS read-only file system        |
| uffs      | uffs file system                   |

Take the elm-FAT file system format block device as an example. The formatting process is as follows:

![Formatted file system](figures/elm-fat-mkfs.png)

You can also format the file system using the `mkfs` command. The result of formatting the block device sd0 is as follows:

```shell
msh />mkfs sd0                    # Sd0 is the name of the block device, the command will format by default
sd0 is elm-FAT file system
msh />
msh />mkfs -t elm sd0             # Use the -t parameter to specify the file system type as elm-FAT file system
```

### Mount file system

In RT-Thread, mounting refers to attaching a storage device to an existing path. To access a file on a storage device, we must mount the partition where the file is located to an existing path and then access the storage device through this path. The interface to mount the file system is as follows:

```c
int dfs_mount(const char   *device_name,
              const char   *path,
              const char   *filesystemtype,
              unsigned long rwflag,
              const void   *data);
```

| **Parameter**  | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| device_name    | the name of the block device that has been formatted         |
| path           | the mount path                                               |
| filesystemtype | The type of the mounted file system. Possible values can refer to the dfs_mkfs() function description. |
| rwflag         | read and write flag bit                                      |
| data           | private data for a specific file system                      |
| **return**     | **——**                                                       |
| 0              | file system mounted successfully                             |
| -1             | file system mount fail to be mounted                         |

If there is only one storage device, it can be mounted directly to the root directory `/`.

### Unmount a file system

When a file system does not need to be used anymore, it can be unmounted. The interface to unmount the file system is as follows:

```c
int dfs_unmount(const char *specialfile);
```

| **Parameter** | **Description**                      |
| ------------- | ------------------------------------ |
| specialfile   | mount path                           |
| **return**    | **——**                               |
| 0             | unmount the file system successfully |
| -1            | fail to unmount the file system      |

## Document Management

This section introduces the functions that are related to the operation of the file. The operation of the file is generally based on the file descriptor fd, as shown in the following figure:

![common function of file management](figures/fs-mg.png)

### Open and Close Files

To open or create a file, you can call the following open() function:

```c
int open(const char *file, int flags, ...);
```

| **Parameter**   | **Description**                                              |
| --------------- | ------------------------------------------------------------ |
| file            | file names that are opened or created                        |
| flags           | Specify the way to open the file, and values can refer to the following table. |
| **return**      | **——**                                                       |
| file descriptor | file opened successfully                                     |
| -1              | fail to open the file                                        |

A file can be opened in a variety of ways, and multiple open methods can be specified at the same time. For example, if a file is opened by O_WRONLY and O_CREAT, then when the specified file which need to be open does not exist, it will create the file first and then open it as write-only. The file opening method is as follows:

| **Parameter** | **Description**                                              |
| ------------- | ------------------------------------------------------------ |
| O_RDONLY      | open file in read-only mode                                  |
| O_WRONLY      | open file in write-only mode                                 |
| O_RDWR        | open file in read-write mode                                 |
| O_CREAT       | if the file to be open does not exist, then you can create the file |
| O_APPEND      | When the file is read or written, it will start from the end of the file, that is, the data written will be added to the end of the file in an additional way. |
| O_TRUNC       | empty the contents of the file if it already exists          |

If you no longer need to use the file, you can use the `close()` function to close the file, and `close()` will write the data back to disk and release the resources occupied by the file.

```
int close(int fd);
```

| **Parameter** | **Description**          |
| ------------- | ------------------------ |
| fd            | file descriptor          |
| **return**    | **——**                   |
| 0             | file closed successfully |
| -1            | fail to close the file   |

### Read and Write Data

To read the contents of a file, use the `read()` function:

```c
int read(int fd, void *buf, size_t len);
```

| **Parameter** | **Description**                                              |
| ------------- | ------------------------------------------------------------ |
| fd            | file descriptor                                              |
| buf           | buffer pointer                                               |
| len           | read number of bytes of the files                            |
| **return**    | **——**                                                       |
| int           | the number of bytes actually read                            |
| 0             | read data has reached the end of the file or there is no readable data |
| -1            | read error, error code to view the current thread's errno    |

This function reads the `len` bytes of the file pointed to by the parameter `fd` into the memory pointed to by the `buf pointer`. In addition, the read/write position pointer of the file moves with the byte read.

To write data into a file, use the `write()` function:

```c
int write(int fd, const void *buf, size_t len);
```

| **Parameter** | **Description**                                            |
| ------------- | ---------------------------------------------------------- |
| fd            | file descriptor                                            |
| buf           | buffer pointer                                             |
| len           | the number of bytes written to the file                    |
| **return**    | **——**                                                     |
| int           | the number of bytes actually written                       |
| -1            | write error, error code to view the current thread's errno |

This function writes `len` bytes in the memory pointed out by the `buf pointer` into the file pointed out by the parameter `fd`. In addition, the read and write location pointer of the file moves with the bytes written.

### Rename

To rename a file, use the `rename()` function:

```
int rename(const char *old, const char *new);
```

| **Parameter** | **Description**              |
| ------------- | ---------------------------- |
| old           | file's old name              |
| new           | new name                     |
| **return**    | **——**                       |
| 0             | change the name successfully |
| -1            | fail to change the name      |

This function changes the file name specified by the parameter `old` to the file name pointed to by the parameter `new`. If the file specified by `new` already exists, the file will be overwritten.

### Get Status

To get the file status, use the following `stat()` function:

```c
int stat(const char *file, struct stat *buf);
```

| **Parameter** | **Description**                                              |
| ------------- | ------------------------------------------------------------ |
| file          | file name                                                    |
| buf           | structure pointer to a structure that stores file status information |
| **return**    | **——**                                                       |
| 0             | access status successfully                                   |
| -1            | fail to access to status                                     |

### Delete Files

Delete a file in the specified directory using the `unlink()` function:

```
int unlink(const char *pathname);
```

| **Parameter** | **Description**                              |
| ------------- | -------------------------------------------- |
| pathname      | specify the absolute path to delete the file |
| **return**    | **——**                                       |
| 0             | deleted the file successfully                |
| -1            | fail to deleted the file                     |

### Synchronize File Data to Storage Devices

Synchronize all modified file data in memory to the storage device using the `fsync()` function:

```c
int fsync(int fildes);
```

| **Parameter** | **Description**                |
| ------------- | ------------------------------ |
| fildes        | file descriptor                |
| **Return**    | **——**                         |
| 0             | synchronize files successfully |
| -1            | fail to synchronize files      |

### Query file system related information

Use the `statfs()` function to query file system related information.

```c
int statfs(const char *path, struct statfs *buf);
```

| **Parameter** | **Description**                                       |
| ------------- | ----------------------------------------------------- |
| path          | file system mount path                                |
| buf           | structure pointer for storing file system information |
| **Return**    | **——**                                                |
| 0             | query file system information successfully            |
| -1            | fail to query file system information                 |

### Monitor I/O device status

To monitor the I/O device for events, use the `select()` function:

```c
int select( int nfds,
            fd_set *readfds,
            fd_set *writefds,
            fd_set *exceptfds,
            struct timeval *timeout);
```

| **Parameter**  | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| nfds           | The range of all file descriptors in the collection, that is, the maximum value of all file descriptors plus 1 |
| readfds        | Collection of file descriptors that need to monitor read changes |
| writefds       | Collection of file descriptors that need to monitor write changes |
| exceptfds      | Collection of file descriptors that need to be monitored for exceptions |
| timeout        | timeout of **select**                                        |
| **return**     | **——**                                                       |
| positive value | a read/write event or error occurred in the monitored file collection |
| 0              | waiting timeout, no readable or writable or erroneous files  |
| negative value | error                                                        |

Use the `select()` interface to block and simultaneously detect whether a group of non-blocking I/O devices have events (such as readable, writable, high-priority error output, errors, etc.) until a device triggered an event or exceeded a specified wait time.

## Directory management

This section describes functions that directory management often uses, and operations on directories are generally based on directory addresses, as shown in the following image:

![functions that directory management often uses](figures/fs-dir-mg.png)

### Create and Delete Directories

To create a directory, you can use the mkdir() function:

```c
int mkdir(const char *path, mode_t mode);
```

| **Parameter** | **Description**                       |
| ------------- | ------------------------------------- |
| path          | the absolute address of the directory |
| mode          | create a pattern                      |
| **Return**    | **——**                                |
| 0             | create directory successfully         |
| -1            | fail to create directory              |

This function is used to create a directory as a folder, the parameter path is the absolute path of the directory, the parameter mode is not enabled in the current version, so just fill in the default parameter 0x777.

Delete a directory using the rmdir() function:

```c
int rmdir(const char *pathname);
```

| **Parameter** | **Description**                       |
| ------------- | ------------------------------------- |
| pathname      | absolute path to delete the directory |
| **Return**    | **——**                                |
| 0             | delete the directory successfully     |
| -1            | fail to delete the directory          |

### Open and Close the Directory

Open the directory to use the `opendir()` function:

```c
DIR* opendir(const char* name);
```

| **Parameter** | **Description**                                              |
| ------------- | ------------------------------------------------------------ |
| name          | absolute address of the directory                            |
| **Return**    | **——**                                                       |
| DIR           | open the directory successfully, and return to a pointer to the directory stream |
| NULL          | fail to open                                                 |

To close the directory, use the `closedir()` function:

```c
int closedir(DIR* d);
```

| **Parameter** | **Description**               |
| ------------- | ----------------------------- |
| d             | directory stream pointer      |
| **Return**    | **——**                        |
| 0             | directory closed successfully |
| -1            | directory closing error       |

This function is used to close a directory and must be used with the `opendir()` function.

### Read Directory

To read the directory, use the `readdir()` function:

```c
struct dirent* readdir(DIR *d);
```

| **Parameter** | **Description**                                              |
| ------------- | ------------------------------------------------------------ |
| d             | directory stream pointer                                     |
| **Return**    | **——**                                                       |
| dirent        | read successfully and return to a structure pointer to a directory entry |
| NULL          | read to the end of the directory                             |

This function is used to read the directory, and the parameter d is the directory stream pointer. In addition, each time a directory is read, the pointer position of the directory stream is automatically recursed by 1 position backward.

### Get the Read Position of the Directory Stream

To get the read location of the directory stream, use the `telldir()` function:

```
long telldir(DIR *d);
```

| **Parameter** | **Description**                 |
| ------------- | ------------------------------- |
| d             | directory stream pointer        |
| **Return**    | **——**                          |
| long          | read the offset of the position |

The return value of this function records the current position of a directory stream. This return value represents the offset from the beginning of the directory file. You can use this value in the following  `seekdir()` to reset the directory to the current position. In other words, the `telldir()` function can be used with the `seekdir()` function to reset the read position of the directory stream to the specified offset.

### Set the Location to Read the Directory Next Time

Set the location to read the directory next time using the `seekdir()` function:

```
void seekdir(DIR *d, off_t offset);
```

| **Parameter** | **Description**                                    |
| ------------- | -------------------------------------------------- |
| d             | directory stream pointer                           |
| offset        | the offset value, displacement from this directory |

This is used to set the read position of the parameter d directory stream, and starts reading from this new position when readdir() is called.

### Reset the Position of Reading Directory to the Beginning

To reset the directory stream's read position to the beginning, use the `rewinddir()` function:

```
void rewinddir(DIR *d);
```

| **Parameter** | **Description**          |
| ------------- | ------------------------ |
| d             | directory stream pointer |

This function can be used to set the current read position of the `d` directory stream to the initial position of the directory stream.

## DFS Configuration Options

The specific configuration path of the file system in menuconfig is as follows:

```c
RT-Thread Components  --->
    Device virtual file system  --->
```

The configuration menu description and corresponding macro definitions are shown in the following table:

| **Configuration Options**                                    | **Corresponding Macro Definition** | **Description**                          |
| ------------------------------------------------------------ | ---------------------------------- | ---------------------------------------- |
| [*] Using device virtual file system                         | RT_USING_DFS                       | Open DFS virtual file system             |
| [*]   Using working directory                                | DFS_USING_WORKDIR                  | open a relative path                     |
| (2)   The maximal number of mounted file system              | DFS_FILESYSTEMS_MAX                | maximum number of mounted file systems   |
| (2)   The maximal number of file system type                 | DFS_FILESYSTEM_TYPES_MAX           | maximum number of supported file systems |
| (4)   The maximal number of opened files                     | DFS_FD_MAX                         | maximum number of open files             |
| [ ]   Using mount table for file system                      | RT_USING_DFS_MNTTABLE              | open the automatic mount table           |
| [*]   Enable elm-chan fatfs                                  | RT_USING_DFS_ELMFAT                | open the elm-FatFs file system           |
| [*]   Using devfs for device objects                         | RT_USING_DFS_DEVFS                 | open the DevFS device file system        |
| [ ]   Enable ReadOnly file system on flash                   | RT_USING_DFS_ROMFS                 | open the RomFS file system               |
| [ ]   Enable RAM file system                                 | RT_USING_DFS_RAMFS                 | open the RamFS file system               |
| [ ]   Enable UFFS file system: Ultra-low-cost Flash File System | RT_USING_DFS_UFFS                  | open the UFFS file system                |
| [ ]   Enable JFFS2 file system                               | RT_USING_DFS_JFFS2                 | open the JFFS2 file system               |
| [ ]   Using NFS v3 client file system                        | RT_USING_DFS_NFS                   | open the NFS file system                 |

By default, the RT-Thread operating system does not turn on the relative path function in order to obtain a small memory footprint. When the Support Relative Paths option is not turned on, you should use an absolute directory when working with files and directory interfaces (because there is no currently working directory in the system). If you need to use the current working directory and the relative directory, you can enable the relative path function in the configuration item of the file system.

When the option `[*] Use mount table for file system` is selected, the corresponding macro `RT_USING_DFS_MNTTABLE` will be enabled to turn on the automatic mount table function. The automatic `mount_table[]` is provided by the user in the application code. The user needs to specify the device name, mount path, file system type, read and write flag and private data in the table. After that, the system will traverse the mount table to execute the mount. It should be noted that the mount table must end with `{0}` to judge the end of the table.

The automatic mount table `mount_table []` is shown below, where the five members of `mount_table [0]` are the five parameters of function `dfs_mount ()`. This means that the elm file system is mounted `/` path on the flash 0 device, rwflag is 0, data is 0, `mount_table [1]` is `{0}` as the end to judge the end of the table.

```c
const struct dfs_mount_tbl mount_table[] =
{
    {"flash0", "/", "elm", 0, 0},
    {0}
};
```

### elm-FatFs File System Configuration Option

Elm-FatFs can be further configured after opening the elm-FatFs file system in menuconfig. The configuration menu description and corresponding macro definitions are as follows:

| **Configuration Options**                                   | **Corresponding Macro Definition** | **Description**                    |
| ----------------------------------------------------------- | ---------------------------------- | ---------------------------------- |
| (437) OEM code page                                         | RT_DFS_ELM_CODE_PAGE               | encoding mode                      |
| [*] Using RT_DFS_ELM_WORD_ACCESS                            | RT_DFS_ELM_WORD_ACCESS             |                                    |
| Support long file name (0: LFN disable)  --->               | RT_DFS_ELM_USE_LFN                 | open long file name submenu        |
| (255) Maximal size of file name length                      | RT_DFS_ELM_MAX_LFN                 | maximum file name length           |
| (2) Number of volumes (logical drives) to be used.          | RT_DFS_ELM_DRIVES                  | number of devices mounting FatFs   |
| (4096) Maximum sector size to be handled.                   | RT_DFS_ELM_MAX_SECTOR_SIZE         | the sector size of the file system |
| [ ] Enable sector erase feature                             | RT_DFS_ELM_USE_ERASE               |                                    |
| [*] Enable the reentrancy (thread safe) of the FatFs module | RT_DFS_ELM_REENTRANT               | open reentrant                     |

#### Long File Name

By default, FatFs file naming has the following disadvantages:

- The file name (without suffix) can be up to 8 characters long and the suffix can be up to 3 characters long. The file name and suffix will be truncated when the limit is exceeded.
- File name does not support case sensitivity (displayed in uppercase).

If you need to support long filenames, you need to turn on the option to support long filenames. The  submenu of the long file name is described as follows:

| **Configuration Options**                               | **Corresponding Macro Definition** | **Description**                                              |
| ------------------------------------------------------- | ---------------------------------- | ------------------------------------------------------------ |
| ( ) 0: LFN disable                                      | RT_DFS_ELM_USE_LFN_0               | close the long file name                                     |
| ( ) 1: LFN with static LFN working buffer               | RT_DFS_ELM_USE_LFN_1               | use static buffers to support long file names, and multi-threaded operation of file names will bring re-entry problems |
| ( ) 2: LFN with dynamic LFN working buffer on the stack | RT_DFS_ELM_USE_LFN_2               | long file names are supported by temporary buffers in the stack. Larger demand for stack space. |
| (X) 3: LFN with dynamic LFN working buffer on the heap  | RT_DFS_ELM_USE_LFN_3               | use the heap (malloc request) buffer to store long filenames, it is the safest (default) |

#### Encoding Mode

When long file name support is turned on, you can set the encoding mode for the file name. RT-Thread/FatFs uses 437 encoding (American English) by default. If you need to store the Chinese file name, you can use 936 encoding (GBK encoding). The 936 encoding requires a font library of approximately 180KB. If you only use English characters as a file, we recommend using 437 encoding (American English), this will save this 180KB of Flash space.

The file encodings supported by FatFs are as follows:

```c
/* This option specifies the OEM code page to be used on the target system.
/  Incorrect setting of the code page can cause a file open failure.
/
/   1   - ASCII (No extended character. Non-LFN cfg. only)
/   437 - U.S.
/   720 - Arabic
/   737 - Greek
/   771 - KBL
/   775 - Baltic
/   850 - Latin 1
/   852 - Latin 2
/   855 - Cyrillic
/   857 - Turkish
/   860 - Portuguese
/   861 - Icelandic
/   862 - Hebrew
/   863 - Canadian French
/   864 - Arabic
/   865 - Nordic
/   866 - Russian
/   869 - Greek 2
/   932 - Japanese (DBCS)
/   936 - Simplified Chinese (DBCS)
/   949 - Korean (DBCS)
/   950 - Traditional Chinese (DBCS)
*/
```

#### File System Sector Size

Specify the internal sector size of FatFs, which needs to be greater than or equal to the sector size of the actual hardware driver. For example, if a spi flash chip sector is 4096 bytes, the above macro needs to be changed to 4096. Otherwise, when the FatFs reads data from the driver, the array will be out of bounds and the system will crash (the new version gives a warning message when the system is executed) .

Usually Flash device can be set to 4096, and the common TF card and SD card have a sector size of 512.

#### Reentrant

FatFs fully considers the situation of multi-threaded safe read and write security. When reading and writing FafFs in multi-threading, in order to avoid the problems caused by re-entry, you need to open the macro above. If the system has only one thread to operate the file system and there is no reentrancy problem, you can turn it off to save resources.

#### More Configuration

FatFs itself supports a lot of configuration options and the configuration is very flexible. The following file is a FatFs configuration file that can be modified to customize FatFs.

```c
components/dfs/filesystems/elmfat/ffconf.h
```

## DFS Application Example

### FinSH Command

After the file system is successfully mounted, the files and directories can be operated. The commonly used FinSH commands for file system operations are shown in the following table:

| **FinSH Command** | **Description**                                              |
| ----------------- | ------------------------------------------------------------ |
| ls                | display information about files and directories              |
| cd                | enter the specified directory                                |
| cp                | copy file                                                    |
| rm                | delete the file or the directory                             |
| mv                | move the file or rename it                                   |
| echo              | write the specified content to the specified file, write the file when it exists, and create a new file and write when the file does not exist. |
| cat               | display the contents of the file                             |
| pwd               | print out the current directory address                      |
| mkdir             | create a folder                                              |
| mkfs              | formatted the file system                                    |

Use the `ls` command to view the current directory information, and the results are as follows:

```c
msh />ls                          # use the `ls` command to view the current directory information
Directory /:                      # you can see that the root directory already exists /
```

Use the `mkdir` command to create a folder, and the results are as follows:

```c
msh />mkdir rt-thread             # create an rt-thread folder
msh />ls                          # view directory information as follows
Directory /:
rt-thread           <DIR>
```

Use the `echo` command to output the input string to the specified output location. The result is as follows:

```c
msh />echo "hello rt-thread!!!"                # outputs the string to standard output
hello rt-thread!!!
msh />echo "hello rt-thread!!!" hello.txt      # output the string output to the hello.txt file
msh />ls
Directory /:
rt-thread           <DIR>
hello.txt           18
msh />
```

Use the `cat` command to view the contents of the file. The result is as follows:

```c
msh />cat hello.txt                     # view the contents of the hello.txt file and output
hello rt-thread!!!
```

Use the `rm` command to delete a folder or file. The result is as follows:

```c
msh />ls                                # view the information of current directory
Directory /:
rt-thread           <DIR>
hello.txt           18
msh />rm rt-thread                      # delete the rt-thread folder
msh />ls
Directory /:
hello.txt           18
msh />rm hello.txt                      # delete the hello.txt file
msh />ls
Directory /:
msh />
```

### Read and Write File Examples

Once the file system is working, you can run the application example. In the sample code, you first create a file `text.txt` using the `open()` function and write the string `"RT -Thread Programmer!\n"` in the file using the `write()` function, and then close the file. Use the ` open()` function again to open the `text.txt` file, read the contents and print it out, and close the file finally.

The sample code is as follows:

```c
#include <rtthread.h>
#include <dfs_posix.h> /* this header file need to be included when you need to operate the file */

static void readwrite_sample(void)
{
    int fd, size;
    char s[] = "RT-Thread Programmer!", buffer[80];

    rt_kprintf("Write string %s to test.txt.\n", s);

    /* open the ‘/text.txt’ file in create and read-write mode and create the file if it does not exist*/
    fd = open("/text.txt", O_WRONLY | O_CREAT);
    if (fd>= 0)
    {
        write(fd, s, sizeof(s));
        close(fd);
        rt_kprintf("Write done.\n");
    }

      /* open the ‘/text.txt’ file in read-only mode */
    fd = open("/text.txt", O_RDONLY);
    if (fd>= 0)
    {
        size = read(fd, buffer, sizeof(buffer));
        close(fd);
        rt_kprintf("Read from file test.txt : %s \n", buffer);
        if (size < 0)
            return ;
    }
  }
/* export to the msh command list */
MSH_CMD_EXPORT(readwrite_sample, readwrite sample);

```

### An Example of Changing the File Name

The sample code in this section shows how to modify the file name. The program creates a function `rename_sample()` that manipulates the file and exports it to the msh command list. This function calls the `rename()` function to rename the file named `text.txt` to `text1.txt`. The sample code is as follows:

```c
#include <rtthread.h>
#include <dfs_posix.h> /* this header file need to be included when you need to operate the file */

static void rename_sample(void)
{
    rt_kprintf("%s => %s", "/text.txt", "/text1.txt");

    if (rename("/text.txt", "/text1.txt") < 0)
        rt_kprintf("[error!]\n");
    else
        rt_kprintf("[ok!]\n");
}
/* export to the msh command list */
MSH_CMD_EXPORT(rename_sample, rename sample);
```

Run the example in the FinSH console and the results are as follows:

```shell
msh />echo "hello" text.txt
msh />ls
Directory /:
text.txt           5
msh />rename_sample
/text.txt => /text1.txt [ok!]
msh />ls
Directory /:
text1.txt           5
```

In the example demonstration, we first create a file named `text.txt` using the echo command, and then run the sample code to change the file name of the file `text.txt` to `text1.txt`.

### Get File Status Example

The sample code shows how to get the file status. The program creates a function `stat_sample()` that manipulates the file and exports it to the msh command list. This function calls the `stat()` function to get the file size information of the text.txt file. The sample code is as follows:

```c
#include <rtthread.h>
#include <dfs_posix.h> /* this header file need to be included when you need to operate the file */

static void stat_sample(void)
{
    int ret;
     struct stat buf;
     ret = stat("/text.txt", &buf);
    if(ret == 0)
    rt_kprintf("text.txt file size = %d\n", buf.st_size);
    else
    rt_kprintf("text.txt file not fonud\n");
}
/* export to the msh command list */
MSH_CMD_EXPORT(stat_sample, show text.txt stat sample);
```

Run the example in the FinSH console and the results are as follows:

```c
msh />echo "hello" text.txt
msh />stat_sample
text.txt file size = 5
```

During the example run, the file `text.txt` is first created with the `echo` command, then the sample code is run, and the file size information for the file `text.txt` is printed.

### Create a Directory Example

The sample code in this section shows how to create a directory. The program creates a function file `mkdir_sample()` that manipulates the file and exports it to the msh command list, which calls the `mkdir()` function to create a folder called `dir_test`. The sample code is as follows:

```c
#include <rtthread.h>
#include <dfs_posix.h> /* this header file need to be included when you need to operate the file */

static void mkdir_sample(void)
{
    int ret;

    /* create a directory */
    ret = mkdir("/dir_test", 0x777);
    if (ret < 0)
    {
        /* fail to create a directory */
        rt_kprintf("dir error!\n");
    }
    else
    {
        /* create a directory successfully */
        rt_kprintf("mkdir ok!\n");
    }
}
/* export to the msh command list */
MSH_CMD_EXPORT(mkdir_sample, mkdir sample);
```

Run the example in the FinSH console and the result is as follows:

```shell
msh />mkdir_sample
mkdir ok!
msh />ls
Directory /:
dir_test                 <DIR>    # <DIR> it indicates that the type of the directory is a folder
```

This example demonstrates creating a folder named `dir_test` in the root directory.

### Read directory Example

The sample code shows how to read the directory. The program creates a function `readdir_sample()` that manipulates the file and exports it to the msh command list. This function calls the `readdir()` function to get the contents of the `dir_test` folder and print it out. The sample code is as follows:

```c
#include <rtthread.h>
#include <dfs_posix.h> /* this header file need to be included when you need to operate the file */

static void readdir_sample(void)
{
    DIR *dirp;
    struct dirent *d;

    /* open the / dir_test directory */
    dirp = opendir("/dir_test");
    if (dirp == RT_NULL)
    {
        rt_kprintf("open directory error!\n");
    }
    else
    {
        /* read the directory */
        while ((d = readdir(dirp)) != RT_NULL)
        {
            rt_kprintf("found %s\n", d->d_name);
        }

        /* close the directory */
        closedir(dirp);
    }
}
/* exports to the msh command list */
MSH_CMD_EXPORT(readdir_sample, readdir sample);
```

Run the example in the FinSH console and the result is as follows:

```shell
msh />ls
Directory /:
dir_test                 <DIR>
msh />cd dir_test
msh /dir_test>echo "hello" hello.txt       # create a hello.txt file
msh /dir_test>cd ..                        # switch to the parent folder
msh />readdir_sample
found hello.txt
```

In this example, first create a hello.txt file under the dir_test folder and exit the dir_test folder. At this point, run the sample program to print out the contents of the dir_test folder.

### An Example of Setting the location of the read directory

The sample code in this section shows how to set the location to read the directory next time. The program creates a function `telldir_sample()` that manipulates the file and exports it to the msh command list. This function first opens the root directory, then reads all the directory information in the root directory and prints the directory information. Meanwhile, use the `telldir()` function to record the location information of the third directory entry. Before reading the directory information in the root directory for the second time, use the `seekdir()` function to set the read location to the address of the third directory entry previously recorded. At this point, read the information in the root directory again, and the directory information is printed out. The sample code is as follows:

```c
#include <rtthread.h>
#include <dfs_posix.h> /* this header file need to be included when you need to operate the file */

/* assume that the file operation is done in one thread */
static void telldir_sample(void)
{
    DIR *dirp;
    int save3 = 0;
    int cur;
    int i = 0;
    struct dirent *dp;

    /* open the root directory */
    rt_kprintf("the directory is:\n");
    dirp = opendir("/");

    for (dp = readdir(dirp); dp != RT_NULL; dp = readdir(dirp))
    {
        /* save the directory pointer for the third directory entry */
        i++;
        if (i == 3)
            save3 = telldir(dirp);

        rt_kprintf("%s\n", dp->d_name);
    }

    /* go back to the directory pointer of the third directory entry you just saved */
    seekdir(dirp, save3);

    /* Check if the current directory pointer is equal to the pointer to the third directory entry that was saved. */
    cur = telldir(dirp);
    if (cur != save3)
    {
        rt_kprintf("seekdir (d, %ld); telldir (d) == %ld\n", save3, cur);
    }

    /* start printing from the third directory entry */
    rt_kprintf("the result of tell_seek_dir is:\n");
    for (dp = readdir(dirp); dp != NULL; dp = readdir(dirp))
    {
         rt_kprintf("%s\n", dp->d_name);
    }

    /* close the directory */
    closedir(dirp);
}
/* exports to the msh command list */
MSH_CMD_EXPORT(telldir_sample, telldir sample);
```

In this demo example, you need to manually create the five folders from `hello_1` to `hello_5` in the root directory with the `mkdir` command, making sure that there is a folder directory under the root directory for running the sample.

Run the example in the FinSH console and the results are as follows:

```shell
msh />ls
Directory /:
hello_1             <DIR>
hello_2             <DIR>
hello_3             <DIR>
hello_4             <DIR>
hello_5             <DIR>
msh />telldir_sample
the directory is:
hello_1
hello_2
hello_3
hello_4
hello_5
the result of tell_seek_dir is:
hello_3
hello_4
hello_5
```

After running the sample, you can see that the first time you read the root directory information, it starts from the first folder and prints out all the directory information in the root directory. When the directory information is printed for the second time, since the starting position of the reading is set to the position of the third folder by using the `seekdir()` function, the second time when reading the root directory is from the third folder. Start reading until the last folder, only the directory information from `hello_3` to `hello_5` is printed.

## FAQ

### Q: What should I do if I find that the file name or folder name is not displayed properly?

  **A:** Check if long file name support is enabled, DFS feature configuration section.

### Q: What should I do if the file system fails to initialize?

  **A:** Check if the type and number of file systems allowed to be mounted in the file system configuration project are sufficient.

### Q: What should I do if the file system *mkfs* command fails?

  **A:** Check if the storage device exists. If it exists, check to see if the device driver can pass the function test, if it fails, check the driver error. Check if the libc function is enabled.

### Q: What should I do if the file system fails to mount?

**A:**

- Check if the specified mount path exists. The file system can be mounted directly to the root directory ("/"), but if you want to mount it on another path, such as ("/sdcard"). You need to make sure that the ("/sdcard") path exists. Otherwise, you need to create the `sdcard` folder in the root directory before the mount is successful.
- Check if the file system is created on the storage device. If there is no file system on the storage device, you need to create a file system on the storage using the `mkfs` command.

### Q: What should I do if SFUD cannot detect the Flash specific model?

  **A:**

- Check if the hardware pin settings are wrong.
- Whether the SPI device is already registered.
- Whether the SPI device is mounted to the bus.
- Check the `Using auto probe flash JEDEC SFDP parameter` and the `Using defined supported flash chip information table' under the 'RT-Thread Components → Device Drivers -> Using SPI Bus/Device device drivers -> Using Serial Flash Universal Driver` menu, to see whether the configuration item is selected, if it is not selected then you need to enable these two options.
- If the storage device is still not recognized with the above option turned on, then issues can be raised in the [SFUD](https://github.com/armink/SFUD) project.

 ### Q: Why does the benchmark test of the storage device take too long?

**A:**

  - Compare the [benchmark test data](https://github.com/armink/SFUD/blob/master/docs/zh/benchmark.txt) when the `system tick` is 1000 and the length of time required for this test. If the time lag is too large, you can think that the test work is not working properly.
  - Check the settings of the system tick, because some delay operations will be determined according to the tick time, so you need to set the appropriate `system tick` value according to the system conditions. If the system's `system tick` value is no less than 1000, you will need to use a logic analyzer to check the waveform to determine that the communication rate is normal.

### Q: SPI Flash implements elmfat file system, and how to keep some sectors not used by file system?

  **A:** You can create multiple block devices for the entire storage device using the [partition](https://github.com/RT-Thread-packages/partition) tool package provided by RT-Thread. And block devices can be assigned different functions.

### Q: What should I do if the program gets stuck during the test file system?

  **A:** Try using the debugger or print some necessary debugging information to determine where the program is stuck and ask questions.

### Q: How can I check the problem of the file system step by step?

  **A:** You can step through the problem from the bottom to the top.

- First check if the storage device is successfully registered and the function is normal.
- Check if a file system is created in the storage device.
- Check if the specified file system type is registered to the DFS framework, and often check if the allowed file system types and quantities are sufficient.
- Check if DFS is successfully initialized. The initialization of this step is pure software, so the possibility of error is not high. It should be noted that if component auto-initialization is turned on, there is no need to manually initialize it again. 

# Ulog Log

## Ulog Introduction

**Log definition**：The log is to output the status, process and other information of the software to different media (for example: file, console, display, etc.), display and save. Provide reference for software traceability, performance analysis, system monitoring, fault warning and other functions during software debugging and maintenance. It can be said that the use of logs consumes at least 80% of the software life cycle.

**The importance of the log**：For the operating system, because the complexity of the software is very large, single-step debugging is not suitable in some scenarios, the log component is almost standard part on the operating system. A sophisticated logging system can also make the debugging of the operating system more effective.

**The origin of ulog**: RT-Thread has always lacked a small, useful log component, and the birth of ulog complements this short board. It will be open sourced as a basic component of RT-Thread, allowing our developers to use a simple and easy-to-use logging system to improve development efficiency.

Ulog is a very simple and easy to use C/C++ log component. The first letter u stands for μ, which means micro. It can achieve the lowest **ROM<1K, RAM<0.2K** resource usage. Ulog is not only small in size, but also has very comprehensive functions. Its design concept refers to another C/C++ open source log library: EasyLogger (referred to as elog), and has made many improvements in terms of functions and performance. The main features are as follows:

* The backend of the log output is diversified and can support, for example, serial port, network, file, flash memory and other backend forms.

* The log output is designed to be thread-safe and supports asynchronous output mode.

* The logging system is highly reliable and is still available in complex environments such as interrupted ISRs and Hardfault.

* The log supports runtime/compilation time to set the output level.

* The log content supports global filtering by keyword and label.

* The APIs and log formats are compatible with linux syslog.

* Support for dumping debug data to the log in hex format.

* Compatible with `rtdbg` (RTT's early log header file) and EasyLogger's log output API.

### Ulog Architecture

The following figure shows the ulog log component architecture diagram:

![ulog architecture](figures/ulog_framework.png)

* **Front end**：This layer is the closest layer to the application, and provides users with two types of API interfaces, `syslog` and `LOG_X`, which are convenient for users to use in different scenarios.

* **Core**：The main work of the middle core layer is to format and filter the logs passed by the upper layer, and then generate log frames, and finally output them to the lowest-end back-end devices through different output modules.

* **Back end**：After receiving the log frames sent from the core layer, the logs are output to the registered log backend devices, such as files, consoles, log servers, and so on.

### Configuration Options ###

The path to configure ulog using menuconfig in the ENV tool is as follows:

```c
 RT-Thread Components → Utilities → Enable ulog
```

 The ulog configuration options are described below. In general, the default configuration is used:

```c
[*] Enable ulog                   /* Enable ulog */
      The static output log level./* Select a static log output level. After the selection is completed, the log level lower than the set level (here specifically the log using the LOG_X API) will not be compiled into the ROM. */
[ ]   Enable ISR log.             /* Enable interrupted ISR log, ie log output API can also be used in ISR */
[*]   Enable assert check.        /* Enable assertion checks. If fter disabled, the asserted log will not be compiled into ROM */
(128) The log's max width.        /* The maximum length of the log. Since ulog's logging API is in units of rows, this length also represents the maximum length of a row of logs. */
[ ]   Enable async output mode.   /* Enable asynchronous log output mode. When this mode is turned on, the log will not be output to the backend immediately, but will be cached first, and then handed to the log output thread (for example: idle thread) to output. */
      log format  --->            /* Configure the format of the log, such as time information, color information, thread information, whether to support floating point, etc. */
[*]   Enable console backend.     /* Enable the console as a backend. After enabling, the log can be output to the console serial port. It is recommended to keep it on. */
[ ]   Enable runtime log filter.  /* Enable the runtime log filter, which is dynamic filtering. After enabling, the log will support dynamic filtering when the system is running, by means of tags, keywords, and so on. */
```

**The configuration log format option description is as follows:**

```c
[ ] Enable float number support. It will using more thread stack.   /* Supporting floating-point variables (traditional rtdbg/rt_kprintf does not support floating-point logs) */
    [*] Enable color log.                   /* Colored log */
    [*] Enable time information.            /* Time information */
    [ ]   Enable timestamp format for time. /* Including timestamp */
    [*] Enable level information.           /* Level information */
    [*] Enable tag information.             /* Label Information */
    [ ] Enable thread information.          /* Thread information */
```

### Log Level

The log level represents the importance of the log, from high to low in ulog, with the following log levels:

| Level           | Name        | Description                                                  |
| --------------- | ----------- | ------------------------------------------------------------ |
| LOG_LVL_ASSERT  | assertion   | Unhandled and fatal errors occurred, so that the system could not continue to run. These are assertion logs. |
| LOG_LVL_ERROR   | error       | The log that is output when a serious, **unrepairable** error occurs is an error level log. |
| LOG_LVL_WARNING | warning     | These warning logs are output when there are some less important errors with **repairability**. |
| LOG_LVL_INFO    | information | A log of important prompt information that is viewed by the upper-level user of the module, for example, initialization success, current working status, and so on. This level of log is generally **retained** during mass production. |
| LOG_LVL_DBG     | debug       | The debug log that is viewed by the developer of this module. This level of log is generally **closed** during mass production. |

The log level in ulog also has the following classification:

* **Static and dynamic levels**：Classify according to whether the log can be modified during the run phase. The dynamic level that can be modified during the run phase can only be called static level in the **compilation phase**. Logs that are lower than the static level (here specifically the logs using the `LOG_X` API) will not be compiled into the ROM and will not be output or displayed. The dynamic level can control logs that their level are higher than or equal to the static level. Logs that are lower than the dynamic level are filtered out when ulog is running.

* **Global level and module level**：Classification by scope. Each file (module) can also be set to a separate log level in ulog. The global level scope is larger than the module level, that is, the module level can only control module logs higher than or equal to the global level.

As can be seen from the above classification, the output level of the log can be set in the following four aspects of ulog:

* **Global static **log level：Configured in menuconfig, corresponding to the `ULOG_OUTPUT_LVL` macro.

* **Global Dynamics** log level：Use the `void ulog_global_filter_lvl_set(rt_uint32_t level)` function to set it.

* **Module static** log level：The `LOG_LVL` macro is defined in the module (file), similar to the way the log tag macro `LOG_TAG` is defined.

* **Module dynamics** log level：Use the `int ulog_tag_lvl_filter_set(const char *tag, rt_uint32_t level)` function to set it.

Their scope of action is：**Global Static**>**Global Dynamics**>**Module Static**>**Module Dynamic**.

### Log Label

Due to the increasing log output, in order to avoid the log being outputted indiscriminately, it is necessary to use a tag to classify each log. The definition of the label is in the form of **modular**, for example: Wi-Fi components include device driver (wifi_driver), device management (wifi_mgnt) and other modules, Wi-Fi component internal module can use `wifi.driver`, `wifi.mgnt` is used as a label to perform classified output of logs.

The tag attribute of each log can also be output and displayed. At the same time, ulog can also set the output level of each tag (module) corresponding to the log. The log of the current unimportant module can be selectively closed, which not only reduces ROM resources, but also helps developers filter irrelevant logs.

See the `rt-thread\examples\ulog_example.c` ulog routine file with the `LOG_TAG` macro defined at the top of the file:

```c
#define LOG_TAG     "example"     // The label corresponding to this module. When not defined, default: NO_TAG
#define LOG_LVL     LOG_LVL_DBG   // The log output level corresponding to this module. When not defined, default: debug level
#include <ulog.h>                 // this header file Must be under LOG_TAG and LOG_LVL
```

Note that the definition log tag must be above `#include <ulog.h>`, otherwise the default `NO_TAG` will be used (not recommended to define these macros in the header file).

The scope of the log tag is the current source file, and the project source code will usually be classified according to the module. Therefore, when defining a label, you can specify the module name and sub-module name as the label name. This is not only clear and intuitive when the log output is displayed, but also facilitates subsequent dynamic adjustment of the level or filtering by label.

## Log Initialization

### Initialization

```c
int ulog_init(void)
```

| **Return** | **Description**             |
| :--------- | :-------------------------- |
| >=0        | Succeeded                   |
| -5         | Failed, insufficient memory |

This function must be called to complete ulog initialization before using ulog. This function will also be called automatically if component auto-initialization is turned on.

### Deinitialization

```c
void ulog_deinit(void)
```

This deinit release resource can be executed when ulog is no longer used.

## Log Output API

Ulog mainly has two log output macro APIs, which are defined in the source code as follows:

```c
#define LOG_E(...)                           ulog_e(LOG_TAG, __VA_ARGS__)
#define LOG_W(...)                           ulog_w(LOG_TAG, __VA_ARGS__)
#define LOG_I(...)                           ulog_i(LOG_TAG, __VA_ARGS__)
#define LOG_D(...)                           ulog_d(LOG_TAG, __VA_ARGS__)
#define LOG_RAW(...)                         ulog_raw(__VA_ARGS__)
#define LOG_HEX(name, width, buf, size)      ulog_hex(name, width, buf, size)
```

* The macro `LOG_X(...)`:`X` corresponds to the first letter of the different levels. The parameter `...` is the log content, and the format is the same as printf. This method is preferred because on the one hand, because its API format is simple, only one log information is entered, and the static log level filtering by module is also supported.

* The macro `ulog_x(LOG_TAG, __VA_ARGS__)`:  `x ` corresponds to a different level of shorthand. The parameter `LOG_TAG` is the log label, the parameter `...` is the log content, and the format is the same as printf. This API is useful when you use different tag output logs in one file.

| **API**                         | **Description**                           |
| ------------------------------- | ----------------------------------------- |
| LOG_E(...)                      | Error level log                           |
| LOG_W(...)                      | Error level log                           |
| LOG_I(...)                      | Prompt level log                          |
| LOG_D(...)                      | Debug level log                           |
| LOG_RAW(...)                    | Output raw log                            |
| LOG_HEX(name, width, buf, size) | Output hexadecimal format data to the log |

API such as ` LOG_X` and `ulog_x` , the output are formatted logs. When you need to output logs without any format, you can use `LOG_RAW` or `ulog_raw()`. E.g:

```c
LOG_RAW("\r");
ulog_raw("\033[2A");
```

You can use `LOG_HEX()` or `ulog_hex` to dump data into the log in hexadecimal hex format. The function parameters and descriptions are as follows:

| **Parameter** | **Description**                             |
| ------------- | ------------------------------------------- |
| tag           | Log label                                   |
| width         | The width (number) of a line of hex content |
| buf           | Data content to be output                   |
| size          | Data size                                   |

The `hexdump` log is DEBUG level, supports runtime level filtering. The tag corresponding to the hexdump log supports tag filtering during runtime.

Ulog also provides the assertion API: `ASSERT(expression)`. When the assertion is triggered, the system will stop running, and `ulog_flush()` will be executed internally, and all log backends will execute flush. If asynchronous mode is turned on, all logs in the buffer will also be flushed. An example of the use of assertions is as follows:

```c
void show_string(const char *str)
{
    ASSERT(str);
    ...
}
```

## ULog Usage Example

### Example

The following is a description of the ulog routine. Open `rt-thread\examples\ulog_example.c` and you can see that there are labels and static priorities defined at the top.

```c
#define LOG_TAG              "example"
#define LOG_LVL              LOG_LVL_DBG
#include <ulog.h>
```

The `LOG_X` API is used in the `void ulog_example(void)` function, which is roughly as follows:

```c
/* output different level log by LOG_X API */
LOG_D("LOG_D(%d): RT-Thread is an open source IoT operating system from China.", count);
LOG_I("LOG_I(%d): RT-Thread is an open source IoT operating system from China.", count);
LOG_W("LOG_W(%d): RT-Thread is an open source IoT operating system from China.", count);
LOG_E("LOG_E(%d): RT-Thread is an open source IoT operating system from China.", count);
```

These log output APIs support the printf format and will automatically wrap lines at the end of the log.

The following will show the effect of the ulog routine on qemu:

- Copy `rt-thread\examples\ulog_example.c` to the `rt-thread\bsp\qemu-vexpress-a9\applications` folder.
- Go to the `rt-thread\bsp\qemu-vexpress-a9` directory in Env
- After determining that the configuration of ulog has been executed before, execute the `scons` command and wait for the compilation to complete.
- Run `qemu.bat` to open RT-Thread's qemu simulator
- Enter the `ulog_example` command to see the results of the ulog routine. The effect is as follows.

![ulog routine](figures/ulog_example.png)

You can see that each log is displayed in rows, and different levels of logs have different colors. At the top of the log is the tick of the current system, with the log level and label displayed in the middle, and the specific log content at the end. These log formats and configuration instructions are also highlighted later in this article.

### Used in Interrupt ISR

Many times you need to output a log in the interrupt ISR, but the ISR may interrupt the thread that is doing the log output. To ensure that the interrupt log and the thread log do not interfere with each other, special handling must be performed for the interrupt condition.

Ulog has integrated interrupt log function, but it is not enabled by default. Open the `Enable ISR log` option when using it. The API of the log is the same as that used in the thread, for example:

```c
#define LOG_TAG              "driver.timer"
#define LOG_LVL              LOG_LVL_DBG
#include <ulog.h>

void Timer2_Handler(void)
{
    /* enter interrupt */
    rt_interrupt_enter();

    LOG_D("I'm in timer2 ISR");

    /* leave interrupt */
    rt_interrupt_leave();
}

```

Here are the different strategies for interrupt logging in ulog in synchronous mode and asynchronous mode:

**In synchronous mode**：If the thread is interrupted when the log is being output at this time, and there is a log to be output in the interrupt, it will be directly output to the console, and output to other backends is not supported;

**In asynchronous mode**：If the above situation occurs, the log in the interrupt will be put into the buffer first, and finally sent to the log output thread for processing together with the thread log.

### Set the Log Format

The log format supported by ulog can be configured in menuconfig, located in `RT-Thread Components` → `Utilities` → `ulog` → `log format`. The specific configuration is as follows:

![ulog format configuration](figures/ulog_menuconfig_format.png)

They can be configured separately: floating-point number support (traditional rtdbg/rt_kprintf does not support floating-point logs), colored logs, time information (including timestamps), level information, tag information, thread information. Below we will **select all of these options**, save and recompile and run the ulog routine again in qemu to see the actual effect:

![ulog routine (figures/ulog_example_all_format.png)](figures/ulog_example_all_format.png)

It can be seen that the time information has been changed from the tick value of the system to the timestamp information compared to the first run routine, and the thread information has also been output.

### Hexdump Output Using

Hexdump is also a more common function when logging output. hexdump can output a piece of data in hex format. The corresponding API is: `void ulog_hexdump(const char *tag, rt_size_t width, rt_uint8_t *buf, rt_size_t size)` , see below the specific use method and operation effect:

```c
/* Define an array of 128 bytes in length */
uint8_t i, buf[128];
/* Fill the array with numbers */
for (i = 0; i < sizeof(buf); i++)
{
    buf[i] = i;
}
/* Dumps the data in the array in hex format with a width of 16 */
ulog_hexdump("buf_dump_test", 16, buf, sizeof(buf));
```

You can copy the above code into the ulog routine, and then look at the actual running results:

![ulog routine](figures/ulog_example_hexdump.png)

It can be seen that the middle is the hexadecimal information of the buf data, and the rightmost is the character information corresponding to each data.

## Log Advanced Features

After understanding the introduction of the log in the previous section, the basic functions of ulog can be mastered. In order to let everyone better use ulog, this application note will focus on the advanced features of ulog and some experience and skills in log debugging. After learning these advanced uses, developers can also greatly improve the efficiency of log debugging.

It also introduces the advanced mode of ulog: syslog mode, which is fully compatible with the Linux syslog from the front-end API to the log format, greatly facilitating the migration of software from Linux.

### Log Backend

![Ulog framework](figures/ulog_framework_backend.png)

Speaking of the backend, let's review the ulog's framework. As can be seen from the above figure, ulog is a design with front and back ends separated, and there is no dependence on the front and back ends. And the backends that are supported are diversified, no matter what kind of backends, as long as they are implemented, they can be registered.

Currently ulog has integrated the console backend, the traditional device that outputs `rt_kprintf` print logs. Ulog also supports the Flash backend, which seamlessly integrates with EasyFlash. See its package for details.（[Click to view](https://github.com/armink-rtt-pkgs/ulog_easyflash_be)）。Later ulog will also increase the implementation of backends such as file backends and network backends. Of course, if there are special needs, users can also implement the backend themselves.

#### Register Backend Device

```c
rt_err_t ulog_backend_register(ulog_backend_t backend, const char *name, rt_bool_t support_color)
```

| **Parameter** | **Description**                |
| :------------ | :----------------------------- |
| backend       | Backend device handle          |
| name          | Backend device name            |
| support_color | Whether it supports color logs |
| **return**    | --                             |
| >=0           | Succeeded                      |

This function is used to register the backend device into the ulog, ensuring that the function members in the backend device structure are set before registration.

#### Logout Backend Device

```c
rt_err_t ulog_backend_unregister(ulog_backend_t backend);
```

| **Parameter** | **Description**       |
| :------------ | :-------------------- |
| backend       | Backend device handle |
| **return**    | --                    |
| >=0           | Succeeded             |

This function is used to unregister a backend device that has already been registered.

#### Backend Implementation and Registration Examples

The console backend is taken as an example to briefly introduce the implementation method and registration method of the backend.

Open the `rt-thread/components/utilities/ulog/backend/console_be.c` file and you can see the following:

```c
#include <rthw.h>
#include <ulog.h>

/* Defining console backend devices */
static struct ulog_backend console;
/* Console backend output function */
void ulog_console_backend_output(struct ulog_backend *backend, rt_uint32_t level, const char *tag, rt_bool_t is_raw, const char *log, size_t len)
{
    ...
    /* Output log to the console */
    ...
}
/* Console backend initialization */
int ulog_console_backend_init(void)
{
    /* Set output function */
    console.output = ulog_console_backend_output;
    /* Registration backend */
    ulog_backend_register(&console, "console", RT_TRUE);

    return 0;
}
INIT_COMPONENT_EXPORT(ulog_console_backend_init);
```

Through the above code, it can be seen that the implementation of the console backend is very simple. Here, the `output` function of the backend device is implemented, and the backend is registered in the ulog, and then the log of ulog is output to the console.

If you want to implement a more complex back-end device, you need to understand the back-end device structure, as follows:

```c
struct ulog_backend
{
    char name[RT_NAME_MAX];
    rt_bool_t support_color;
    void (*init)  (struct ulog_backend *backend);
    void (*output)(struct ulog_backend *backend, rt_uint32_t level, const char *tag, rt_bool_t is_raw, const char *log, size_t len);
    void (*flush) (struct ulog_backend *backend);
    void (*deinit)(struct ulog_backend *backend);
    rt_slist_t list;
};
```

From the perspective of this structure, the requirements for implementing the backend device are as follows:

* `The name` and `support_color` properties can be passed in through the `ulog_backend_register()` function.

* `output` is the back-end specific output function, and all backends must implement the interface.

* `init`/`deinit` is optional, `init` is called at `register`, and `deinit` is called at `ulog_deinit`.

* `flush` is also optional, and some internal output cached backends need to implement this interface. For example, some file systems with RAM cache. The flush of the backend is usually called by `ulog_flush` in the case of an exception such as assertion or hardfault.

### Asynchronous Log

In ulog, the default output mode is synchronous mode, and in many scenarios users may also need asynchronous mode. When the user calls the log output API, the log is cached in the buffer, and the thread dedicated to the log output takes out the log and outputs it to the back end.

Asynchronous mode and synchronous mode are the same for the user, there is no difference in the use of the log API, because ulog will distinguish between the underlying processing. The difference between the two works is as follows:

![ulog asynchronous VS synchronization](figures/ulog_async_vs_sync.png)

The advantages and disadvantages of asynchronous mode are as follows:

**Advantage**：

* First, the log output will not block the current thread, and some backend output rates are low, so using the synchronous output mode may affect the timing of the current thread. The asynchronous mode does not have this problem.

* Secondly, since each thread that uses the log omits the action of the backend output, the stack overhead of these threads may also be reduced, and from this perspective, the resource consumption of the entire system can also be reduced.

* Interrupt logs in synchronous mode can only be output to the console backend, while in asynchronous mode interrupt logs can be output to all backends.

**Disadvantage**：First, the asynchronous mode requires a log buffer. Furthermore, the output of the asynchronous log needs to be completed by a special thread, such as an idle thread or a user-defined thread, which is slightly more complicated to use. The overall sense of asynchronous mode resource occupancy will be higher than the synchronous mode.

#### Configuration Option

Use menuconfig in the Env tool to enter the ulog configuration options:

```c
 RT-Thread Components → Utilities → Enable ulog
```

The asynchronous mode related configuration options are described as follows:

```c
[*]   Enable async output mode.                 /* Enable asynchronous mode */
(2048)  The async output buffer size.           /* Asynchronous buffer size, default is 2048*/
[*]     Enable async output by thread.          /* Whether to enable the asynchronous log output thread in ulog, the thread will wait for log notification when it runs, and then output the log to all backends. This option is turned on by default, and can be turned off if you want to modify it to another thread, such as an idle thread. */
(1024)    The async output thread stack size.   /* Asynchronous output thread stack size, default is 1024 */
(30)      The async output thread stack priority./* The priority of the asynchronous output thread, the default is 30*/
```

When using the idle thread output, the implementation is simple, just call `rt_thread_idle_sethook(ulog_async_output)` at the application layer, but there are some limitations.

* The idle thread stack size needs to be adjusted based on actual backend usage.

* Because thread suspend operations are not allowed inside idle threads, backends such as Flash and networking may not be available based on idle threads.

#### Use Example

Save the asynchronous output option configuration and copy `rt-thread\examples\ulog_example.c` to the `rt-thread\bsp\qemu-vexpress-a9\applications` folder.

Execute the `scons` command and wait for the compilation to complete. Run `qemu.bat` to open the qemu emulator for RT-Thread.
Enter the `ulog_example` command to see the results of the ulog routine. The approximate effect is as follows:

![ulog asynchronous routine](figures/ulog_example_async.png)

If you look carefully, you can see that after the asynchronous mode is turned on, the time information of these logs that are very close in code is almost the same. However, in synchronous mode, the log is output using the user thread. Since the log output takes a certain amount of time, there is a certain interval between each log. It also fully shows that the asynchronous log output is very efficient, and it takes almost no time for the caller.

### Log Dynamic Filter

In the previous section, some static filtering functions have been introduced. Static filtering has its advantages such as saving resources, but in many cases, users need to dynamically adjust the filtering mode of the log while the software is running. This allows the dynamic filter function of ulog to be used. To use the dynamic filter feature, turn on the `Enable runtime log filter.` option in menuconfig, which is **turned off by default**.

There are four types of dynamic filtering supported by ulog, and there are corresponding API functions and Finsh/MSH commands, which will be introduced one by one.

#### Filter by Module Level

```c
int ulog_tag_lvl_filter_set(const char *tag, rt_uint32_t level)
```

| **Parameter** | **Description**           |
| ------------- | ------------------------- |
| tag           | Log label                 |
| level         | Set log level             |
| **return**    | --                        |
| >=0           | Succeeded                 |
| -5            | Failed, not enough memory |

* Command format： `ulog_tag_lvl <tag> <level>`

The **module** referred to here represents a class of log code with the same tag attributes. Sometimes it is necessary to dynamically modify the log output level of a module at runtime.

The parameter log `level` can take the following values:

| **Level**             | **Name**             |
| --------------------- | -------------------- |
| LOG_LVL_ASSERT        | Assertion            |
| LOG_LVL_ERROR         | Error                |
| LOG_LVL_WARNING       | Warning              |
| LOG_LVL_INFO          | Information          |
| LOG_LVL_DBG           | Debug                |
| LOG_FILTER_LVL_SILENT | Silent (stop output) |
| LOG_FILTER_LVL_ALL    | All                  |

An example of a function call and command is as follows:

| Function                                   | Function Call                                             | Execute an order      |
| ------------------------------------------ | --------------------------------------------------------- | --------------------- |
| Close all logs of `wifi` module            | `ulog_tag_lvl_filter_set("wifi", LOG_FILTER_LVL_SILENT);` | `ulog_tag_lvl wifi 0` |
| Open all logs of `wifi` module             | `ulog_tag_lvl_filter_set("wifi", LOG_FILTER_LVL_ALL);`    | `ulog_tag_lvl wifi 7` |
| Set the `wifi` module log level to warning | `ulog_tag_lvl_filter_set("wifi", LOG_LVL_WARNING);`       | `ulog_tag_lvl wifi 4` |

#### Global Filtering by Label

```c
void ulog_global_filter_tag_set(const char *tag)
```

| **Parameter** | **Description**  |
| :------------ | :--------------- |
| tag           | Set filter label |

* The command format: `ulog_tag [tag]`, when the tag is empty, the label filtering is canceled.

This filtering method can perform label filtering on all logs, and only the **log containing the label information** is allowed to output.

For example: there are 3 kinds of tags for `wifi.driver`, `wifi.mgnt`, `audio.driver`. When setting the filter tag to `wifi`, only the tags are `wifi.driver` and `wifi.mgnt. The log of ` will be output. Similarly, when the filter tag is set to `driver`, only logs with tags `wifi.driver` and `audio.driver` will be output. Examples of function calls and commands for common functions are as follows:

| Function                       | Function Call                           | Execute an Order  |
| ------------------------------ | --------------------------------------- | ----------------- |
| Set the filter tag to `wifi`   | `ulog_global_filter_tag_set("wifi");`   | `ulog_tag wifi`   |
| Set the filter tag to `driver` | `ulog_global_filter_tag_set("driver");` | `ulog_tag driver` |
| Cancel label filtering         | `ulog_global_filter_tag_set("");`       | `ulog_tag`        |

#### Global Filtering by Level

```c
void ulog_global_filter_lvl_set(rt_uint32_t level)
```

| **Parameter** | **Description** |
| ------------- | --------------- |
| level         | Set log level   |

* Command format: `ulog_lvl <level>` , level Refer to the following table:

| **Value** | **Description** |
| :-------- | :-------------- |
| 0         | assertion       |
| 3         | error           |
| 4         | warning         |
| 6         | information     |
| 7         | debug           |

After setting the global filter level by function or command, the log below **setting level** will stop output. Examples of function calls and commands for common functions are as follows:

| Function                     | Function Call                                        | Execute an order |
| ---------------------------- | ---------------------------------------------------- | ---------------- |
| Close all logs               | `ulog_global_filter_lvl_set(LOG_FILTER_LVL_SILENT);` | `ulog_lvl 0`     |
| Open all logs                | `ulog_global_filter_lvl_set(LOG_FILTER_LVL_ALL);`    | `ulog_lvl 7`     |
| Set the log level to warning | `ulog_global_filter_lvl_set(LOG_LVL_WARNING);`       | `ulog_lvl 4`     |

#### Global Filtering by Keyword

```c
void ulog_global_filter_kw_set(const char *keyword)
```

| **Parameter** | **Description**     |
| :------------ | :------------------ |
| keyword       | Set filter keywords |

* The command format: `ulog_kw [keyword]`, when the keyword is empty, the keyword filtering is canceled.

This filtering method can filter all the logs by keyword, and the log **containing the keyword information** is allowed to output. Examples of function calls and commands for common functions are as follows:

| Function                         | Function Call                        | Execute an order |
| -------------------------------- | ------------------------------------ | ---------------- |
| Set the filter keyword to `wifi` | `ulog_global_filter_kw_set("wifi");` | `ulog_kw wifi`   |
| Clear filter keywords            | `ulog_global_filter_kw_set("");`     | `ulog_kw`        |

#### View Filter Information

After setting the filter parameters, if you want to view the current filter information, you can enter the `ulog_filter` command. The approximate effect is as follows:

```c
msh />ulog_filter
--------------------------------------
ulog global filter:
level   : Debug
tag     : NULL
keyword : NULL
--------------------------------------
ulog tag's level filter:
wifi                   : Warning
audio.driver           : Error
msh />
```

> Filter parameters are also supported storing in Flash and also support boot autoload configuration. If you need this feature, please see the instructions for the **ulog_easyflash** package.（[Click to check](https://github.com/armink-rtt-pkgs/ulog_easyflash_be)）

#### Use Example

Still executing in qemu BSP, first open dynamic filter in menuconfig, then save the configuration and compile and run the routine. After the log output is about **20** times, the corresponding filter code in ulog_example.c will be executed:

```c
if (count == 20)
{
    /* Set the global filer level is INFO. All of DEBUG log will stop output */
    ulog_global_filter_lvl_set(LOG_LVL_INFO);
    /* Set the test tag's level filter's level is ERROR. The DEBUG, INFO, WARNING log will stop output. */
    ulog_tag_lvl_filter_set("test", LOG_LVL_ERROR);
}
...
```

At this point, the global filter level is set to the INFO level, so you can no longer see logs lower than the INFO level. At the same time, the log output level of the `test` tag is set to ERROR, and the log lower than ERROR in the `test` tag is also stopped. In each log, there is a count value of the current log output count. The effect of the comparison is as follows:

![ulog filter routine 20](figures/ulog_example_filter20.png)

After the log output is about **30** times, the following filter code corresponding to ulog_example.c is executed:

```c
...
else if (count == 30)
{
    /* Set the example tag's level filter's level is LOG_FILTER_LVL_SILENT, the log enter silent mode. */
    ulog_tag_lvl_filter_set("example", LOG_FILTER_LVL_SILENT);
    /* Set the test tag's level filter's level is WARNING. The DEBUG, INFO log will stop output. */
    ulog_tag_lvl_filter_set("test", LOG_LVL_WARNING);
}
...
```

At this point, the filter of the `example` module has been added, and all the logs of this module are stopped, so the module log will not be visible next. At the same time, reduce the log output level of the `test` tag to WARING. At this point, you can only see the WARING and ERROR level logs of the `test` tag. The effect is as follows:

![ulog filter routine 30](figures/ulog_example_filter30.png)

After the log output is about **40** times, the following filter code corresponding to ulog_example.c is executed:

```c
...
else if (count == 40)
{
    /* Set the test tag's level filter's level is LOG_FILTER_LVL_ALL. All level log will resume output. */
    ulog_tag_lvl_filter_set("test", LOG_FILTER_LVL_ALL);
    /* Set the global filer level is LOG_FILTER_LVL_ALL. All level log will resume output */
    ulog_global_filter_lvl_set(LOG_FILTER_LVL_ALL);
}
```

At this time, the log output level of the `test` module is adjusted to `LOG_FILTER_LVL_ALL`, that is, the log of any level of the module is no longer filtered. At the same time, the global filter level is set to `LOG_FILTER_LVL_ALL`, so all the logs of the `test` module will resume output. The effect is as follows:

![ulog filter routine 40](figures/ulog_example_filter40.png)

### Usage when the System is Abnormal

Since the asynchronous mode of ulog has a caching mechanism, the registered backend may also have a cache inside. If there are error conditions such as hardfault and assertion in the system, but there are still logs in the cache that are not output, which may cause the log to be lost. It is impossible to find the cause of the exception.

For this scenario, ulog provides a unified log flush function: `void ulog_flush(void)`. When an exception occurs, when the exception information log is output, the function is called at the same time to ensure that the remaining logs in the cache can also be output to the back end.

The following is an example of RT-Thread assertion and CmBacktrace:

#### Assertion

RT-Thread assertions support assertion callbacks. We define an assertion hook function similar to the following, and then set it to the system via the `rt_assert_set_hook(rtt_user_assert_hook);` function.

```c
static void rtt_user_assert_hook(const char* ex, const char* func, rt_size_t line)
{
    rt_enter_critical();

    ulog_output(LOG_LVL_ASSERT, "rtt", RT_TRUE, "(%s) has assert failed at %s:%ld.", ex, func, line);
    /* flush all log */
    ulog_flush();
    while(1);
}
```

#### CmBacktrace

CmBacktrace is a fault diagnosis library for ARM Cortex-M series MCUs. It also has a corresponding RT-Thread package, and the latest version of the package has been adapted for ulog. The adaptation code is located in `cmb_cfg.h` :

```c
...
/* print line, must config by user */
#include <rtthread.h>
#ifndef RT_USING_ULOG
#define cmb_println(...)               rt_kprintf(__VA_ARGS__);rt_kprintf("\r\n")
#else
#include <ulog.h>
#define cmb_println(...)               ulog_e("cmb", __VA_ARGS__);ulog_flush()
#endif /* RT_USING_ULOG */
...
```

It can be seen that when ulog is enabled, each log output of CmBacktrace will use the error level, and `ulog_flush` will be executed at the same time, and the user does not need to make any modifications.

### Syslog Mode

On Unix-like operating systems, syslog is widely used in system logs. The common backends of syslog are files and networks. The syslog logs can be recorded in local files or sent over the network to the server that receives the syslog.

Ulog provides support for the syslog mode, not only the front-end API is exactly the same as the syslog API, but the log format is also RFC compliant. However, it should be noted that after the syslog mode is enabled, the log output format of the entire ulog will be in syslog format regardless of which log output API is used.

To use the syslog configuration you need to enable the `Enable syslog format log and API.` option.

#### Log Format

![ulog syslog format](figures/ulog_syslog_format.png)

As shown in the figure above, the ulog syslog log format is divided into the following four parts:

| Format  | **Description**                                              |
| ------- | ------------------------------------------------------------ |
| PRI     | The PRI part consists of a number enclosed in angle brackets. This number contains the Facility and Severity information, which is made by Facility multiplying 8 and then adding Severity. Facility and Severity are passed in by the syslog function. For details, see syslog.h. |
| Header  | The Header part is mainly a timestamp indicating the time of the current log; |
| TAG     | The current log label can be passed in via the `openlog` function. If not specified, `rtt` will be used as the default label. |
| Content | The specific content of the log                              |

#### Instruction

The syslog option needs to be enabled in menuconfig before use. The main commonly used APIs are:

* Open syslog：`void openlog(const char *ident, int option, int facility)`

* Output syslog log：`void syslog(int priority, const char *format, ...)`

> Hint: Calling `openlog` is optional. If you do not call `openlog`, the openlog is automatically called when syslog is called for the first time.


syslog() is very simple to use. The input format is the same as the printf function. There are also syslog routines in ulog_example.c that work as follows in qemu:

![ulog syslog routine](figures/ulog_example_syslog.png)

### Migrate from *rt_dbg.h* or elog to ulog

If the two types of log components were used in the project before, when ulog is to be used, it will involve how to make the previous code also support ulog. The following will focus on the migration process.

#### Migrate from rt_dbg.h

Currently rtdbg has completed **seamless docking** ulog. After ulog is turned on, the code of rtdbg in the old project can be used to complete the log output without any modification.

#### Migrate from Elog(EasyLogger)

If you are not sure that a source code file is running on a target platform that will use ulog, then it is recommended to add the following changes to the file:

```c
#ifdef RT_USING_ULOG
#include <ulog.h>
#else
#include <elog.h>
#endif /* RT_USING_ULOG */
```

If you explicitly only use the ulog component, then simply change the header file reference from `elog.h` to `ulog .h`, and no other code needs to be changed.

### Log Usage Tip

With the logging tool, if it is used improperly, it will also cause the log to be abused, and the log information cannot be highlighted. Here we will focus on sharing some tips on the use of the log component to make the log information more intuitive. The main concerns are:

#### Rational Use of Label Classification

Reasonably use the label function. For each module code before the use of the log, first clear the module, sub-module name. This also allows the logs to be categorized at the very beginning and ready for later log filtering.

#### Rational Use of Log Levels

When you first use the log library, you will often encounter warnings and errors. The logs cannot be distinguished. The information cannot be distinguished from the debug logs, which makes the log level selection inappropriate. Some important logs may not be visible, and unimportant logs are full of problems. Therefore, be sure to read the log level section carefully before using it. For each level, there are clear standards.

#### Avoid Repetitive and Redundant Logs

In some cases, repeated or cyclic execution of code occurs, and the same, similar log problems are output multiple times. Such a log not only takes up a lot of system resources, but also affects the developer's positioning of the problem. Therefore, in this case, it is recommended to add special processing for repetitive logs, such as: let the upper layer output some business-related logs, the bottom layer only returns the specific result status; the same log at the same time point, can increase to deal with the re-processing, only output once and so on when the error state has not changed.

#### Open More Log Formats

The timestamp and thread information are not turned on in the default log format of ulog. These two log messages are useful on RTOS. They can help developers to understand the running time and time difference of each log, and clearly see which thread is executing the current code. So if conditions permit, it is still recommended to open.

#### Close Unimportant Logs

Ulog provides log switch and filtering functions in various dimensions, which can completely control the refinement. Therefore, if you debug a function module, you can properly close the log output of other unrelated modules, so that you can focus on the current debugging on the module.

## Common Problems

### Q: The log code has been executed, but there is no output.

 **A:** Refer to the Log Levels section for the log level classification and check the log filtering parameters. There is also the possibility of accidentally closing the console backend and re-opening `Enable console backend`.

### Q: After ulog is turned on, the system crashes, for example: thread stack overflow.

 **A:** Ulog will occupy more part of the thread stack space than the previous rtdbg or `rt_kprintf` printout function. If floating-point printing support is enabled, it is recommended because it uses the internal memory of libc with a large amount of `vsnprintf`. Reserve more than 250 bytes. If the timestamp function is turned on, the stack recommends a reserve of 100 bytes.

### Q: The end of the log content is missing.

 **A:** This is because the log content exceeds the maximum width of the set log. Check the `The log's max width` option and increase it to the appropriate size.

### Q: After turning on the timestamp, why can't I see the millisecond time?

 **A:** This is because ulog currently only supports millisecond timestamps when the software emulation RTC state is turned on. To display, just turn on the RT-Thread software to simulate the RTC function.

### Q: Define LOG_TAG and LOG_LVL before each include ulog header file, can it be simplified?

**A:** If `LOG_TAG` is not defined, the `NO_TAG` tag will be used by default, so the output log will be easily misunderstood. Therefore the tag macro is not recommended to be omitted.

If `LOG_LVL` is not defined, the debug level is used by default. If the module is in the development stage, this process can be omitted, but if the module code is stable, it is recommended to define the macro and modify the level to information level.

### Q: Warning when running：Warning: There is no enough buffer for saving async log, please increase the ULOG_ASYNC_OUTPUT_BUF_SIZE option。

**A:** When this prompt is encountered, it indicates that the buffer in the asynchronous mode has overflowed, which will cause some of the log to be lost. Increasing the ULOG_ASYNC_OUTPUT_BUF_SIZE option can solve the problem.

### Q: Compile time prompt：The idle thread stack size must more than 384 when using async output by idle (ULOG_ASYNC_OUTPUT_BY_IDLE)。

**A:** When using an idle thread as the output thread, the stack size of the idle thread needs to be increased, depending on the specific backend device. For example, when the console being a backend, the idle thread must be at least 384 bytes.

# utest Framework

## utest Introduction

utest (unit test) is a unit testing framework developed by RT-Thread. The original intention of designing utest is to make it easier for RT-Thread developers to write test programs using a unified framework interface for unit testing, coverage testing, and integration testing.

### Test Case Definition

A test case (tc) is a single test performed to achieve a specific test objective. It is a specification that includes test input, execution conditions, test procedures, and expected results. It is a infinite loop with clear end conditions and test results.

The utest (unit test) framework defines user-written test programs as **test cases**, and a test case contains only one *testcase* function (similar to the main function), which can contain multiple *test unit* functions.

The test code for a function, specifically through the API provided by the utest framework, is a test case.

### Test Unit Definition

The test unit is a test point subdivided by the function to be tested. Each test point can be the smallest measurable unit of the function to be tested. Of course, different classification methods will subdivide different test units.

### utest Application Block Diagram

![utest Application Block Diagram](figures/UtestAppStruct-1.png)

As shown in the figure above, the test case is designed based on the service interface provided by the test framework utest, which supports compiling multiple test cases together for testing. In addition, as you can see from the figure, a test case corresponds to a unique *testcase* function, and multiple test units are included in *testcase*.

## utest API

To enable uniform test case code, the test framework utest provides a common API interface for test case writing.

### Macros of assertion

> NOTE:
> Here assert only records the number of passes and failures, it does not generate assertions or terminates program execution. Its function is not equivalent to RT_ASSERT.


| assert Macro                          | Description                                                  |
| :------------------------------------ | :----------------------------------------------------------- |
| uassert_true(value)                   | If the value is true then the test passes, otherwise the test fails. |
| uassert_false(value)                  | If the value is false then the test passes, otherwise the test fails. |
| uassert_null(value)                   | If the Value is null then the test passes, otherwise the test fails |
| uassert_not_null(value)               | If the value is a non-null value, the test passes, otherwise the test fails. |
| uassert_int_equal(a, b)               | If the values of a and b are equal, the test passes, otherwise the test fails. |
| uassert_int_not_equal(a, b)           | If the values of a and b are not equal, the test passes, otherwise the test fails. |
| uassert_str_equal(a, b)               | If the string a and the string b are the same, the test passes, otherwise the test fails. |
| uassert_str_not_equal(a, b)           | If the string a and the string b are not the same, the test passes, otherwise the test fails. |
| uassert_in_range(value, min, max)     | If the value is in the range of min and max, the test passes, otherwise the test fails. |
| uassert_not_in_range(value, min, max) | If the value is not in the range of min and max, the test passes, otherwise the test fails. |

### Macros for Running Test Units

```c
UTEST_UNIT_RUN(test_unit_func)
```

In the test case, the specified test unit function `test_unit_func` is executed using the `UTEST_UNIT_RUN` macro. The test unit must be executed using the `UTEST_UNIT_RUN` macro.

### Macros for Exporting Test Cases

```c
UTEST_TC_EXPORT(testcase, name, init, cleanup, timeout)
```

| Parameters | Description                                                  |
| :--------- | :----------------------------------------------------------- |
| testcase   | Test case main-bearing function (**specifies** using a function called *static void testcase(void)* |
| name       | Test case name (uniqueness). Specifies the naming format for connecting relative names of test cases relative to `testcases directory` with `.` |
| init       | the initialization function before Test case startup         |
| cleanup    | Cleanup function after the end of the test case              |
| timeout    | Test case expected test time (in seconds)                    |

**Test case naming requirements:**

Test cases need to be named in the prescribed format. Specifies the naming format for the connection of the current test case relative to the `testcases directory ` linked with  `.` . The name contains the file name of the current test case file (the file name except the suffix name).

**Test case naming example:**

Assuming that there is a `testcases\components\filesystem\dfs\dfs_api_tc.c` test case file in the test case `testcases` directory, the test case name in the `dfs_api_tc.c` is named `components.filesystem.dfs.dfs_api_tc`.

### Test Case LOG Output Interface

The utest framework relies on the *ulog log module* for log output and the log output level in the utest framework. So just add `#include "utest.h"` to the test case to use all level interfaces (LOG_D/LOG_I/LOG_E) of the ulog log module.

In addition, the utest framework adds an additional log control interface as follows:

```c
#define UTEST_LOG_ALL    (1u)
#define UTEST_LOG_ASSERT (2u)

void utest_log_lv_set(rt_uint8_t lv);
```

Users can use the `utest_log_lv_set` interface to control the log output level in test cases. The `UTEST_LOG_ALL` configuration outputs all logs, and the `UTEST_LOG_ASSERT` configuration only outputs logs after the failure of uassert.

## Configuration Enable

Using the utest framework requires the following configuration in the ENV tool using menuconfig:

```c
RT-Thread Kernel  --->
    Kernel Device Object  --->
        (256) the buffer size for console log printf /* The minimum buffer required by the utest log */
RT-Thread Components  --->
    Utilities  --->
        -*- Enable utest (RT-Thread test framework) /* Enable utest framework */
        (4096) The utest thread stack size          /* Set the utest thread stack (required for -thread mode) */
        (20)   The utest thread priority            /* Set utest thread priority (required for -thread mode) */
```

## Application Paradigm

The utest framework and related APIs were introduced earlier. The basic test case code structure is described here.

The code blocks necessary for the test case file are as follows:

```c
/*
 * Copyright (c) 2006-2019, RT-Thread Development Team
 *
 * SPDX-License-Identifier: Apache-2.0
 *
 * Change Logs:
 * Date           Author       Notes
 * 2019-01-16     MurphyZhao   the first version
 */

#include <rtthread.h>
#include "utest.h"

static void test_xxx(void)
{
    uassert_true(1);
}

static rt_err_t utest_tc_init(void)
{
    return RT_EOK;
}

static rt_err_t utest_tc_cleanup(void)
{
    return RT_EOK;
}

static void testcase(void)
{
    UTEST_UNIT_RUN(test_xxx);
}
UTEST_TC_EXPORT(testcase, "components.utilities.utest.sample.sample_tc", utest_tc_init, utest_tc_cleanup, 10);
```

A basic test case must contain the following:

- File comment header (Copyright)

  The test case file must contain a file comment header containing `Copyright`, time, author, and description information.

- utest_tc_init(void)

  The initialization function before the test run is generally used to initialize the environment required for the test.

- utest_tc_cleanup(void)

  The cleanup function after the test is used to clean up the resources (such as memory, threads, semaphores, etc.) applied during the test.

- testcase(void)

  The mainly function of testcase, a test case implementation can only contain one testcase function (similar to the main function). Usually this function is only used to run the test unit execution function `UTEST_UNIT_RUN`.

  A testcase can contain multiple test units, each of which is executed by `UTEST_UNIT_RUN`.

- UTEST_UNIT_RUN

  Test unit execution function.

- test_xxx(void)

  Test implementation of each functional unit. The user determines the function name and function implementation based on the requirements.

- uassert_true

  The assertion macro used to determine the test result (this assertion macro does not terminate the program run). Test cases must use the `uassert_xxx` macro to determine the test results, otherwise the test framework does not know if the test passed.

  After all the `uassert_xxx` macros have been passed, the entire test case is passed.

- UTEST_TC_EXPORT

  Export the test case testcase function to the test framework.

## Requirements for running test cases

The test framework utest exports all test cases to the `UtestTcTab` code segment. The `UtestTcTab` section is not required to be defined in the link script in the IAR and MDK compilers, but it needs to be explicitly set in the link script when GCC is compiled.

Therefore, in order for test cases to be compiled and run under GCC, the `UtestTcTab` code segment must be defined in the *link script* of GCC.

In the `.text` of the GCC link script, add the definition of the `UtestTcTab` section in the following format:

```c
/* section information for utest */
. = ALIGN(4);
__rt_utest_tc_tab_start = .;
KEEP(*(UtestTcTab))
__rt_utest_tc_tab_end = .;
```

## Running Test Cases

The test framework provides the following commands to make it easy for users to run test cases on the RT-Thread MSH command line. The commands are as follows:

***utest_list* command**

Lists the test cases supported by the current system, including the name of the test case and the time required for the test. This command has no parameters.

***utest_run* command**

Test case execution command, the format of the command is as follows:

```c
utest_run [-thread or -help] [testcase name] [loop num]
```

| utest_run Command Parameters | Description                                                  |
| :--------------------------- | :----------------------------------------------------------- |
| -thread                      | Run the test framework in threaded mode                      |
| -help                        | Print help information                                       |
| testcase name                | Specify the test case name. Using the wildcard `*` is supported, specifying front byte of the test case name is supported. |
| loop num                     | the number of iterations of test cases                       |

**Example of test command usage:**

```c
msh />utest_list
[14875] I/utest: Commands list :
[14879] I/utest: [testcase name]:components.filesystem.dfs.dfs_api_tc; [run timeout]:30
[14889] I/utest: [testcase name]:components.filesystem.posix.posix_api_tc; [run timeout]:30
[14899] I/utest: [testcase name]:packages.iot.netutils.iperf.iperf_tc; [run timeout]:30
msh />
msh />utest_run components.filesystem.dfs.dfs_api_tc
[83706] I/utest: [==========] [ utest    ] started
[83712] I/utest: [----------] [ testcase ] (components.filesystem.dfs.dfs_api_tc) started
[83721] I/testcase: in testcase func...
[84615] D/utest: [    OK    ] [ unit     ] (test_mkfs:26) is passed
[84624] D/testcase: dfs mount rst: 0
[84628] D/utest: [    OK    ] [ unit     ] (test_dfs_mount:35) is passed
[84639] D/utest: [    OK    ] [ unit     ] (test_dfs_open:40) is passed
[84762] D/utest: [    OK    ] [ unit     ] (test_dfs_write:74) is passed
[84770] D/utest: [    OK    ] [ unit     ] (test_dfs_read:113) is passed
[85116] D/utest: [    OK    ] [ unit     ] (test_dfs_close:118) is passed
[85123] I/utest: [  PASSED  ] [ result   ] testcase (components.filesystem.dfs.dfs_api_tc)
[85133] I/utest: [----------] [ testcase ] (components.filesystem.dfs.dfs_api_tc) finished
[85143] I/utest: [==========] [ utest    ] finished
msh />
```

### Test result analysis

![utest log display](figures/UtestRunLogShow.png)

As shown in the figure above, the log of the test case run is divided into four columns from left to right, which are `(1) log header information`, `(2) result bar`, `(3) property bar`, and `(4) detail information display bar`. The test case test result (PASSED or FAILED) is identified in the log using the `result` attribute.

## Test Case Run Process

![Test Case Run Process](figures/testcase-runflowchart.jpg)

From the above flow chart you can get the following:

* The utest framework is a sequential execution of all **test units** in the *testcase* function
* Assert of the previous UTEST_UNIT_RUN macro has occurred, and all subsequent UTEST_UNIT_RUN will skip execution.

## NOTE

- Determine whether the link script has the `UtestTcTab` section added before compiling with GCC.
- Make sure `RT-Thread Kernel -> Kernel Device Object -> (256) the buffer size for console log printf` is at least 256 bytes before compiling.
- The resources (threads, semaphores, timers, memory, etc.) created in the test case need to be released before the test ends.
- A test case implementation can only export a test body function (testcase function) using `UTEST_TC_EXPORT`
- Write a `README.md` document for the your test case to guide the user through configuring the test environment.



# Online packages

The software packages offered by RT-Thread are primarily classified into the following categories: IOT, Peripherals, Systems, Programming Languages, Tools, Miscellaneous, Multimedia, Security, Embedded AI, Signal Processing & Control, and RTDuino.

The Internet of Things (IoT) encompasses a range of software packages, including those related to networks, cloud access, and other aspects of digital connectivity.
The Peripherals category encompasses a range of software packages that facilitate the interaction between the system and external devices. The packages are related to the underlying peripheral hardware and include sensor packages.
- System: System-level software packages are designed to monitor system behaviour and manage file systems.
- Programming languages: a variety of programming languages, scripts, or interpreters that can be executed on the terminal card.
- Tools: A selection of tools and software packages for supplementary use.
- Miscellaneous: Packages that do not fall into any of the aforementioned categories, including demonstrations and examples.
- Multimedia: audio and video packages on RT-Thread.
- Security: encryption algorithms and secure transport layer.
- Embedded AI: RT-Thread embedded AI software packages.
- RTDuino: Arduino ecosystem library core project, which can be directly run on RT-Thread through RTDuino.

