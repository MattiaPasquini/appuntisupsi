# Ingegneria del Software 2

**Data lezione**: 23.09.2025

**Autori**: Pasqui

[back](./../index.md)

# Capitolo 2 LAB:  Librerie statiche e dinamiche

## Librerie statiche vs dinamiche

### Statiche (.a)
Semplicemente un archivio di files oggetto (.o) compressi insieme. Il codice viene copiato direttamente nell'eseguibile durante la compilazione
#### Vantaggi
Eseguibile autonomo (non serve altro per funzionare) ed è più veloce all'avvio
#### Svantaggi
Eseguibile più grande e se aggiorni la libreria, va ricompilato tutto.

### Dinamiche (.so)
Sono i file che contengono codice condivisibile. L'eseguibile contiene solo **riferimenti** al codice, che viene caricato a runtime.
#### Vantaggi
Eseguibile più piccolo. Più programmi possono condividere la stessa libreria. Puoi aggiornare la libreria senza ricompilare
#### Svantaggi
Bisogna avere la libreria installata sul sistema. Leggermente più lento all'avvio.

## Comandi pratici
### Come creare libreria statica:
```bash
# Compila i file oggetto
gcc -c -o course.o course.c
gcc -c -o student.o student.c


# Crea la libreria statica 
ar rcs libstarwars.a course.o student.o 

# Compila il main
gcc -c -o starwars.o starwars.c

# Linka il main con la libreria statica
gcc starwars.o libstarwars.a -o starwars
```

### Creare libreria dinamica
```bash
# Compila i file oggetto (con -fPIC per Position Independent Code) 
gcc -c -fPIC -o course.o course.c
gcc -c -fPIC -o student.o student.c

# Crea la libreria dinamica 
gcc -shared -o libstarwars.so course.o student.o 

# Compila il main
gcc -c -o starwars.o starwars.c

# Compila l'eseguibile linkando dinamicamente 
gcc -o dynamic_starwars starwars.o -L. -lstarwars

# Esegui (specificando dove trovare la libreria dinamica)
LD_LIBRARY_PATH=. ./dynamic_starwars
```


## Makefile

esempio di makefile qua sotto
```make
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

ma a scrivere a mano tutte le volte è una rottura =>

```make
starwars: libstarwars.so starwars.o
	gcc -o $@ starwars.o -L. -lstarwars

libstarwars.so: course.o student.o
    ls -shared -o $@ $^

%.o: %.c
	gcc -c -o $^ $@
```


poi puoi fare anche la pulizia sempre nel file make

```make
clean:
	rm .....
```

Ecco la versione completa che ci ha fornito Corti:
```make
CC=/usr/bin/gcc
LD=/usr/bin/ld
RMF=/usr/bin/rm -f 

starwars: starwars.o libstarwars.so 
	$(CC) -o $@ $< -L. -lstarwars 
	
libstarwars.so: student.o course.o 
	$(LD) -shared -o $@ $^ 

%.o: %.c 
	$(CC) -c -o $@ $^

package: starwars 
	# impacchetta il bundle per la distribuzione 
	# binario + libreria .so 
	tar zcvf starwars.tar.gz starwars libstarwars.so 
	
test: 
	# esegue i test 

clean: 
	# pulisce tutto 
	$(RMF) starwars libstarwars.so starwars.o course.o student.o
```
