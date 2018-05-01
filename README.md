# MIPS Logisim

## Programa de teste


Cada diretório contém arquivos de memória destinados aos respectivos processadores. 
MEM_Instruction deve ser carregado na memória de instruções e MEM_Data na de dados (com exceção do MIPS Monociclo, que possui uma memória para ambos).
Além disso, a versão do processador com pipeline inclui o arquivo de ROM de controle, pois a instrução J não está implementada na versão original.

O programa em assembly é o seguinte:



8c010000 8c020004 00221820 00402009 00621822 10610005 AC000008 08000000 ac030008 08000000
00804009 ac030008 00232824 AC05000c 04A10003 00221820 00221820 ac030010 ac050010 8c05001C
ac030014 04010001 ac000014 90030020 ac030018

LW R1, 0(R0)        8c010000 
LW R2, 4(R0)        8c020004
ADD R3, R1, R2      00221820
JARL R4, R2         00402009
SUB R3, R3, R2      00621822
BEQ R3, R1 LB3      10610005      
SW R0, 8(R0)        AC000008
J 0                 08000000
SW R0, 8(R0)        ac030008
J 0                 08000000
LB1:               
JARL R4, R4         00804009         
LB3:
SW R3, 8(R0)        ac030008        
AND R3,R1,R5        00232824
SW R5,12(R0)        AC05000c
BEGZ R5, LB2        04A10003      
ADD R3, R1, R2      00221820
ADD R3, R1, R2      00221820
SW  R3,16(R0)       ac030010
LB2:
SW  R5,16(R0)       ac050010
LW R5, 28(R0)       8c05001C
BEGZ R5, LB4        04A10002
SW R3, 20(R0)       ac030014
BEQ R0, LB5         04010001
LB4:
SW  R0,20(R0)       ac000014
LB5:
LBU R3, 32(R0)      90030020
SW R3, 24(R0)       ac030018

programa em hexa: 

8c010000 8c020004 00221820 00402009 00621822 10610005 AC000008 08000000 ac030008 08000000
00804009 ac030008 00232824 AC05000c 04A10003 00221820 00221820 ac030010 ac050010 8c05001C
ac030014 04010001 ac000014 90030020 ac030018



Ele carrega as duas primeiras posições da memória de dados (no multiciclo, carrega os endereços 40 e 44) em R1 e R2, 
soma esses registradores e armazena em R3, faz a operação de JARL para outro JARL que por sua vez posiciona o PC na 
próxima intrução. Em seguida, subtrai R3 de R2 e armazena em R3 e compara R3 com R1. Se os registradores forem iguais 
(deu tudo certo), pula para LB3 e armazena R3 no endereço 8 (48 no multiciclo), se não, armazena R0 em 8.
Caso a primeira etapa seja concluída com sucesso, o programa fará AND entre os registradores R1 e R2 e armazenará o resultado
em R5, e salva o R5 na quarta posição de memória. Em seguida, testa se R5 >= 0, caso positivo (deu certo) pula para LB2
onde é armazenado o R5 na quinta posição de memória. Carrega em R5 o valor na oitava posição da memória (número negativo)
e testa se R5 >= 0, (não pula - deu certo) armazena na sexta posição da memória o R5. Carrega em R3 os últimos 8 bits da nona 
posição memória e grava o valor na sétima posição da memória de dados.


O objetivo do programa é testar as operações básicas que já estão implementadas no processador juntamente com as instruções implementadas pelo grupo. 
