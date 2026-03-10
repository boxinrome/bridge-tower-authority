# D-защелка
D-триггер-защелка состоит из:
1. Входа данных D
2. Входа тактового сигнала clk

D определяет следующее состояние, а clk - когда оно меняется.

<p align="center">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/D-latch.png" alt="D-защелка"><br>
  <em>Схема D-защелки</em>
</p>\
'''
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
)

endmodule
'''
# RS-триггер
Рассмотим сначала cхему и таблицу истинности RS-триггера:
<p>
<div class = "image-box">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/СхемаRS.png" alt="RS-триггер" width = "400" length = "400">
  <img src="https://github.com/boxinrome/bridge-tower-authority/blob/main/RISC-V/imgs/Rs_table.png" alt="Таблица истинности RS-триггера" width = "400" length = "400">
</div>
</p>

Из схемы D-защелки видно, что когда clk = 0 , то на обоих выходах R и S будет ноль 
Если clk = 1, то на одном и будет ноль, а а другом - 1, и это определяется из  


module rs_latch#(
    input r,
    input s,
    output q,
    output q_neg

);
    assign q = ~ (r | q_n);
    assign q_n = ~ ( s | q);
    
endmodule
