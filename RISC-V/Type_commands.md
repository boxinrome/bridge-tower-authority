# 1. Команды R-формата
3 операнда и все - адреса регистров. 2 регистра источника, 1 регистр-назначение.

Используется для сложения, вычитания, логических операций и сдвигов.

```verilog
begin
    oppcode = command[6:0];
    rd = command[11:7];
    func3 = command[14:12];
    rs1 = command[19:15];
    rs2 = command[24:20];
    func7 = command[31:25];
    rd_reg = rs1 + rs2; //add rc_reg, rs1, rs2 - сложение
end
```
# 2. Команды I-формата
Такие комманды могут работать с immidiate данными - константами, значение которых содержится непосредственно в инструкции

```verilog
begin
    opcode = command[6:0];
    rd = command[11:7];
    rs1 = command[19:15];
    funct3 = command[14:12];
    imm = command[31:20];
    rd_reg = rs1 << imm[0:4]; // slli rd_reg, rs1, imm - сдвиг влево
end
```
# 3. Команды S-формата
Используются за записи данных из регистра в память

``` verilog
begin
    opcode = command[6:0];
    rs1 = command[19:15];
    rs2 = command[24:20];
    funct3 = command[14:12];
    imm = {command[31:25], command[11:7]};
    mem[rs1 + imm][0:7] = rs2[0:7]; // sb rs2, imm(rs1)
end
```
# 4. Команды B-формата
Такие команды используются для условных переходов 

```verilog
begin
    opcode = command[6:0];
    rs1 = command[19:15];
    rs2 = command[24:20];
    funct3 = command[14:12];
    imm = {command[12], command[7], command[30:25], command[11:8]};

    PC = ( rs1 == rs2) ? PC += imm : PC += 4; // beq rs1, rs2, spot - если r1 == r2 прыгнет на spot, иначе на следующую инструкцию

 end
```
# 5. Команды U-формата
Используются для загрузки 20-битных констант в верхние 20 битов регистров. Можно использовать связку lui и addi, чтобы поместить 32-битное число в комманду, несмотрч на то что часть битов занята кодом самой инструкции.
``` verilog
begin
    opcode = command[6:0];
    rd = command[11:7];
    uimm = command[31:12];

    rd_reg = uimm << 12; // lui rd_reg, uimm
end


```

# 6. Команды J-формата
Такие команды используются для безусловных переходов
``` verilog
begin
    opcode = command[6:0];
    rd = command[11:7];
    uimm = {command[31], command[19:12], command[20], command[30:21]};
    rd = PC + 4;
    PC = PC + uimm; // lui rd, point - прыгает к point и сохраняет адрес возврата
end
```