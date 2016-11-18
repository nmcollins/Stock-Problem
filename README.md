# Stock-Problem

Common interview programming problem.  Given a list of stock prices for a day, find the time and price you would buy and sell at to 
maximize profit.

Output can be seen in Output.pdf

Main Code is in StockProblem.cs

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace StockProblem
{

   

    public partial class StockProblem : Form
    {
        public StockProblem()
        {
            InitializeComponent();
        }

        public class StockPriceTime
        {
            public int price;
            public DateTime time;
        }

        private void get_max_profit(List<StockPriceTime> priceTimes)
        {
            int max=0;
            StockPriceTime buyData = new StockPriceTime();
            StockPriceTime sellData = new StockPriceTime();
            for (int i=0; i<priceTimes.Count;i++)
            {
                for (int j=i+1;j<priceTimes.Count;j++)
                {
                    if (priceTimes[j].price - priceTimes[i].price > max)
                    {
                        buyData.time = priceTimes[i].time;
                        buyData.price = priceTimes[i].price;
                        sellData.time = priceTimes[j].time;
                        sellData.price = priceTimes[j].price;
                        max = priceTimes[j].price - priceTimes[i].price;
                    }
                }

            }
            txtBxProfit.Text = max.ToString("C");
            txtBxPurchasePrice.Text = buyData.price.ToString("C");
            txtBxPurchaseTime.Text = buyData.time.ToString();
            txtBxSellPrice.Text = sellData.price.ToString("C");
            txtBxSellTime.Text = sellData.time.ToString();

           
        }

        private void StockProblem_Load(object sender, EventArgs e)
        {

            List<StockPriceTime> yesterday_time_prices = new List<StockPriceTime>();
           
            int[] stock_prices_yesterday = new int[] { 10, 7, 5, 8, 11, 9 };
            DateTime today = DateTime.Today;
            DateTime yesterday = today.AddDays(-1);
            DateTime openingTime = new DateTime(yesterday.Year, yesterday.Month, yesterday.Day, 9, 30, 0);
            for (int i=0; i<stock_prices_yesterday.Length;i++)
            {
                txtBxStockPrices.Text += openingTime.AddMinutes(i).ToString();
                txtBxStockPrices.Text += "     ";
                txtBxStockPrices.Text += stock_prices_yesterday[i].ToString("C");
                txtBxStockPrices.Text += "\r\n";

                StockPriceTime currentPriceTime = new StockPriceTime();
                currentPriceTime.price = stock_prices_yesterday[i];
                currentPriceTime.time = openingTime.AddMinutes(i);
                yesterday_time_prices.Add(currentPriceTime);

            }


            get_max_profit(yesterday_time_prices);
        }
    }
}

