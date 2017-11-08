1. Escriban qué esperan de cada uno de los pasos

a)`make preprocessing`
crea el file "calculator.pp_c". Se encarga de icluir el #include "calculator.h" en el file

b)`make assembler`
crea el file "calculator.asm". Segun lo que vimos, la idea seria traducir de C a assembler.

c)`make object`
crea el file "calculator.o". Genera objetos en binario para que los vea la maquina.

d)`make executable`
finalmente se crea el file "calculator.e". Este paso hace el linkeo, resolviendo las dependencias de las funciones definidas en files ".c", cosa que despues no sea necesario compilar todo de vuelta si cambio las definiciones de las funciones. Este file no se puede ver como un texto. En este caso en particular no hay funciones que definir o que linkear, pero de todas formas este paso es necesario para llegar al ejecutable.

2. ¿Qué agregó el preprocesador?

La idea es que incluye calculator.h. Este file a su vez llama a "#include <stdio.h>" y declara la funcion "add_numbers". Entonces en el file "calculator.pp_c" se agregan "add_numbers" y "printf"

3. Identificar en la rutina de assembler las funciones

Supongo que son las que callea (o llama). Aparece tipo:

main:
.LFB0:
      ...cosas...
call	add_numbers
call	printf
      ...cosas....    
add_numbers aparece bocha de veces mas.

4. Explicar qué quieren decir los símbolos que se crean en el objeto
000000000000003c T add_numbers
0000000000000000 T main
                 U printf

El numero indica la posicion relativa en la memoria. La T signficia que add_numbers y main son texto y la U signfica que printf no esta definida. Las varaibles estan en mayuscula, eso quiere decir que son visibles de afuera.

5. ¿En qué se diferencian los símbolos del objeto y del ejecutable?

en el .e tengo:
                 w __gmon_start__
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
                 w _Jv_RegisterClasses
                 U __libc_start_main@@GLIBC_2.2.5
                 U printf@@GLIBC_2.2.5
00000000004003e0 T _init
0000000000400440 T _start
0000000000400470 t deregister_tm_clones
00000000004004a0 t register_tm_clones
00000000004004e0 t __do_global_dtors_aux
0000000000400500 t frame_dummy
000000000040052d T main
0000000000400569 T add_numbers
0000000000400580 T __libc_csu_init
00000000004005f0 T __libc_csu_fini
00000000004005f4 T _fini
0000000000400600 R _IO_stdin_used
0000000000400778 r __FRAME_END__
0000000000600e10 t __frame_dummy_init_array_entry
0000000000600e10 t __init_array_start
0000000000600e18 t __do_global_dtors_aux_fini_array_entry
0000000000600e18 t __init_array_end
0000000000600e20 d __JCR_END__
0000000000600e20 d __JCR_LIST__
0000000000600e28 d _DYNAMIC
0000000000601000 d _GLOBAL_OFFSET_TABLE_
0000000000601030 D __data_start
0000000000601030 W data_start
0000000000601038 D __dso_handle
0000000000601040 B __bss_start
0000000000601040 b completed.6973
0000000000601040 D _edata
0000000000601040 D __TMC_END__
0000000000601048 B _end

en particular podemos ver que print sigue siendo .U pero ahora me dice donde buscarlo con el @@. Ademas me agregaron banda de cosas...



