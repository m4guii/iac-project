===============================================================================
PROJETO 3: PROCESSADOR DE 16 BITS
===============================================================================

1. ESPECIFICAÇÃO DO FORMATO DE INSTRUÇÕES (ISA)
----------------------------------------------------------------------
O processador adota instruções de tamanho fixo de 16 bits (Inst[15:0]),
com o campo "DNA" (Opcode) mapeado nos bits de menor ordem Inst[3:0].

Formato Colosso (Instrução: li)
+------------------------+-------------------+-------------------+
| Colosso  (Int) [9b]    | Fates (Rd) [3b]   | DNA (Opcode) [4b] |
+------------------------+-------------------+-------------------+
| Inst[15:7]             | Inst[6:4]         | Inst[3:0]         |
+------------------------+-------------------+-------------------+

Formato Tipo-R2 (Instrução: add)
+------------+------------+-----------------+------------+------------+
| Cc [3b]    | Space [3b] | Passport [3b]   | Fates [3b] | DNA [4b]   |
+------------+------------+-----------------+------------+------------+
| Inst[15:13]| Inst[12:10]| Inst[9:7]       | Inst[6:4]  | Inst[3:0]  |
+------------+------------+-----------------+------------+------------+

Formato Tipo-R3 (Instruções: dot, dota)
+------------+------------+-----------------+------------+------------+
| Back [3b]  | Space [3b] | Passport [3b]   | Fates [3b] | DNA [4b]   |
+------------+------------+-----------------+------------+------------+
| Inst[15:13]| Inst[12:10]| Inst[9:7]       | Inst[6:4]  | Inst[3:0]  |
+------------+------------+-----------------+------------+------------+

2. EXEMPLOS DE CODIFICAÇÃO EM LINGUAGEM MÁQUINA (BINÁRIO)
----------------------------------------------------------------------
Considerando os DNAs por nós definidos:
* li   R2, -5  -> Int=-5 (111111011), Rd=2 (010), DNA=0001
  Binário: 1111 1101 1010 0001 (0xFDA1)
* add  R1, R3  -> cc=000, Rs1=3 (011), Funct3=000, Rd=1 (001), DNA=0010
  Binário: 0000 1100 0001 0010 (0x0C12)
* dot  R0, R2  -> Rs2=3 (011), Rs1=2 (010), Funct3=000, Rd=0 (000), DNA=0011
  Binário: 0110 1000 0000 0011 (0x6803)
* dota R0, R1, R3 -> Rs2=3 (011), Rs1=1 (001), Funct3=001, Rd=0 (000), DNA=0100
  Binário: 0110 0100 1000 0100 (0x6484)

3. JUSTIFICAÇÃO DAS OPÇÕES DE IMPLEMENTAÇÃO
----------------------------------------------------------------------
* Simplicidade do Circuito: O posicionamento fixo do Opcode (DNA) em 
  Inst[3:0] reduz a lógica combinatória de descodificação na Unidade 
  de Controlo.
* Número de Componentes: A reutilização dos campos comuns (fates/Rd) 
  em todos os formatos minimiza o uso de multiplexadores antes do 
  banco de registos, poupando área no datapath do Logisim.
* Expressividade da ISA: O formato Tipo-R3 permite endereçar até três
  registos distintos diretamente na instrução dota, eliminando a 
  necessidade de instruções intermédias de movimentação de dados.
* Extensibilidade: A inclusão dos modificadores "passport" e "cc" 
  reserva espaço lógico livre para expandir a ISA com novas operações 
  aritméticas e lógicas sem alterar a largura fixa de 16 bits.
* Caso o input seja 0000 (memória ROM vazia), o nosso circuito apenas 
  faz overwrite do r0 com o conteúdo dele próprio (nop).

