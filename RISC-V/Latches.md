# RS-триггер
1) Рассмотим сначала cхему и таблицу истинности RS-триггера:
<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/СхемаRS.png" alt="RS-защелка"><br>
  <em>Схема RS-защелки</em>
</p>
<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/Rs_table.png" alt="Таблица истинности RS-защелка"><br>
  <em>Таблица истинности RS-защелки</em>
</p>

- Когда поступает команда сброса R=1, выход Q принимает значение 0, а выход Q¯ – противоположное
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
    assign q = ~ (r | q_neg);
    assign q_neg = ~ ( s | q);
    
endmodule
```
<p>
<div class = "image-box">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/rs_time.png" alt="RS-триггер"><br>
  <em>Временная диаграмма RS-защелки</em>
</div>
</p>



# D-защелка
Состоит из :
- Входа данных D
- Входа тактового сигнала clk
  
1. Тактовый сигнал контролирует, когда данные проходят через триггер-защелку. 
- Когда CLK=1, защелка «прозрачна», т.е. она пропускает данные D на выход Q, как если бы он являлся обычным буфером.
- Когда CLK=0, защелка «непрозрачна», она не пропускает новые данные с входа D на выход Q,
а Q сохраняет свое значение. D-защелку иногда называют прозрачным триггером или триггером, синхронизируемым уровнем.
D-триггер-защелка состоит из:


D определяет следующее состояние, а clk - когда оно меняется.

<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/D-latch.png" alt="D-защелка"><br>
  <em>Схема D-защелки</em>
</p>

<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/D_table.png" alt="D-защелка"><br>
  <em>Таблица истинности D-защелки</em>
</p>
2. Код для D-защелки :

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
<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/d_time.png" alt="D-защелка"><br>
  <em>Временная диаграмма D-защелки</em>
</p>


# Cтробируемая RS-защелка
Имеется еще оди вход по сравнению c RS-защелкой. Когда на E логическая единица, то ведется себя как обычная защелка, а если ноль - игнорирует сигнал и сохраняет свое состояние. Это необходимо, чтобы управлять моментами, когда защелка будет реагировать на входы R и S.
<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/grs_table.png" alt="D-защелка"><br>
  <em>Схема стробируемой RS-защелки</em>
</p>

```verilog
module gated_sr_latch (
    input  logic E,
    input  logic s,
    input  logic r,
    output logic q,
    output logic q_neg
);
    logic s_gated, r_gated;
    assign s_gated = s & E;
    assign r_gated = r & E;

    assign q  = ~(r_gated | q_neg);
    assign q_neg = ~(s_gated | q);

endmodule
```

<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/grs_time.png" alt="D-защелка"><br>
  <em>Временная диаграмма RS-защелки</em>
</p>
