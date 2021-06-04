Curs d'Administració Linux en RST per ReadTheDocs i Sphinx.

Autor: Joan Francesc Arbona

Llicència del curs: Creative Commons

## Generar documentació

S'ha convertit amb pandoc:

`for N in $(seq 1 14); do pandoc -f latex -t rst Tema$N.tex -o ../../../RstLinuxCourse/Tema$N/Tema$N.rst  --columns 9999; done`

Generar html:

```
make clean
make html
```
