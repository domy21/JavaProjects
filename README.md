package com.test;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.text.NumberFormat;
import java.util.Currency;
import java.util.Random;

public class BangTest {
	public static void main(String[] args) {
		BangTest test = new BangTest();
//		test.getTradeResult();
//		System.out.println("=================== compund x2");
//		test.test1(false);
		System.out.println("=================== compund x3");
		test.test2(true);
		
//		long initFund = 1000;
//		int multiplier = Integer.parseInt((2040/initFund)+"");
//		System.out.println(multiplier);
	}
	
	public void test1(boolean printData) {
		long fund = (long) 100.00;
		long initFund = fund;
		boolean sp1 = false;
		int reward = 2;
		int win = 0,loss = 0;
		int tradeCount = 500;
		
		
		for(int i=0; i<=tradeCount; i++) {
//			int multiplier = Integer.parseInt(fund+"");
			int multiplier = Integer.parseInt(( (fund/initFund) < 1 ? 1 : (fund/initFund)  )+"");
			
			long sp1TradeAmt = (long) ((initFund*multiplier) * 0.01);
			long sp2TradeAmt = (long) ((initFund*multiplier) * 0.01 * reward);
			
//			long sp1TradeAmt = (long) ((initFund*multiplier) * 0.001);
//			long sp2TradeAmt = (long) ((initFund*multiplier) * 0.001 * reward);
			
			
			if(getTradeResult()) {
				long winAmt = 0l;
				if(sp1) {
//					System.out.println("profit2 + "+sp2TradeAmt);
					fund = fund + sp2TradeAmt;
					winAmt = sp2TradeAmt;
					sp1=false;
					
				}else {
//					System.out.println("profit1 + "+sp1TradeAmt);
					fund = fund + sp1TradeAmt;
					winAmt = sp1TradeAmt;
					sp1=true;
					
				}
				win++;
				if(printData)
					System.out.println("profit! fund = "+getCurrencyVal(fund, "USD")+ ", win amount: ("+getCurrencyVal(winAmt, "USD") +")");
			}else {
//				System.out.println("loss");
				long lossAmt = 0l;
				if(sp1) {
					//loss sp2
					fund -= sp2TradeAmt;
					lossAmt = sp2TradeAmt;
				}else {
					//loss sp1
					fund -= sp1TradeAmt;
					lossAmt = sp1TradeAmt;
				}
				loss ++;
				if(printData)
					System.out.println("loss! fund = "+getCurrencyVal(fund, "USD") + 
							", loss amount: (-"+getCurrencyVal(lossAmt, "USD") +")");
				
				sp1=false;
			
			}
		}
		
		System.out.println("\nInitial fund: "+initFund);
		System.out.println("Total trades: "+tradeCount);
		System.out.println("Total win trades: "+win +". Equal to "+getPercentage(win,tradeCount) +"%");
		System.out.println("Total loss trades: "+loss+". Equal to "+getPercentage(loss,tradeCount) +"%");
		System.out.println("\nFinal Fund : US"+getCurrencyVal(fund, "USD"));
		System.out.println("Final Fund : PHP"+getCurrencyVal((fund*55),"PHP"));
		
		System.out.println("\nProfit : US "+getCurrencyVal(fund-initFund, "USD"));
		System.out.println("In Peso : PHP "+getCurrencyVal((fund-initFund)*55,"PHP"));
	
	}
	
	public void test2(boolean printData) {
		long fund = (long) 1000.00;
		long initFund = fund;
		boolean sp1 = false;
		boolean sp2 = false;
		int reward = 1;
		int win = 0,loss = 0;
		int tradeCount = 500;
		
		
		for(int i=0; i<=tradeCount; i++) {
			int multiplier = Integer.parseInt(( (fund/initFund) < 1 ? 1 : (fund/initFund)  )+"");
			
//			long sp1TradeAmt = (long) ((initFund*multiplier) * 0.01);
//			long sp2TradeAmt = (long) ((initFund*multiplier) * 0.01 * (reward*2));
//			long sp3TradeAmt = (long) ((initFund*multiplier) * 0.01 * (reward*3));
			
			long sp1TradeAmt = (long) ((initFund*multiplier) * 0.025);
			long sp2TradeAmt = (long) ((initFund*multiplier) * 0.025 * (reward*2));
			long sp3TradeAmt = (long) ((initFund*multiplier) * 0.025 * (reward*3));
			
			
			
			if(getTradeResult()) {
				long winAmt = 0l;
				if(sp2) {
//					System.out.println("profit3 + "+sp3TradeAmt);
					fund = fund + sp3TradeAmt;
					winAmt = sp3TradeAmt;
					sp1=false;
					sp2=false;
					
				}else if(sp1) {
//					System.out.println("profit2 + "+sp2TradeAmt);
					fund = fund + sp2TradeAmt;
					winAmt = sp2TradeAmt;
					sp2=true;
					
				}else {
//					System.out.println("profit1 + "+sp1TradeAmt);
					fund = fund + sp1TradeAmt;
					winAmt = sp1TradeAmt;
					sp1=true;
					
				}
				win++;
				if(printData)
					System.out.println("trade#"+(i+1)+": profit! fund = "+getCurrencyVal(fund, "USD")+ ", win amount: ("+getCurrencyVal(winAmt, "USD") +")");
			}else {
//				System.out.println("loss");
				long lossAmt = 0l;
				if(sp2) {
					//loss sp3
					fund -= sp3TradeAmt;
					lossAmt = sp3TradeAmt;
				}else if(sp1) {
					//loss sp2
					fund -= sp2TradeAmt;
					lossAmt = sp2TradeAmt;
				}else {
					//loss sp1
					fund -= sp1TradeAmt;
					lossAmt = sp1TradeAmt;
				}
				loss ++;
				if(printData)
					System.out.println("trade#"+(i+1)+": loss! fund = "+getCurrencyVal(fund, "USD") + ", loss amount: (-"+getCurrencyVal(lossAmt, "USD") +")");
				
				sp1=false;
				sp2=false;
			
			}
		}
		
		System.out.println("\nInitial fund: "+initFund);
		System.out.println("Total trades: "+tradeCount);
		System.out.println("Total win trades: "+win +". Equal to "+getPercentage(win,tradeCount) +"% win rate!");
		System.out.println("Total loss trades: "+loss+". Equal to "+getPercentage(loss,tradeCount) +"% lose rate!");
		System.out.println("\nFinal Fund : US"+getCurrencyVal(fund, "USD"));
		System.out.println("Final Fund : PHP"+getCurrencyVal((fund*55),"PHP"));
		
		System.out.println("\nProfit : US "+getCurrencyVal(fund-initFund, "USD"));
		System.out.println("In Peso : PHP "+getCurrencyVal((fund-initFund)*55,"PHP"));
	
	}
	
	public String getCurrencyVal(long val, String ccy) {
		NumberFormat format = NumberFormat.getCurrencyInstance();
		format.setMaximumFractionDigits(0);
		format.setCurrency(Currency.getInstance(ccy));

		return format.format(val);
	}
	
	public String getPercentage(int val, int base) {
		BigDecimal a = new BigDecimal(val);
		BigDecimal b = new BigDecimal(base);
		
		return a.divide(b, 2, RoundingMode.HALF_EVEN).multiply(new BigDecimal(100)).toString();
	}
	
	public boolean getTradeResult() {
		Random random = new Random();
//		int randomNumer = random.nextInt((100-25)+1)+20 ;
		int randomNumer = random.nextInt((100-5)+1) +20 ;  //(+XX) - 50+XX %
//		System.out.println(randomNumer);
		return randomNumer>50;
	}
}
