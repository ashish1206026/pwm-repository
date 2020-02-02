module dpwm(d,clk,out);
input [7:0]d;
input clk;
output reg out;
wire reset,set;
reg [31:0]Q;
reg [2:0]cnt;
wire mux_output;
wire comp1_output;
wire comp2_output;

mux32to1 mux(Q[31:0],d[4:0],mux_output);
comparator first (cnt,d[7:5],comp1_output);
assign reset=mux_output & comp1_output;

comparator second(cnt,3'b000,comp2_output);
assign set=comp2_output & Q[0];

always@(posedge clk)
	begin
		if(Q==32'b00000000000000000000000000000000)
			Q=32'b00000000000000000000000000000001;
		else
			begin
				if(Q[31]==1)
					Q=32'b00000000000000000000000000000001;
				else
					Q=Q<<1;
				if(Q[0]==1)
					cnt=cnt+1;
			end
		if(reset)
			out=0;
		else 
			begin 
				if(set)
					out=1;
				else
					out=out;
			end
	end
		
endmodule

module mux32to1(W,S,F);
input [31:0]W;
input [4:0]S;
output reg F;

always @(W,S)
	begin
		case(S)
			0: F=W[0];
			1: F=W[1];
			2: F=W[2];
			3: F=W[3];
			4: F=W[4];
			5: F=W[5];
			6: F=W[6];
			7: F=W[7];
			8: F=W[8];
			9: F=W[9];
			10: F=W[10];
			11: F=W[11];
			12: F=W[12];
			13: F=W[13];
			14: F=W[14];
			15: F=W[15];
			16: F=W[16];
			17: F=W[17];
			18: F=W[18];
			19: F=W[19];
			20: F=W[20];
			21: F=W[21];
			22: F=W[22];
			23: F=W[23];
			24: F=W[24];
			25: F=W[25];
			26: F=W[26];
			27: F=W[27];
			28: F=W[28];
			29: F=W[29];
			30: F=W[30];
			31: F=W[31];
	
	endcase
end

endmodule

module comparator(A,B,AeqB);
input [2:0]A,B;
output reg AeqB;

always @(A,B)
	begin
		AeqB=0;
		if (A==B)
			AeqB=1;		
	end

endmodule