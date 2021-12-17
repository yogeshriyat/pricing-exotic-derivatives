# pricing-exotic-derivatives
Exploring various models, including neural nets, for predicting the prices of options

## Data
I referenced a finance textbook ("Investments" by Bodie, Kane, and Marcus) to better understand the Black-Scholes formula. From that, I learned about the inputs needed (stock price, exercise price, interest rate, time to expiration, and standard deviation), and I also learned about the equations. I decided to create dummy/toy data so that I would be able to execute as much of the assignment as possible. I picked random numbers from ranges for the stock price and exercise price, and I picked constants for interest rate, time to expiration, and standard deviation. I took these values and created a (1000, 5) dataframe. 

After creating the initial dataframe, I defined the functions needed for the Black-Scholes pricing model. 

![image](https://user-images.githubusercontent.com/39508404/145350293-a0cead28-2417-4882-a080-3b1ebea42d14.png)

I used the Black-Scholes function to calculate the call values, which I then added to the dataframe. 

<img width="743" alt="Screen Shot 2021-12-09 at 1 24 13 AM" src="https://user-images.githubusercontent.com/39508404/145352222-a2800a1b-6e5a-45d3-9564-e5d767fe34f8.png">

Then I normalized the values for the stock price and exercise price and performed a 80/10/10 split for the train/test/val data sets. I also reshaped the datasets to work with the LSTM model (see appendix). 

## Models
I trained a simple neural net and an LSTM model.

![image](https://user-images.githubusercontent.com/39508404/145353611-06712ce9-5bd1-45b4-920b-bcb3ff3909fb.png)

The neural network had an average training loss of 0.036 and an average validation loss of 0.037. Those are well below 1.0 which is great, but the validation loss was quite volatile!

![image](https://user-images.githubusercontent.com/39508404/145349569-e5c8a04f-5fb9-4718-8032-1529c7c0d33e.png)

I wanted to try the LSTM model, which is a model that has feedback connections. I thought this would work vell for sequenced data, and I am also interested in the fact that the cells in the model remember values over arbitrary time/sequence interals. LSTMs are known to work well in situations where there can be lags of unknown duration between important events in a sequence/time series. 

![image](https://user-images.githubusercontent.com/39508404/145355192-0d8a0c40-80f6-4180-a2b1-eaf3778d7133.png)

The LSTM model also has a nicely downward sloping loss graph, but the loss values were still way too high. I'm sure if I trained for longer (500+ epochs) and also played with the learning rate, I might have been able to bring the losses down. 

![image](https://user-images.githubusercontent.com/39508404/145350087-61fe9536-2a37-48e3-a8c5-eac47cb3de5f.png)

The LSTM model predictions do not appear to stray too much from the true test target values.

![image](https://user-images.githubusercontent.com/39508404/145353722-bd3fe283-11be-4a13-b5b4-526df0408317.png)
