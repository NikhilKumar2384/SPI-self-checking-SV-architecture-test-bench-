module master(mosi,miso,start,mode,done,cs,sclk,data_tx,data_rx,reset,clk);
  input  logic miso,clk,start,reset;
  output logic mosi,done,cs,sclk;
  input logic [1:0]mode;
  input   [7:0]data_tx;
  output logic [7:0]data_rx;
  logic cpol, cpha;
  logic [7:0]data_inrx;
  
  
   assign cpol=mode[1];
  assign  cpha = mode[0];
  
  logic negedgesclk,posedgesclk;
  
  
  logic last_sclk;
  typedef enum reg [1:0]{
   idle= 2'b00,
   transfer =2'b01,
   finish= 2'b10,
   retrn =2'b11
                          }state_t;
  state_t state = idle;
  
  logic [4:0]clk_count;
  logic [3:0]bit_count_tx;
  logic [3:0]bit_count_rx;
  reg [8:0]data_intx;
  
  
   
             assign negedgesclk=(!sclk && last_sclk);
            assign  posedgesclk=(sclk && !last_sclk);
              
  assign data_intx={1'b0 , data_tx};
  
  always@(posedge clk or posedge reset)
      begin
        
        if (reset)begin
          cs<=1;
         
          data_rx<=0;
         
          done<=0;
         
          mosi<=0;
        
          state<=idle;
          sclk<=cpol;
          clk_count <=0;
          bit_count_tx<=0;
          bit_count_rx<=0;
          
        end
        else begin
          case(state)
            
            
            idle: begin 
               sclk<=mode[1] ;
               
              if (start)
                begin
                  cs<=0;
               
                  state<=transfer;
              
                  
                end
            end
            
            
            
            transfer: begin
              
              last_sclk=sclk;
              
             if (clk_count == 25) begin
                sclk <= ~sclk;
   				 clk_count <= 0;
  					end 
              else begin
  				  clk_count <= clk_count + 1;
			  end
             
              
              
              
              
              if (posedgesclk)
                 begin 
                   if (cpha)begin
                     if (bit_count_rx<9)begin
                       data_inrx[bit_count_rx-1]<=miso;
        					  bit_count_rx<=bit_count_rx+1;
       										 end
        
        
            					  end
     				 else 
       					 begin
                           if (bit_count_tx<9)begin
          					 	mosi<=data_intx[bit_count_tx];
         					 	bit_count_tx<=bit_count_tx+1;
                             
//                                 if(bit_count_tx==9)begin mosi<=0; end
                            
         					 end
      					 end
  			  end
              
              if (negedgesclk) begin 
     							 if (!cpha)begin
                                   if (bit_count_rx<9)begin
                                     data_inrx[bit_count_rx-1]<=miso;
       									   bit_count_rx<=bit_count_rx+1;
                                   
       								  end
        
        
              end
      else 
        begin
          if (bit_count_tx<9)begin
            mosi<=data_intx[bit_count_tx];
            bit_count_tx<=bit_count_tx+1;
//             if(bit_count_tx==9)begin mosi<=0; end
                                   
            
          end
        end
      
    end
              
              
              
              if (bit_count_rx>8 && bit_count_tx>8)begin
               state<=finish;
                bit_count_rx<=0; 
                bit_count_tx<=0;
                data_rx<=data_inrx;
                mosi<=0;
            
              end
              
             
              
           
            
            end
            
            
            finish : begin
              
              sclk<=cpol;
             
              
              done<=1;
             state<=retrn;  
            end
            
            
            retrn: begin
              if (done)begin
              done<=0; cs<=1;
             
              state<=idle;
              end
            end
            
          endcase 
        end
      end
  
  

  
  
endmodule
