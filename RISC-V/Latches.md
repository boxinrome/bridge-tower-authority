# D-защелка
1) В целом

D-триггер-защелка состоит из:
1. Входа данных D
2. Входа тактового сигнала clk

D определяет следующее состояние, а clk - когда оно меняется.

<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/D-latch.png" alt="D-защелка"><br>
  <em>Схема D-защелки</em>
</p>

<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/D_table.png" alt="D-защелка"><br>
  <em>Таблица истинности D-защелки</em>
</p>

2) Код для D-защелки :

``` verilog 
module d_latch (
    input clk,
    input d,
    output q,
    output q_n

);

wire r,s;
assign r = ~d & clk;
assign s = d & clk;

rs_latch rs1 (
    .s(s),
    .r(r),
    .q_n(q_n),
    .q(q)
);

endmodule
```

# RS-триггер
1) В целом
Рассмотим сначала cхему и таблицу истинности RS-триггера:
<p>
<div class = "image-box">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/СхемаRS.png" alt="RS-триггер" width = "400" length = "400">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/Rs_table.png" alt="Таблица истинности RS-триггера" width = "400" length = "400">
</div>
</p>

- Когда поступает команда сброса R=1, выход Q принимает значение 0, а выход Q¯ – противоположное (лог. 1). 
- Когда поступает команда установки бита S=1, выход Q становится единицей,а Q¯ – нулем. Если ни на один из входов не поступает логическая
единица, на обоих выходах сохраняется предыдущее значение Qпред. 
- Подача на входы одновременно R=1 и S=1 не имеет особого смысла, так как это означает, что выход должен быть одновременно и
установлен и сброшен, что невозможно. Защелка, не зная, что ей делать, выставляет как на прямом, так и на инверсном выходе логический 0.

2) Код для RS-триггера :
``` verilog
module rs_latch (
    input r,
    input s,
    output q,
    output q_neg

);
    assign q = ~ (r | q_n);
    assign q_n = ~ ( s | q);
    
endmodule
```
