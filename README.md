Integrantes: Guzman Ayelen, dni: 44606586.
Archivos: "P2" es la maquina, "subrutinas" el programa con 3 niveles que suma tres numeros y "SumaRPila" es una subrutina recursiva que suma dos numeros.(Explicacion en el programa)

Máquina creada con CPUSim versión 4.0.11

Le damos un tamaño de arquitectura en 16 bits, por lo tanto voy a tener:
OPCODE de 4 bits (identifica la operación).
Registros de 16 bits (A y B para propósitos generales, MDR para trabajar con datos, IR de registro de instrucción).
Direcciones de 10 bits (MAR, PC, BP, SP). ;para mantener la estructura de P1
FLAGS de 3 bits (CO = carryOut, HALT bandera del Stop y OF = overflow).
Memoria RAM con 256 direcciones de 8 bits c/u.

Cuenta con 16 instrucciones por tener 4 bits de opcode y son:
Stop: detiene la ejecución.
In: ingresa un valor (entero) por teclado.
Out: muestra lo que contiene el registro A o B.
Load: dado un registro, un valor/dirección y un tipo (inmediato o directo), carga en el registro A o B. ej: Load A 5 I
Save: dado un registro y una dirección, guardo lo que contiene el registro en la dirección. ej: Save A tmp
Sub: dado un registro y un valor, se lo resto al registro (es inmediato y no utilizo restar el valor de una dirección dada). ej: Sub A 1 es A = A - 1
Jump: cambia el PC (program counter) a una dirección dada. Sin condiciones salta sí o sí.
JumpZ: cambia el PC (program counter) a una dirección dada. Solo lo cambia si el registro A es cero.
Move: toma dos registros y copia lo que contiene el segundo en el primero. Cada registro tiene 2 bits porque tiene en cuenta el Move BP SP (el field REG2 tiene valor 0 para A, 1 para B y 3 para BP. Necesito dos bits).
Call: toma una dirección y cambia el PC a esa dirección. Antes guarda en la pila la dirección anterior que tenía el PC (lo pone en el tope SP). ej: Call SUB1
Ret: toma el tope de la pila (Call anteriormente guardó el PC que debo retornar) y lo pone en el PC. También desapila el tope. ej: después de Call SUB1 tengo un Ret (en la subrutina) para continuar en la línea después de Call
Pop: dado un registro, saca un elemento de la pila y a este lo guarda en el registro dado. ej: Pop BP
Push: agrega un elemento a la pila, toma un registro. ej: Push A (pone A como parámetro)
Add: suma un valor inmediato a un registro (A, B o SP). El tipo es siempre inmediato porque no hay espacio suficiente. Tengo 4 bits de opcode, 2 de registro y 10 de valor. 16 bits no alcanzaban para poner tipo (dos bits).
LoadStack: toma un registro (A o B) donde guardar el valor leído desde la pila y un valor inmediato de desplazamiento (positivo o negativo) para poder indicar a qué posición de memoria tomando como referencia el BP.
SaveStack: toma un registro (A o B) que contiene el valor que va a almacenar en la pila y un valor inmediato que indica a qué posición de memoria teniendo en cuenta el BP (guardo variables locales).

Teniendo 2 parámetros 
Push A con 7 y Push B con 4
LoadStack A 4 ; pongo en A el segundo parámetro 4
LoadStack B 6 ; pongo en B el primer parámetro 7
In B ; supongamos que 9
SaveStack B -2 ; dejo en una variable local la entrada de B
Mi pila quedaría
[9 la entrada de B - local] ;BP + -2
[Base Pointer] ;BP + 0
[Dirección de retorno] ;BP + 2
[4 del segundo parámetro push B] ;BP + 4
[7 del primer parámetro push A] ;BP + 6
