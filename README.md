# pricing-exotic-derivatives
Exploring various models, including neural nets, for predicting the prices of options

## Data
I researched for ways that I could acquire data on European Basket options, but I could not find any free resources. I did find a tutorial that showed how to scrape data from Yahoo Finance, but it looked like it would take too long to setup, and the data inputs did not match what I needed for Black-Scholes. I also came across a paid data source in my research called wrds from the Wharton Business School, and I would like to explore using it in the future:

```
https://wrds-www.wharton.upenn.edu/
```

I referenced a finance textbook ("Investments" by Bodie, Kane, and Marcus) to better understand the Black-Scholes formula. From that, I learned about the inputs needed (stock price, exercise price, interest rate, time to expiration, and standard deviation), and I also learned about the equations. I decided to create dummy/toy data so that I would be able to execute as much of the assignment as possible. I picked random numbers from ranges for the stock price and exercise price, and I picked constants for interest rate, time to expiration, and standard deviation. I took these values and created a (1000, 5) dataframe. 

I do realize that this setup is not really a "basket." I wanted to use NDX because I that fits the requirements for "European Basket options", but it wasn't clear to me that I would have the inputs that I needed for Black-Scholes. I would love to figure out how to use this data:
```
https://finance.yahoo.com/quote/%5ENDX/options?p=%5ENDX&.tsrc=fin-srch
```

After creating the initial dataframe, I defined the functions needed for the Black-Scholes pricing model. 

![image](https://user-images.githubusercontent.com/39508404/145350293-a0cead28-2417-4882-a080-3b1ebea42d14.png)

I used the Black-Scholes function to calculate the call values, which I then added to the dataframe. 

<img width="743" alt="Screen Shot 2021-12-09 at 1 24 13 AM" src="https://user-images.githubusercontent.com/39508404/145352222-a2800a1b-6e5a-45d3-9564-e5d767fe34f8.png">


Then I normalized the values for the stock price and exercise price and performed a 80/10/10 split for the train/test/val data sets. I also reshaped the datasets to work with the LSTM model (see appendix). 

## Models
I tried various models on the data including a simple neural net, binary option model, monte carlo, and LSTM. Out of all of them, I got the neural network and the LSTM to train, but I got errors when trying the rest of them. The neural network appears to be predicting prices very well, but it appears to be too accurate, which makes me believe that I may have made an error somewhere!

![image](https://user-images.githubusercontent.com/39508404/145353611-06712ce9-5bd1-45b4-920b-bcb3ff3909fb.png)

The neural network had an average training loss of 0.036 and an average validation loss of 0.037. Those are well below 1.0 which is great, but the validation loss was quite volatile!

![image](https://user-images.githubusercontent.com/39508404/145349569-e5c8a04f-5fb9-4718-8032-1529c7c0d33e.png)

I wanted to try the LSTM model, which is a model that has feedback connections. I thought this would work vell for sequenced data, and I am also interested in the fact that the cells in the model remember values over arbitrary time/sequence interals. LSTMs are known to work well in situations where there can be lags of uknown duration between important events in a sequence/time series. 

![image](https://user-images.githubusercontent.com/39508404/145355192-0d8a0c40-80f6-4180-a2b1-eaf3778d7133.png)

The LSTM model also has a nicely downward sloping loss graph, but the loss values were still way too high. I'm sure if I trained for longer (500+ epochs) and also played with the learning rate, I might have been able to bring the losses down. 

![image](https://user-images.githubusercontent.com/39508404/145350087-61fe9536-2a37-48e3-a8c5-eac47cb3de5f.png)

The LSTM model predictions do not appear to stray too much from the true test target values.

![image](https://user-images.githubusercontent.com/39508404/145353722-bd3fe283-11be-4a13-b5b4-526df0408317.png)

## Notes
I was not quite sure what was meant by "analytical solution" in the assignment. I believe that, since I didn't really use a basket of options, I was probably calculating the options price analytically for the 1 asset case. 

I did conduct unit tests for functions whenever I could. Another way that I test is to use various methods (shape, len) and slicing to visualize the data.

I did not get a chance to explore how to add the Heston stochastic volatility to the pricing model. I would like to understand where in the neural network architecture this can be implemented.

I sourced code from various tutorials, but I did often modify/adapt code to my needs. 
