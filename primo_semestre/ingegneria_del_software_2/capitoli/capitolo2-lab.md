lib statica e dinamici, rileggere perché non ho capito una sega porcodio. capire cosa sono, mostrare i comandi e capire la differenza tra statici e dinamici e i loro caso d'utilizzo.

mostrare gli esempi pratici.

cosa fa makefile

esempio di makefile qua sotto
```
starwars: starwars.so starwars.o
    gcc -o starwars starwars.o -L. -lstarwars
    
libstarwars.so: course.o student.o
    ld -shared -o libstarwars.so course.o student.o
    
starwars.o: starwars.o starwars.h
   gcc -c -o starwars.o starwars.c
    
course.o: course.c course.h
    gcc -c -o course.o course.c
    
student.o: student.c student.h
    gcc -c -o student.o student.c
    
```

ma a scrivere a mano tutte le volte è una rottura

=>

```
starwars: libstarwars.so starwars.o
	gcc -o $@ starwars.o -L. -lstarwars

libstarwars.so: course.o student.o
    ls -shared -o $@ $^

%.o: %.c
	gcc -c -o $^ $@
```


poi puoi fare anche la pulizia sempre nel file make

```
clean:
	
```