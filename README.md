Integrantes: Guzman Ayelen, dni: 44606586.
Archivos: "P2" es la maquina, "subrutinas" el programa con 3 niveles que suma tres numeros y "SumaRPila" es una subrutina recursiva que suma dos numeros.(Explicacion en el programa)

Maquina creada con CPUSim version 4.0.11

Le damos un tamaño de arquitectura en 16bits por lo tanto voy a tener:
OPCODE de 4 bits. (identificador de operacion)
Registros de 16 bits. (A y B para propositos generales, MDR para trabajar con datos. IR de registro de instruccion)
Direcciones 10 bits. (MAR, PC, BP, SP) ;se utilizo 10 bits para mantener la estructura hecha en P1
FLAGS 3 bits(CO = carryOut, HALT bandera del Stop y OF = overflow)
Memoria RAM con 256 direcciones de 8 bits c/u.


Cuenta con 16 instrucciones por tener 4 bits de opcode y son:
Stop: detiene la ejecucion.
In: ingresa un valor(entero) por teclado.
Out: Muestra lo que contiene el registro A o B.
Load: dado un registro, un valor/direccion y un tipo(inmediato o direcro) carga en el registro A o B. ej Load A 5 I
Save: dado un registro y una direccion, guardo lo que contiene el registro en la direccion. ej: Save A tmp
Sub: dado un registro y un valor, se lo resto al registro. (es inmediato y no utilizo restar el valor de una direccion dada) ej: Sub A 1 es A = A - 1
jump: cambia el PC(program counter) a una direccion dada. Sin condiciones salta si o si.
jumpZ: cambia el PC(program counter) a una direccion dada. Solo lo cambia si el registro A es cero.
Move: toma dos registros y copia lo que contiene el segundo en el primero. Cada registro tiene 2 bits porque tiene en cuenta el Move BP SP (el fild REG2 tiene valor 0 para A, 1 para B y 3 para BP.Necesito dos bits)
Call: toma una direccion y cambia el PC a esa direccion. Antes guarda en la pila la direccion anterior que tenia el PC (lo pone en el tope SP) ej: Call SUB1
Ret: toma el tope de la pila (Call anteriormente guardo el PC que debo retornar) y lo pone en el PC. Tambien desapila el tope ej: despues de Call SUB1 tengo un Ret (en la subrutina) para continuar en la linea despues de Call
Pop: dado un registro, saca un elemento de la pila y a este lo guarda en el registro dado. ej Pop BP 
Push: Agrega un elemento a la pila, toma un registro. ej Push A (pone A como parametro)
Add: Suma un valor inmediato a un registro (A, B o SP). El tipo es siempre inmediato porque no hay espacio suficiente.Tengo 4 bits de opcode, 2 de registro y 10 de valor. 16 bits no alcanzaban para poner tipo(dos bits)
LoadStack: toma un registro(A o B) donde guardar el valor leído desde la Pila y un valor inmediato de desplazamiento (positivo o negativo) para poder indicar a qué posición de memoria tomando como referencia el BP.
SaveStack: toma un registro(A o B) que contiene el valor que va almacenar en la pila y un valor inmediato que indica a que posicion de memoria teniendo en cuenta el BP.(guardo variables locales) 

ejemplo Teniendo 2 parametros 
Push A con 7 y Push B con 4
LoadStack A 4 ; pongo en A el segundo parametro 4
LoadStack B 6 ; pongo en B el  primer parametro 7
In B ; supongamos que 9
SaveStack B -2 ; dejo en una variable local la entrada de B
Mi pila quedaria 
[9 la entrada de B - local] ;BP + -2
[ Base Pointer]
[Direccion de retorno] ;BP + 2
[4 del segundo parametro push B] ;BP + 4
[7 del primer parametro push A] ;BP + 6
