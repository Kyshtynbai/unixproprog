## Код apue.h для книги UNIX профессиональное программирование СОБРАН и ИСПРАВЛЕН ##
просто сделать
``` 
sudo cp apue.3e/include/apue.h /usr/include/
sudo cp apue.3e/lib/libapue.a /usr/local/lib
```
код собирать командой
``` 
gcc -lapue siski.c
```
## Кто хочет пересобрать ##
1. Скачать исходный код отсюда:
http://www.apuebook.com/code3e.html
2. Добавить в файл `filedir/devrdev.c` код
```
#ifdef LINUX
#include <sys/sysmacros.h>
#endif
```
3. В файле buf.c заменить все строки с 90 на:
```
#ifdef _LP64
#define _flag __pad[4]
#define _ptr __pad[1]
#define _base __pad[2]
#endif

int
is_unbuffered(FILE *fp)
{
	return(fp->_flags & _IONBF);
}

int
is_linebuffered(FILE *fp)
{
	return(fp->_flags & _IOLBF);
}

int
buffer_size(FILE *fp)
{
#ifdef _LP64
/*	return(fp->_base - fp->_ptr); */
	return(fp->_IO_buf_end - fp->_IO_buf_base);
#else
	return(BUFSIZ);	/* just a guess */
#endif
}

#else

#error unknown stdio implementation!

#endif
```



## источники ##
https://www.reddit.com/r/C_Programming/comments/v6zmz2/using_apue_package/
https://stackoverflow.com/questions/55770771/how-to-fix-struct-file-has-no-member-named-pad-error-when-using-make-in
