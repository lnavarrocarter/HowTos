Elementary OS
=============

Mis notas y tutoriales para Elementary OS.

[[TOC]]

## Crear una Entrada de Menú Contextual para Ejecutar Archivos *.desktop

Debido a políticas de seguridad, Elementary OS Loki ha desactivado la
ejecución por defecto de archivos *.desktop. Sin embargo, existe una forma
de ejecutarlos por medio de una entrada de menú contextual.

Lo primero que necesitamos es pcmanfm para poder ejecutar el archivo.
Podemos instalarlo fácilmente con `sudo apt install pcmanfm`.

Las entradas de menú contextual se guardan en la carpeta `contractor`,
localizada en `~/.local/share`. Sin embargo, ésta no está creada por
defecto. Para crearla basta un simple `mdkir ~/.local/share/contractor`.
Vamos al directorio con `cd ~/.local/share/contractor` y creamos un nuevo
archivo `nano run_desktop_file.contract` para guardar nuestras entradas de
menú contextual. Pegamos lo siguiente en el archivo:

```
[Contractor Entry]
Name=Run the Desktop File
MimeType=application/x-desktop
Exec=pcmanfm %f
```

Luego, si hacemos click derecho en cualquier archivo *.desktop, debería
aparecer la entrada para ejecutarlo.

### Editando Contractors Existentes en Elementary

Cambiar el videobin contract por uno de [YouTube uploader](https://github.com/tokland/youtube-upload).

Además, se pueden crear entradas de "Ejecutar" y "Hacer ejecutable" [aquí](https://unix.stackexchange.com/questions/192158/how-to-add-make-executable-and-run-entries-to-elementary-os-file-manager-con).

Una cosa extra para hacer es añadir Mount ISO, como dice [aquí](https://elementaryos.stackexchange.com/questions/264/contract-file-to-mount-iso).