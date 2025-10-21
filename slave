
module slave(miso,mosi,sclk,cs,data_tx_sl,data_rx_sl,reset,mode,done_sl,clk);
  input mosi,sclk,cs,reset,clk;
  input [7:0]data_tx_sl;
  output logic miso,done_sl;
  output logic [7:0]data_rx_sl;
  input [1:0]mode;
  logic cpol,cpha;
  logic [7:0]data_inrx_sl;
  assign cpol = mode[1];
  assign cpha = mode[0];
  
  
  typedef enum logic [1:0] {
    idle     = 2'b00,
    transfer = 2'b01,
    exit     = 2'b10,
    finish   = 2'b11    
  } state_t;
  
  state_t state = idle;
  logic [3:0]bit_count_rx ;
  logic [8:0]data_intx_sl;
  logic [3:0]bit_count_tx;
  logic negedgesclk,posedgesclk;
  reg last_sclk=mode[1];
  assign data_intx_sl = {1'b0 , data_tx_sl};
  
  
  always @(posedge clk)
  begin
    if (cs) begin
      last_sclk <= mode[1];
      posedgesclk <= 0;
      negedgesclk <= 0;
    end
     begin
      negedgesclk <= (!sclk && last_sclk);
      posedgesclk <= (sclk && !last_sclk);
      last_sclk <= sclk;
    end
  end
  



  always @(posedge reset or posedge clk)
    
  begin
    
   
  
    if (reset) begin
      miso <= 0;
      bit_count_rx <= 0;
      bit_count_tx <= 0;
      done_sl<=0;
    end
    else begin
      case (state)
        idle : begin
          if (!cs) begin 
            state <= transfer;
          end
        end
             
        transfer : begin
          if (posedgesclk) begin 
            if (cpha) begin
              if (bit_count_tx < 9) begin
                miso <= data_intx_sl[bit_count_tx];
                bit_count_tx <= bit_count_tx + 1;
              end
            end
            else begin
              if (bit_count_rx < 9) begin
                data_inrx_sl[bit_count_rx] <= mosi;
                bit_count_rx <= bit_count_rx + 1;
              end
            end
          end
              
          if (negedgesclk) begin 
            if (!cpha) begin
              if (bit_count_tx < 9) begin
                miso <= data_intx_sl[bit_count_tx];
                bit_count_tx <= bit_count_tx + 1;
              end
            end
            else begin
              if (bit_count_rx < 9) begin
                data_inrx_sl[bit_count_rx] <= mosi;
                bit_count_rx <= bit_count_rx + 1;
              end
            end
          end
              
          if (bit_count_rx > 8 && bit_count_tx > 8) begin
            state <= finish;  
            bit_count_rx <= 0; 
            bit_count_tx <= 0;
            data_rx_sl<= data_inrx_sl;
            miso <= 0;
            done_sl <= 1;     
          end
        end
                   
        exit : begin 
          done_sl <= 0;      
          state <= idle;
        end
        
        finish : begin
         
          state <= exit;
        end
      endcase
    end
  end
endmodule
